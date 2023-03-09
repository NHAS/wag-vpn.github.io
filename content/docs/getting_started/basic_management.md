+++
weight = 3
bookToc = true
+++
<link rel="stylesheet" href="/css/custom.css">


# Managing Wag

Wag has a bunch of functionality, so this section is only going to define the most basic and most used actions you'll be taking when using wag in your day to day. 


## List Devices
Get a list of devices registered with wag.

{{< tabs "list_devices" >}}

{{< tab "Web UI" >}}

Go to the devices page:

<div style="text-align:center">   
<img src="/img/show_ui/devices.png" alt="Devices menu section" class="shadow">
</div>

Then the devices are all information are listed in the table. Along with the actions you can take on said devices.

<div style="text-align:center">   
<img src="/img/show_ui/devices_page.png" alt="Devices page" class="shadow">
</div>

{{< /tab >}}

{{< tab "CLI" >}}

```sh
./wag devices -list
```

The result is returned as a CSV for ease of parsing:
```sh
[root@archlinux]# ./wag devices -list -socket /tmp/wag-2.sock
username,address,publickey,authattempts,endpoint
John,10.123.5.6,mUQT9Gk2sNqZIt31oId40EdWymvXkrv+ERui5LJLIHw=,0,<nil>
John,10.123.5.5,c1ypaBo6mkVUia4JYB0B/EIs9piEiFd07ARi+endjWk=,0,<nil>
asdasd,10.123.5.4,P8ObCdLuXw1T+YUBRGEa3NM9yYYE2ERQweOslymEHSU=,0,10.0.0.14:46268
bonk,10.123.5.3,rq+nDcQAxbKUbZ2dQjjXRF/tmOc+W4j8RUdUoFfC4hY=,0,<nil>
bonk,10.123.5.2,oUg/qoWKpRljLSuS7qHp4JcXgUXY8iwbox2KaLO+sD4=,0,<nil>
```

{{< /tab >}}

{{< /tabs >}}

## Unlock Device

A device will become locked if a user attempts to authorize too many times (configurable with `Lockout` [reference](/docs/reference/configuration_file/)). 
A user may have multiple devices, if one device goes over the lockout threshold this will not effect the other devices. 

{{< tabs "unlock_devices" >}}

{{< tab "Web UI" >}}

On the devices page select the device to unlock:

<div style="text-align:center">   
<img src="/img/show_ui/devices_select.png" alt="Select locked device" class="shadow">
</div>

Click `Unlock`.

<div style="text-align:center">   
<img src="/img/show_ui/devices_after_unlock.png" alt="After unlock" class="shadow">
</div>

{{< /tab >}}

{{< tab "CLI" >}}

```sh
# Either unlock every device a user has, or a specific device by address
sudo ./wag devices -unlock -address 10.123.5.6
sudo ./wag devices -unlock -username John
```

{{< /tab >}}


{{< /tabs >}}

## Lock User Account

Locking a user account means that no devices a user has are able to authorize. 


{{< tabs "lock_account" >}}

{{< tab "Web UI" >}}
Browse to the users page:

<div style="text-align:center">   
<img src="/img/show_ui/user_page_highlight.png" alt="Users page highlight" class="shadow">
</div>

Select user:

<div style="text-align:center">   
<img src="/img/show_ui/users_page_select.png" alt="Users page with selected user" class="shadow">
</div>

Click `Unlock`:

<div style="text-align:center">   
<img src="/img/show_ui/users_page_unlocked.png" alt="Users unlocked user" class="shadow">
</div>
{{< /tab >}}

{{< tab "CLI" >}}
```sh
./wag users -lockaccount -username John
```
{{< /tab >}}

{{< /tabs >}}



## Reset User MFA

In the event that a user has lost their MFA device, whether it is TOTP, Webauthn or otherwise you may have to reset their session to re-register an MFA device.

{{< tabs "reset_mfa" >}}

{{< tab "Web UI" >}}

Browse to the users page:

<div style="text-align:center">   
<img src="/img/show_ui/user_page_highlight.png" alt="Users page highlight" class="shadow">
</div>

Select user:

<div style="text-align:center">   
<img src="/img/show_ui/users_select2.png" alt="Users page with selected user" class="shadow">
</div>


Click `Reset MFA`:

<div style="text-align:center">   
<img src="/img/show_ui/reset_mfa.png" alt="Reset MFA" class="shadow">
</div>

{{< /tab >}}

{{< tab "CLI" >}}
```sh
sudo ./wag users -reset-mfa jordan_phone
```
{{< /tab >}}

{{< /tabs >}}


<br><br>

<div style="float: left;">
{{< button relref="docs/getting_started/registering_users" >}}Previous{{< /button >}}
</div>
