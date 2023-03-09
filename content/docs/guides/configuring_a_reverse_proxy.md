# Configuring Wag for a Reverse Proxy

When running wag behind a reverse proxy there are two configuration you **must** do in order to maintain security. 
  

{{< hint danger >}}
**WARNING**  
Wag uses the IP address of the tunnel device to determine device identification if `X-Forwarded-For` is **not** set in your reverse proxy this may lead to **catastrophic** security issues where a device can spoof what user it belongs to.
{{< /hint >}}


This guide is reverse proxy agnostic, all that is required is that the proxy speaks HTTP and can set the `X-Forwarded-For` header. 


## Configuring Wag

First configure `Wag` to respect the `X-Forwarded-For` header:  
```json 
Proxied: "true"
```
Add the port that the reverse proxy will expose to the `ExposePorts` option like so:   
```json
ExposePorts: ["80/tcp", "443/tcp"]
```

It is recommended that you set the `WebServer.Public.ListenAddress` to `localhost:port`, so that you can then expose it via your reverse proxy. 
The MFA, or tunnel we listener (`WebServer.Tunnel.Port`) will not have an associated IPtales rule to expose it when `Proxied` is true. 

