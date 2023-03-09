+++
bookToc = true
+++

# Keycloak (OIDC) + Wag

This guide will enable you to setup Wag with Keycloak as an identity provider (IdP), this ues OIDC so really any OIDC provider that supports setting group names in the OIDC claim will work. 
However in this case keycloak is a good and easy way to start. 
  
  
This guide expects that you already have a keycloak realm configured. 
If you want to test this in a development enviroment I suggest using the docker container of keycloak.

```sh
sudo docker run -p 8080:8080 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin quay.io/keycloak/keycloak:20.0.2 start-dev
```

## Configuring KeyCloak


First we must create an OIDC client (in this guide I just used the default `account` client, but you should have a specific wag application):


<div style="text-align:center">   
<img src="/img/keycloak/create_client.png" alt="Create client page" class="shadow">
</div>

Then click on your created client (in this case `account`) and go to settings:

1. Enable `Client Authentication`, `Authorisation`, `Standard Flow and Direct Access` grants like so:
    <div style="text-align:center">   
    <img src="/img/keycloak/settings_capability_config.png" alt="Cleint Cababilities" class="shadow">
    </div>

2. Set your valid redirect URLS to your vpn domain with `/authorise/oidc` as the redirection path, also add `+` in web origins to automatically add the redirection URLs to Origin allow list:
    <div style="text-align:center">   
    <img src="/img/keycloak/settings_redirects.png" alt="Client Cababilities" class="shadow">
    </div>

Then go to `Client Scopes`:

3. Click on the `*-dedicated` scope, in this example `account-dedicated`
    <div style="text-align:center">   
    <img src="/img/keycloak/client_scopes.png" alt="Client scopes" class="shadow">
    </div>

4. Add a mapper: `By configuration`
    <div style="text-align:center">   
    <img src="/img/keycloak/client_scopes_mapper.png" alt="Add mapper" class="shadow">
    </div>

5. Choose the `Group Membership` mapper
    <div style="text-align:center">   
    <img src="/img/keycloak/client_scopes_group_mapper.png" alt="Add mapper" class="shadow">
    </div>

6. Set `Name` and `Token Claim Name` to `groups` (or whatever group name you want, update the wag config file if not groups) disable `Full group path`:
    <div style="text-align:center">   
    <img src="/img/keycloak/set_token_claims.png" alt="Add mapper" class="shadow">
    </div>

7. Get `ClientID` and `Client Secret` from `Credentials`

## Configuring Wag

For `OIDC` to be properly enabled, the following fields in the wag configuration file must be set:  

`DomainURL`: The internal wag VPN host domain. E.g the host users visit to MFA  
`OIDC.IssuerURL`: The OIDC provider path  
`OIDC.ClientID`: The OIDC client ID, what we got in step 7.  
`OIDC.ClientSecret`: The OIDC secret, what we got in step 7.  

Additionally, an optional field may be defined. This is if your OIDC provider defines its groups in something non-standard, i.e not groups. 
`OIDC.GroupsClaimName`
  
  
You may also want to restrict users to only using the `OIDC` provider. Which you can do by setting the `Authenticators.Methods` to `["oidc"]`.

So as an example here is a fragment of a wag configuration file:
```json
"Authenticators": {                                                          
    "Issuer": "otp-uat.domain.tld",                                      
    "DomainURL": "https://uat.domain.tld",                               
    "DefaultMethod": "oidc",                                             
    "Methods": [                                                                                                                     
        "oidc"                                                               
    ],                                                                   
    "OIDC": {                                                            
        "IssuerURL": "https://sso.domain.tld/auth/realms/your-realm-name",    
        "ClientSecret": "SecretKey",              
        "ClientID": "wag-uat",                                       
        "GroupsClaimName": "groups"                                      
    }                                                                
}, 
```

### Management

As the OIDC identity provider is providing groups to wag, there is no need to set up `groups` in wag for OIDC users. Simply go to `Rules` and define your group policies:

In this case, I've added my user to the `toast` group in keycloak, so I create a `Rule` for that group (note the `group:` prefix):

<div style="text-align:center">   
<img src="/img/keycloak/group_toast_oidc.png" alt="Group policies" class="shadow">
</div>


Then after logging in my user can query it's routes:
```json
$ curl http://vpn.test:8081/status/
{
  "IsAuthorised": true,
  "Routes": [
    "2.2.2.2/32",
    "10.123.5.1/32"
  ]
}
```

And we see the `2.2.2.2/32` route.