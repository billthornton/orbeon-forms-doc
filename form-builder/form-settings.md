# Form Settings

<!-- toc -->

## Introduction

Some settings apply to the entire form definition. You access these settings with the "Form Settings" wrench icon on the top right of Form Builder. The General Settings also show initially when creating a new form definition.

## General Settings

General settings allow you to set:

- the Application Name
- the Form Name
- the form title in the current language
- the form description in the current language

![](images/form-settings-general.png)

## Form Options

[SINCE Orbeon Forms 2016.2]

Form options include options which apply to the entire form definition.

![](images/form-settings-options.png)

- [Singleton Form](../form-runner/advanced/singleton-form.md) 
- [Wizard View](../form-runner/feature/wizard-view.md)
- Appearance of Control Labels
    - Use Default: use the `oxf.xforms.label.appearance` property
    - Inline: labels show inline above the control
- Use Placeholder for Text Fields: labels show inline above the control, but for text, date, and time input fields only, labels show as an HTML *placeholder* within the field when the field is empty
- Appearance of Control Hints
    - Use Default: use the `oxf.xforms.hint.appearance` property
    - Inline: hints show inline below the control
    - Tooltips: hints show as tooltips upon mouseover
- Use Placeholder for Text Fields: hints show inline or as tooltips upon mouseover, but for input fields only, hints show as an HTML *placeholder* within the field when the field is empty

Here is how hints appear depending on the type of control they are associated with:

![](../form-runner/images/placeholder-and-inline-hints.png)

## Form Statistics

Form statistics show various counts of form elements.

![](images/form-settings-stats.png)