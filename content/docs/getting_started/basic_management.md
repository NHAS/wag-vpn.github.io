+++
weight = 3
+++


# Create new registration tokens

First generate a token.  
```sh
sh# ./wag registration -add -username tester
token,username
e83253fd9962c68f73aa5088604f3f425d58a963bfb5c0889cca54d63a34b2e3,tester
```

Then curl said token.  
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

Which can then be written to a config file. 

# Signing in to the Management console

Make sure that you have `ManagementUI.Enabled` set as `true`, then do the following from the console:

```
sudo ./wag webadmin -add -username <your_username> -password <your-password-here>
```
Then browse to your management listening address and enter your credentials.

The web interface itself cannot add administrative users.


<div style="float: left;">
{{< button relref="docs/getting_started/install_and_configure" >}}Previous{{< /button >}}
</div>
