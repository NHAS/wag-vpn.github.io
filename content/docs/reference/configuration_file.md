---
bookToC: true
---

# Configuration file reference
  
`Proxied`: Respect the `X-Forward-For` directive, must ensure that you are setting the `X-Forward-For` directive in your reverse proxy as wag relies on the client IP for authentication in the VPN tunnel  
`HelpMail`: The email address that is shown on the prompt page  
`Lockout`: Number of times a person can attempt mfa authentication before their account locks  
  
`ExternalAddress`: The public address of the server, the place where wireguard is listening to the internet, and where clients can reach the `/register_device` endpoint    
  
`MaxSessionLifetimeMinutes`: After authenticating, a device will be allowed to talk to privileged routes for this many minutes, if -1, timeout is disabled  
`SessionInactivityTimeoutMinutes`: If a device has not sent data in `n` minutes, it will be required to reauthenticate, if -1 timeout is disabled  
  
`DatabaseLocation`: Where to load the sqlite3 database from, it will be created if it does not exist  
`Socket`: Wag control socket, changing this will allow multiple wag instances to run on the same machine  
`Acls`: Defines the `Groups` and `Policies` that restrict routes  
  
`Webserver`: Object that contains the public and tunnel listening addresses of the webserver  

`WebServer.Public.ListenAddress`: Listen address for endpoint  
`WebServer.Tunnel.Port`: Port for in-vpn-tunnel webserver, this does not take a full IP address, as the tunnel listener should *never* be outside the wireguard device

`WebServer.<endpoint>.CertPath`: TLS Certificate path for endpoint  
`WebServer.<endpoint>.KeyPath`: TLS key for endpoint  
  
`Authenticators`: Object that contains configurations for the authentication methods wag provides  
`Authenticators.Issuer`: TOTP issuer, the name that will get added to the TOTP app  
`Authenticators.DomainURL`: Full url of the vpn authentication endpoint, required for `webauthn` and `oidc`
`Authenticators.DefaultMethod`: String, default method the user will be presented, if not specified a list of methods is displayed to the user (possible values: `webauth`, `totp`, `oidc`)    
`Authenticators.Methods`: String array, enabled authentication methods, e.g ["totp","webauthn","oidc"]

`Authenticators.OIDC`: Object that contains `OIDC` specific configuration options
`Authenticators.OIDC.IssuerURL`: Identity provider endpoint, e.g `http://localhost:8080/realms/account`
`Authenticators.OIDC.ClientID`:  OIDC identifier for application
`Authenticators.OIDC.ClientSecret`: OIDC secret
`Authenticators.OIDC.GroupsClaimName`: Not yet used. 
  
`Wireguard`: Object that contains the wireguard device configuration  
`Wireguard.DevName`: The wireguard device to attach or to create if it does not exist, will automatically add peers (no need to configure peers with `wg-quick`)  
`Wireguard.ListenPort`: Port that wireguard will listen on  
`Wireguard.PrivateKey`: The wireguard private key, can be generated with `wg genkey`  
`Wireguard.Address`: Subnet the VPN is responsible for  
`Wireguard.MTU`: Maximum transmissible unit defaults to 1420 if not set for IPv4 over Ethernet  
`Wireguard.PersistentKeepAlive`: Time between wireguard keepalive heartbeats to keep NAT entries alive, defaults to 25 seconds
`Wireguard.DNS`: An array of DNS servers that will be automatically used, and set as "Allowed" (no MFA)  
   
`ManagementUI`: Object that contains configurations for the webadministration portal. It is not recommend to expose this portal, I recommend setting `ListenAddress` to `127.0.0.1`/`localhost` and then use ssh forwarding to expose it  
`ManagementUI.Enabled`: Enable the web UI  
`ManagementUI.ListenAddress`: Listen address to expose the management UI on  
`ManagementUI.CertPath`: TLS Certificate path for management endpoint  
`ManagementUI.KeyPath`: TLS key for the management endpoint  
  
Full config example
```json
{
    "Lockout": 5,
    "HelpMail": "help@example.com",
    "MaxSessionLifetimeMinutes": 2,
    "SessionInactivityTimeoutMinutes": 1,
    "ExternalAddress": "192.168.121.61",
    "DatabaseLocation": "devices.db",
    "Socket":"/tmp/wag.sock",
    "Webserver": {
        "Public": {
            "ListenAddress": "192.168.121.61:8080"
        },
        "Tunnel": {
            "Port": "8080"
        }
    },
    "ManagementUI": {
        "ListenAddress": "127.0.0.1:4433",
        "Enabled": true
    },
    "Authenticators": {
        "Issuer": "vpn.test",
        "DomainURL": "https://vpn.test:8080",
        "DefaultMethod":"webauthn",
        "Methods":["totp","webauthn", "oidc"],
        "OIDC": {
            "IssuerURL": "http://localhost:8080/",
            "ClientSecret": "<OMITTED>",
            "ClientID": "account",
            "GroupsClaimName": "groups"
        }
    },
    "Wireguard": {
        "DevName": "wg0",
        "ListenPort": 53230,
        "PrivateKey": "AN EXAMPLE KEY",
        "Address": "192.168.1.1/24",
        "MTU": 1420,
        "PersistentKeepAlive": 25,
        "DNS": ["1.1.1.1"]
    },
    "Acls": {
        "Groups": {
            "group:nerds": [
                "toaster",
                "tester",
                "abc"
            ],
        },
        "Policies": {
            "*": {
                "Allow": [
                    "10.7.7.7",
                    "google.com"
                ]
            },
            "username": {
                  "Allow":[ "10.0.0.1/32"]
            },
            "group:nerds": {
                "Mfa": [
                    "192.168.3.4/32"
                    "thing.internal 443/tcp icmp"
                ],
                "Allow": [
                    "192.168.3.5/32"
                ]
            }
        }
    }
}
```

## Rule syntax
  
The `Policies` section allows you to define what routes, ports and protocols should be both captured by the VPN and allowed through wag respectively.
Currently 3 types of port and protocol rules are supported:  
  
### Any 

When no other rules are defined or the `any` keyword is used wag will allow all services and port combinations.

Example: 

```sh
"1.1.1.1": Allows all ports and protocols to 1.1.1.1/32
"1.1.1.1 54/any": Allows both tcp and udp to 1.1.1.1/32
```

### Single Service

Example:
```sh
192.168.1.1 22/tcp 53/udp: Fairly self explanatory, allows you to hit 22/tcp and 53/udp on a host
1.1.1.1 icmp: As icmp doesnt have ports really you dont need it either
```

### Ranges
You can also define a range of ports with a protocol. wag requires that the lower port is first. 

Example:
```sh
192.168.1.1 22-1024/tcp 53-23/any: Format is low port-high port/service
```