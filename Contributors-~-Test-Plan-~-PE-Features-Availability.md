> [[Home]] ▸ Contributors ▸ [[Test Plan|Contributors ~ Test Plan]]

Check that all PE features are available in PE, but not in CE:

- features
    - [Skip: check not yet in place] test publish from the Form Runner home page is disabled
        - in `form-builder-permissions.xml` add `<role name="orbeon-user" app="*" form="*"/>`
        - in `properties-local.xml` add `<property as="xs:string" name="oxf.fr.authentication.container.roles" value="orbeon-user"/>`
        - in `web.xml` uncomment authentication section
        - access [http://localhost:8080/orbeon/fr/](http://localhost:8080/orbeon/fr/)
        - login with user with the `orbeon-user` role
        - check the page doesn't have any form admin feature
    - all the features listed on the [web site][1]
    - [Skip: check not yet in place] captcha
        - in `properties-local.xml` add
            - `<property as="xs:string" name="oxf.fr.detail.captcha.*.*" value="reCAPTCHA"/>`
            - the properties for the private/public key
        - access [http://localhost:8080/orbeon/fr/orbeon/bookshelf/new](http://localhost:8080/orbeon/fr/orbeon/bookshelf/new)
        - check the captcha isn't shown
    - known issues
        - [#1043](https://github.com/orbeon/orbeon-forms/issues/1043) Disable noscript mode in CE version
        - [#1407][2] Accessible link changes the toolbar with CE builds
        - [#1408][3] PE check not in place for Excel import feature
        - [#1926](https://github.com/orbeon/orbeon-forms/issues/1926) PE check not in place for Publish to Production
        - [#1927](https://github.com/orbeon/orbeon-forms/issues/1927) PE check not place for captcha feature
- in Form Builder, check with CE, that when accessing a PE feature a PE dialog shows

[1]: http://www.orbeon.com/download
[2]: https://github.com/orbeon/orbeon-forms/issues/1407
[3]: https://github.com/orbeon/orbeon-forms/issues/1408