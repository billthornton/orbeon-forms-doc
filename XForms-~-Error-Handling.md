## Rationale

[SINCE Orbeon Forms 4.0]

A number of things can go wrong while the user is interacting with an XForms page. In particular, XPath expressions and actions can raise errors when evaluating and executing.

In XForms 1.1, certain runtime errors, including errors in XPath expressions, must stop the XForms engine, and Orbeon Forms used to implement that behavior. However in many cases this is not desirable, as this prevents the user to attempt to recover from those errors. A user might be able, for example, to save data after an error, but not if the XForms engine has already stopped functioning!

So Orbeon Forms 4.0 implements a new and improved behavior for certain runtime errors. This behavior is described below and is also expected to be part of XForms 2.0.

## Orbeon XForms error handling behavior

The error handling behavior allows recovering from many errors, including XPath errors, binding errors, and errors while running actions.

By default, such errors are logged at WARNING level and then shown to the user in a dialog. The user can then close the dialog and try to continue working with the page. There is no guarantee that further actions on the form will work, but at least the user can try.

![Error dialog](images/xforms-error-dialog.png)

## Errors during page load

Errors can happen while a form is being loaded, or afterwards, as the user interacts with the form. This section covers errors which happen while the form is being loaded.

### With Orbeon Forms 4.0 to 4.5

Errors occurring during the page initialization are not recoverable. They throw an exception and interrupt XForms processing. The idea was that there is not much to recover from, as the user has just landed on the page. The user can attempt to recover from such errors with the browser back button.

Dynamic XPath errors on MIPs are always recoverable via the `xxforms-xpath-error` event.

### With Orbeon Forms 4.6 to 4.8

The behavior is the same as before but fatal errors during initialization can be disabled via the `oxf.xforms.fatal-errors-during-initialization` property.

### With Orbeon Forms 4.9

The `oxf.xforms.fatal-errors-during-initialization` property is removed. This is motivated by the fact that many XPath errors, such as those doing calculations in forms, happen as a matter of course. For example, if you write a formula in Form Builder:

```ruby
$a div $b
```

and the value for `$b`, provided by the user in a field, is `0`, there is an error. But this is not a bug in the form: it is expected, and the calculation simply doesn't produce a result.

So the only types of errors which are now fatal during page load and cause the page to display an unrecoverable error are:

- __Static XPath errors:__ These are typically XPath errors which have syntax errors, which reveal bugs in a form or in Orbeon Forms.
- __Other static errors__: For example, duplicate `id` attributes on elements.

_NOTE: Like before, non-XForms errors typically are not recoverable. This includes errors in XML pipelines, XSLT transformations outside of XForms, and other unexpected Java exceptions in Orbeon Forms._

On the other hand, the following errors do not cause the form to fail loading:

- __Dynamic XPath errors:__ This includes divisions by zero, and other errors which can happen while an XPath expression executes.
- __Errors writing values into the data model__: For example, a `calculate` expression attempting to write to a non-leaf XML element or a an XML document element.
- __XForms actions errors__: In addition to a failed `xf:setvalue`, unexpected errors when running an XForms action.

## Configuration properties

### Error dialog

By default, when the Orbeon Forms JavaScript code intercepts an error, it displays to the user an error dialog.

You can disable this behavior by setting the following property to `false` (its default value is `true`):

```xml
<property
  as="xs:boolean"
  name="oxf.xforms.show-error-dialog"
  value="true"/>
```

If you disable the default error dialog, you might want to provide an alternative way of reporting the issue to the user. You can do this in JavaScript by registering an event listener on [ORBEON.xforms.Events.errorEvent][3].

### Recoverable errors

[SINCE 2011-11-04]

The following property allows you to enable or disable showing recoverable (non-fatal) server errors to the user, and to determine a maximum number of errors to show the user.

```xml
<property
  as="xs:integer"
  name="oxf.xforms.show-recoverable-errors"
  value="10"/>
```

If the value is `0`, no errors are shown the user. If the value is `1` or greater, the value is the maximum number of errors to show the user.

Default:

* `prod` mode: `0`
* `dev` mode: `10`

_NOTE: Before 2012-05-03, the default was _10_._

### Fatal errors during form initialization

[SINCE Orbeon Forms 4.6, UNTIL Orbeon Forms 4.8.x]

The following property controls whether errors occurring during form initialization are considered fatal or not. If `true`, they are fatal and an error shows when loading the page.

```xml
<property
  as="xs:boolean"
  name="oxf.xforms.fatal-errors-during-initialization"
  value="true"/>
```

Default: `true`.

## Noscript mode

In noscript mode, an error panel is also shown for recoverable errors:

![Noscript error panel](images/xforms-noscript-error.png)

The differences with the Ajax mode are:

* the panel cannot be immediately closed (as no JavaScript is required)
* the panel appears at the top of the page instead of as an overlay
* the detail of the errors is immediately shown

## Detailed behavior

### Philosophy of error handling

* Upon initial page load:
    * It is ok to immediately show an error, as the user hasn't yet interacted with the page.
    * In particular, it is ok to immediately report errors detected during static analysis.
    * For dynamic XForms, it is better not to show an error immediately (case of Form Builder and `xxf:dynamic`).
* Upon subsequent interactions:
    * The XForms engine attempts to recover from errors occurring with XPath expressions, bindings, and actions.

### How the XForms engine recovers from errors

1. For control bindings, model bindings, values, variables
    * Upon XPath errors, a default value is chosen:
        * the empty sequence for bindings and variable values
        * the empty string for string values
    * Upon binding errors with the `bind` attribute
        * the binding resolves to the empty sequence
    * Upon binding errors with the `model` attribute
        * the model doesn't change, as if the `model` attribute was missing
1. For XPath MIP values
    * Upon XPath errors for  `calculate` and `xxforms:default` [SINCE 2012-12-06]
        * the destination value is set to blank
        * if `xxforms:expose-xpath-types="true"` and there is an error accessing a typed value
            * the error is logged at debug level
        * else if `xxforms:expose-xpath-types="false"` or there is any other dynamic error
            * `xxforms-xpath-error` is dispatched to the model
    * Upon XPath errors for other MIPs
        * the MIP is not modified, as if the attribute specifying the property was missing
        * `xxforms-xpath-error` is dispatched to the model
    * Upon binding errors with complex or readonly content (`calculate` or `xxforms:default` only)
        * the instance value is not modified
        * `xxforms-binding-error` is dispatched to the model
1. For the submission `instance` and `xxforms:instance` attributes
    * Upon incorrect instance id
        * `xforms-submit-error` is dispatched (as in the case of a target error with the `targetref` attribute)
    * Upon binding errors with complex or readonly content
        * `xforms-submit-error` is dispatched
1. For actions
    * Any error taking place during action processing stops the outermost action handler, including:
        * XPath errors
        * binding errors with the `bind` or `model` attribute
        * binding errors with complex or readonly content
        * missing attributes or unsupported attribute values on action elements
    * `xxforms-action-error` event is dispatched to observer of the action
    * _NOTE: Some actions silently ignore some error conditions, including:_
        * `<setvalue>` pointing to an empty sequence or to an atomic item (such as a string) instead of a node
        * <delete> with an empty sequence or an empty overridden context
        * <insert> with an empty or non-element insert context, an empty overridden context, or an empty origin
        * actions with AVTs evaluating to the empty sequence
    * `<dispatch>`, `<send>`, `<setfocus>`, `<setindex>`, `<toggle>` when the target element is not found

In all cases except typed value access XPath errors on MIPs:

* errors are logged at WARNING level
* errors are added to a list of errors to send to the client
* the client shows an error dialog which the user can discard (see also the configuration properties)

In the case of typed value access XPath errors on MIPs:

* errors are logged at DEBUG level

### Events dispatched

The `xxforms-xpath-error` event is dispatched:

*  to the model, upon encountering an XPath error during processing of an XPath model item property (MIP)

The `xxforms-binding-error` event is dispatched:

* to the model, if a `calculate` or `xxforms:default` MIP points to complex or readonly content
* to the control, if an attempt to store an external value (control value, filename, metadata or size) on a control to a node with complex or readonly content takes place  
_NOTE: This should ideally not occur as the value control binding should not point to complex content, and readonly is disallowed._

The `xxforms-action-error` event is dispatched:

* [SINCE 2012-06-08]
    * to the _observer_ of the action, upon encountering an error during processing of an action
    * this includes: controls, model, instance, and submission
* [PRIOR TO 2012-06-08]
    * to the top-level _document_, upon encountering an error during processing of an action

_NOTE: The fatal, non-cancelable XForms `xforms-compute-exception` and `xforms-binding-exception` are no longer dispatched by Orbeon Forms._

### Reference: kinds of errors that can occur

* XForms errors
    * static XPath errors
        * some are detected during the static analysis phase (PE)
        * some expressions are not analyzed during static analysis of the page, and so can occur at runtime
    * dynamic XPath errors
    * binding errors with the `bind` or `model` attributes, e.g.: `bind="foobar"` where id `foobar` doesn't exist
        * some are detected during the static analysis phase
        * some are not analyzed during static analysis of the page, and so can occur at runtime
    * binding errors with complex or readonly content
        * XForms disallows setting the value of an element with complex content
        * XForms disallows setting the value of a readonly element or attribute
* errors outside of XForms, for example:
    * in the XPL (XML pipeline) engine
    * in the controller
* other server errors:
    * major errors such as the server running out of resources
    * server bugs!

## Future improvements

Static analysis could check _all_ XPath expression contained in the document, including expression in actions.

_NOTE: Currently, CE doesn't analyze XPath expressions, but this is not really analysis: just trying to compile the expressions._

_NOTE: `xxf:dynamic` is a special case, as its "static" errors are considered dynamic ones._

Static analysis could check _all_ bindings via the `bind` or `model` attributes, and the non-AVT attributes referring to ids on actions and submissions.

_Q: Are there any other references by id that can be statically checked?_

## Historical: how things used to work

Until 2011-11-04 builds, including Orbeon Forms 3.8 and 3.9:

Once an error occurred when the user is interacting with the form, the XForms engine displays an error dialog to the user but doesn't allow recovering from that error. The form is basically non-functional after that.

* Most runtime errors are fatal and stop XForms processing.
* In particular, `xforms-compute-exception` and `xforms-binding-exception` can be dispatched and are fatal.
* The user sees an error dialog on the client but typically cannot recover from errors.

[3]: http://wiki.orbeon.com/forms/doc/developer-guide/xforms-extensions#TOC-Custom-Events
[4]: http://wiki.orbeon.com/forms/_/rsrc/1320697168699/welcome/xforms-error-handling/noscript-panel.png
  </toggle></setindex></setfocus></send></dispatch></insert></delete></setvalue></property></property></property>