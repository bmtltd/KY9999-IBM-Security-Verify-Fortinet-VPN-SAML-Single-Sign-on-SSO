# Get Started: Configure FortiGate and IBM Security Verify

Within IBM Security Verify we’re going to create an application and set the conditional-access policies that control when users are sent a two-factor authentication challenge. If you’re using LDAP or RADIUS authentication, this will not impact your existing users. This will simply be configured as an additional authentication mechanism. 

## Section #1: Create an application in IBM Security Verify

Log into the IBM Securtiy Verify Admin portal.

1. Create a new application by going to `Applications` > `Applications` then click `Add Application`. 
2. Choose the first option, `Custom Application`. 
3. Under the `Sign-on` tab fill out the following values:
    
    **Provider ID:**
    ```
    https://{FQDN}:{PORT}/remote/saml/metadata/
    ``` 
    **Assertion consumer service URL (HTTP-POST)**
    ```
    https://{FQDN}:{PORT}/remote/saml/login/
    ```
    **Service provider SSO URL**
    ```
    https://{FQDN}:{PORT}/remote/saml/?acs
    ```
    **NameID format**: `Email`

    **Name identifier**: `email`

4. Under **Attribute mappings** add two additional attributes:

    | Attribute name	      | Attribute name format                                      | Attribute source    |
    | ----------------------- | ---------------------------------------------------------- | ------------------  |
    | `Groups`                | `urn:oasis:names:tc:SAML:2.0:attrname-format:unspecified`  | `groupIds`          |
    | `username`              | `urn:oasis:names:tc:SAML:2.0:attrname-format:unspecified`  | `email`             |


5. Click **Save** to create the application. For now, let's allow everyone to use the application. Once we're finished our proof of concept, we can restrict usage via the **Entitlements** page.




## Section #2: Configure Single Sign-on in FortiGate

!!! notes

    Consider upgrading to v7.2.3 (or higher) before proceeding. You can still set up your SSO VPN without upgrading, but you’ll need to run commands via the terminal to configure SSO. We’re going to provide terminal commands to make it easy.

1. Log into the FortiGate appliance Dashboard.
2. Under `System` > `Certificates`, upload the IBM Security Verify certificate used for the Fortinet VPN application you defined earlier. Remember the certificate name in Fortinet (likely REMOTE_Cert_2).
3. In the FortiGate terminal, configure IBM Security Verify identity SSO for Fortinet VPN:
```
config user saml
    edit "fac-sslvpn"
        set entity-id "https://{FQDN}:{PORT}/remote/saml/metadata/"
        set single-sign-on-url "https:// {FQDN}:{PORT}/remote/saml/login/"
        set single-logout-url "https:// {FQDN}:{PORT}/remote/saml/logout/"
        set idp-entity-id "IBM SECURITY VERIFY/PROVIDER ID"
        set idp-single-sign-on-url " IBM SECURITY VERIFY/LOGIN URL"
        set idp-cert "REMOTE_Cert_2"
        set user-name "username"
        set group-name "Groups"
        set digest-method sha1
    next
end
```
4. Add the SAML remote groups to your existing Fortinet user groups under `User & Authentication` > `User Groups` so that your SAML users have the same permissions as your RADIUS users. 


## Section #3: Configure the FortiClient VPN

This section describes how to configure the FortiClient client for single sign-on.

1. In the FortiClient client, create a new SSL-VPN profile and enter the following values:

    | Field name        | Description                                                   |
    | ----------------- | ------------------------------------------------------------- |
    | `Connection Name` | Give the connection a meaningful name                         |
    | `Remote gateway`  | The public FQDN of the FortiGate firwall                      |


2. Select `Customized port` and enter the port number.

3. Select `Enable Single Sign-on (SSO) for VPN Tunnel`.

4. Select `Use external browser as user-agent for SAML user authentication` (optional).





