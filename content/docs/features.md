+++
weight = 2
bookToc = true
+++

# Features

Wag has a bunch of useful features that make it stand out amongst other wireguard management suites.

## MFA

The primary purpose of wag is to bring MFA to wireguard.

Wag currently supports the following MFA methods: 
- [TOTP](https://en.wikipedia.org/wiki/Time-based_one-time_password) (Time based code)
- [Webauthn](https://www.yubico.com/authentication-standards/webauthn/) (Security key)
- [OIDC](https://openid.net/connect/) (Single Sign On, [guide](/docs/guides/sign_sign_on))


## Access Control

You decide whether routes require MFA, and what dont. Restrict access to ports, protocols or just let users access anything on the host:

```json
"Policies": {
    "group:developers": {
        "Mfa": [
            "internal.dev 443/tcp"
        ],
        "Allow": [
            "1.1.1.1/32 53/udp"
        ]
    }
}
```


## Deployment

Wireguard makes it difficult to manage large deployments of user devices. Wag makes it easy by providing a single registration endpoint, which will generate a wireguard configuration file for you. 

## Management UI

