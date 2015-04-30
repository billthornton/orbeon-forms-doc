> [[Home]] ▸ Contributors ▸ [[Test Plan|Contributors ~ Test Plan]]

- http://localhost:8080/orbeon/fr/ lists deployed forms
- (see also Form Builder permissions above which already tests some of this)
  - comment all roles in form-builder-permissions.xml
- no admin buttons/actions show
- set all Form Builder permissions
  - admin actions show
  - Available/Unavailable/Library labels show
  - publish/unpublish works
- "publish to production"
  - configure  remote server and production-server-uri
    - e.g. remote in Liferay Tomcat

    ```xml
    <property as="xs:string" name="oxf.fr.home.remote-servers">
        [
          {
            "label": "Remote server",
            "url":   "http://Eriks-MacBook-Pro.local:9090/orbeon/"
          }
        ]
    </property>
    ```
    - use orbeon-auth.war on remote
    ```xml
    <property
        as="xs:anyURI"
        processor-name="oxf:page-flow"
        name="authorizer"
        value="/orbeon-auth"/>
    ```
    - set Form Builder permissions
    ```xml
    <role name="*" app="*" form="*"/>
    ```
  - server asks for credentials if user has admin role
    - orbeon-admin/x*
  - Cancel  → loads local forms
  - Connect → loads local and remote forms, sorted by mod date desc
  - Select menu works
  - Operation menu works
    - push/pull forms
  - add 2nd remote server to `oxf.fr.home.remote-servers` property and check user is asked when loading page
  - take form (could be previous `sales/my-sales-form` (see [[Form Builder Permissions|Contributors ~ Test Plan ~ Form Builder Permissions]]) but doesn't have to be)
    - attach static image
    - publish locally
    - push to remote
    - check attachment is pushed
    - load form /new on remote, make sure works and attachment is there
    - *NOTE: `/summary` should do 403 if user is not orbeon-sales on remote!*
    - pull back form
    - load form /new on local, make sure works and attachment is there
  - no checkbox for forms w/o admin access (e.g. set `<role name="*" app="orbeon" form="*"/>`)
- upgrade form definitions
  - upgrade local
  - upgrade remote
  - make sure forms still work