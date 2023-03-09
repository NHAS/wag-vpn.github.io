---
title: Welcome
type: docs
---

<link rel="stylesheet" href="/css/custom.css">

<div style="text-align:center">
<h1>Welcome to Wag</h1>
<p>
Simplify, streamline and secure your Wireguard® deployment.
</p>
<p>Add seamless route and port based MFA restrictions and enable always on management without exposing services to the untrusted web.</p>
<p>Wireguard® + MFA ❤️</p>

{{< button relref="docs/getting_started/install_and_configure" >}}Deploy it!{{< /button >}}

</div>

<br><br>

<img src="/img/show_ui/dashboard.png" alt="dashboard" class="shadow">
<br><br>

# Features

Wag has a bunch of useful features that make it stand out amongst other wireguard management suites.

## MFA

- [TOTP](https://en.wikipedia.org/wiki/Time-based_one-time_password) (Time based code)
- [Webauthn](https://www.yubico.com/authentication-standards/webauthn/) (Security key)
- [OIDC](/docs/guides/sign_sign_on) (Single Sign On)


## Access Control

You decide whether routes require MFA, and what don't. Restrict access to ports, protocols or just let users access anything on the host:

```json
"Policies": {
    "group:developers": {
        "Mfa": [
            "internal.dev 443/tcp 22/any"
        ],
        "Allow": [
            "anything.on.this.host.dev",
            "1.1.1.1/32 53/udp"
        ]
    }
}
```


## Enrollment

Wireguard makes it difficult to automatically enrol devices. Wag makes it easy by providing a single registration endpoint, which will generate a wireguard configuration file for you. 


Create a device enrollment token for your user (or use our [CLI](/docs/reference/cli)).
<div style="text-align:center">
<img src="/img/show_ui/registration_prompt.png" alt="Token create dialog" class="shadow">
</div>
  
Then provide the generated token to the user, or do it all programmically with the [CLI](/docs/reference/cli) and API.  
  
<br>
<img src="/img/show_ui/token_create_result.png" alt="Registration table" class="shadow">


Then the end user can get their wireguard configuration however they like:

Command line.
```sh
$ curl http://10.0.0.3:8081/register_device\?key=0348bd3bd28d19e4b3d1fbf9564b522b0b3367cdb29432703f945a0d98c27629

[Interface]
PrivateKey = <OMITTED>
DNS = 8.8.8.8, 8.8.9.9
Address = 10.123.5.5

[Peer]
Endpoint =  10.0.0.3:53231
PresharedKey = a26yA6jRmzKWXmAOb+NPGlArBNUH2h2PLBS3wpyjoMA=
PublicKey = EkIRbMxogx8uc5TMgTYk80k1afxYHYE47N16a55efSc=
AllowedIPs = 10.123.5.1/32, 8.8.8.8/32, 8.8.9.9/32
PersistentKeepAlive = 10
```

Browing directly to the URL in a Web browser:   
<div style="text-align:center">   
<img src="/img/show_ui/web_browser_download_conf.png" alt="Web browser downloading config" class="shadow">
</div>

Or enabling mobile enrollment with `type=mobile`:  
<div style="text-align:center">  
<img src="/img/show_ui/qr_code.png" alt="Qr code enrollment" class="shadow">
</div>


