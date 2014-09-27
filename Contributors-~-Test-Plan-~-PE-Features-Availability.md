> [[Home]] ▸ Contributors ▸ [[Test Plan|Contributors ~ Test Plan]]

Check that all PE features are available in PE, but not in CE:

- features
    - test publish from the Form Runner home page is disabled
        - edit `form-builder-permissions.xml`
        - add `<role name="orbeon-user" app="*" form="*"/>`
        - in `web.xml` uncomment authentication section
        - access [http://localhost:8080/orbeon/fr/](http://localhost:8080/orbeon/fr/)
        - login with user with the `orbeon-user` role
        - check the page doesn't have any form admin feature
    - all the features listed on the [web site][1]
    - known issues
        - [#1043](https://github.com/orbeon/orbeon-forms/issues/1043)
        - [#1407][2] Accessible link changes the toolbar with CE builds
        - [#1408][3] PE check not in place for Excel import feature
    - captcha
- in Form Builder, check with CE, that when accessing a PE feature a PE dialog shows

[1]: http://www.orbeon.com/download
[2]: https://github.com/orbeon/orbeon-forms/issues/1407
[3]: https://github.com/orbeon/orbeon-forms/issues/1408