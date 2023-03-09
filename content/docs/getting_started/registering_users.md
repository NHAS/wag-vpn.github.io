+++
weight = 2
bookToc = true
+++

## Device Registration

A user is created in wag by registering a device, a user may have many devices but will not be present in the system until they have registered one device.

## Create New Registration Tokens

{{< tabs "new_token" >}}

{{< tab "Web UI" >}}

Under the `Registration Tokens` section, add a new token.

<div style="text-align:center">
<img src="/img/show_ui/registration_prompt.png" alt="Token create dialog" class="shadow">
</div>

Then copy the token from here: 

<img src="/img/show_ui/token_create_result.png" alt="Registration table" class="shadow">

{{< /tab >}}

{{< tab "CLI" >}}

First generate a token with the [registration](/docs/reference/cli#registration) command.  
```sh
sh# ./wag registration -add -username tester
token,username
e83253fd9962c68f73aa5088604f3f425d58a963bfb5c0889cca54d63a34b2e3,tester
```
{{< /tab >}}

{{< /tabs >}}
  
## Accessing Tokens

Wag listens and exposes a public API which, when supplied the generated token will generate and insert a device into the users device list. 
The following example uses curl, but mainly this shows how easy it is to automate.
  
```sh
curl http://public.server.address:8080/register_device?key=e83253fd9962c68f73aa5088604f3f425d58a963bfb5c0889cca54d63a34b2e3
```

The service will return a fully templated response:
```sh
[Interface]
PrivateKey = <omitted>
Address = 192.168.1.1

[Peer]
Endpoint =  public.server.address:51820
PublicKey = pnvl40WiRt++0NucEGexlpfwWA8QzBYg2+8ZWZJvejA=
AllowedIPs = 10.7.7.7/32, 192.168.1.1/32, 192.168.3.4/32, 192.168.3.5/32
PersistentKeepAlive = 10
```

Which can then be written to a config file and started

<div style="float: left;">
{{< button relref="docs/getting_started/install_and_configure" >}}Previous{{< /button >}}
</div>


<div style="float: right;">
{{< button relref="docs/getting_started/basic_management" >}}Next{{< /button >}}
</div>
