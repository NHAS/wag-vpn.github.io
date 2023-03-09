+++
weight = 1
bookToc = true
+++

## Installation

1. Check requirements
   ```
   Glibc 2.31+
   Linux Kernel 5.3+
   ```

2. Download the latest release of wag:
    ```sh
    curl -L https://github.com/NHAS/wag/releases/latest/download/wag -o wag && chmod +x wag
    ```

3. Enable the wag host to act as a router:
    ```sh
    sudo sysctl -w net.ipv4.ip_forward=1
    ```

4. Generate a base configuration file for wag
    ```sh
    sudo ./wag gen-config
    ```
  
## Configuration
  
From here you'll want to add ACLs (access control policies) to define what users can access, the easiest way to do this is using the management UI (which you hopefully enabled in step `4` above).   
Or you can always edit the `JSON` configuration file that was generated.   
  
See the full configuration file reference [here](/docs/reference/configuration_file).

Add yourself an administrative user using the [webadmin](/docs/reference/cli/#webadmin) subcommand:
```sh
sudo ./wag webadmin -add -username <your_username> -password <password_here>
```

{{< tabs "new_token" >}}

{{< tab "Web UI" >}}

Then login to the web interface in this example the web interface is listening on `localhost:4433`:

<div style="text-align:center">
<img src="/img/show_ui/signin.png" alt="Login Page" class="shadow">
</div>


Navigate to "Rules":  
    
<div style="text-align:center">
<img src="/img/show_ui/dashboard_for_config.png" alt="Dashboard" class="shadow">
</div>

Click `+ New`:
<div style="text-align:center">
<img src="/img/show_ui/rules.png" alt="Dashboard" class="shadow">
</div>

  

The `effects` input defines what user, or group the rule applies to.  
To apply to all users, set this to `*`.  
Define your users and rules, the syntax for rule definitions can be found [here](/docs/reference/policy_rules.md):    
<div style="text-align:center">
<img src="/img/show_ui/rules_dialog.png" alt="New Rule" class="shadow">
</div>

{{< /tab >}}

{{< tab "CLI" >}}

Open your configuration file in whatever editor you prefer. 

```sh
nano config.json
```

Navigate to the `Policies` section:

```json
...
"Policies": {
     
    }
...
```
<br>

{{< hint info >}}
**Info**  
Find rule syntax [here](/docs/reference/policy_rules):
{{< /hint >}}

Add your rules:
```json
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
```

{{< /tab >}}

{{< /tabs >}}

## Start!

{{< hint info >}}
**Note**  
This will start the wag server attached to the console. 
{{< /hint >}}

```sh
sudo ./wag start
```



<div style="float: right;">
{{< button relref="docs/getting_started/registering_users" >}}Next{{< /button >}}
</div>
