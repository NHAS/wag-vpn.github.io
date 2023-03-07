---
bookToC: true
---

# CLI Reference


The root user is able to manage the wag server with the following command:
  
```sh
wag subcommand [-options]
```

Supported commands: `start`, `cleanup`, `reload`, `version`, `firewall`, `registration`, `devices`, `users`, `webadmin`, `upgrade`, `gen-config`



## `start`

Initalises the wireguard device and starts wag server:  
```sh
Usage of start:
  Start wag server (does not daemonise)
  -config string
        Configuration file location (default "./config.json")
```

## `cleanup`
Remove all wag added iptables forwards, and shuts down the wireguard device.  

## `reload`
Live reloads wag configuration.

## `version`
Display the version of wag (git tag and commit hash).

## `firewall`: 
Get current state of wags internal eBPF firewall (i.e what users are authed and allowed to what routes):
```sh
Usage of firewall:
  -list
        List firewall rules
  -socket string
        Wag socket to act on (default "/tmp/wag.sock")

``` 

## `registration`

Create, delete and show registration tokens.
```sh
Usage of registration:
  -add
        Create a new enrolment token
  -del
        Delete existing enrolment token
  -group value
        Manually set user group (can supply multiple -group, or use -groups for , delimited group list, useful for OIDC)
  -groups string
        Set user groups manually, ',' delimited list of groups, useful for OIDC
  -list
        List tokens
  -overwrite string
        Add registration token for an existing user device, will overwrite wireguard public key (but not 2FA)
  -socket string
        Wag socket to act on (default "/tmp/wag.sock")
  -token string
        Manually set registration token (Optional)
  -username string
        User to add device to
```  

## `devices`

Create, delete, show and lock user wireguard devices.  
```sh
Usage of devices:
  -address string
        Address of device
  -del
        Remove device and block wireguard access
  -list
        List wireguard devices
  -lock
        Lock device access to mfa routes
  -mfa_sessions
        Get list of devices with active authorised sessions
  -socket string
        Wag control socket to act on (default "/tmp/wag.sock")
  -unlock
        Unlock device
  -username string
        Owner of device (indicates that command acts on all devices owned by user)
```
  
## `users`
Manage user account. 
```sh
Usage of users:
  -del
        Delete user and all associated devices
  -list
        List users, if '-username' supply will filter by user
  -lockaccount
        Lock account disable authention from any device, deauthenticates user active sessions
  -reset-mfa
        Reset MFA details, invalids all session and set MFA to be shown
  -socket string
        Wag socket location, (default "/tmp/wag.sock")
  -unlockaccount
        Unlock a locked account, does not unlock specific device locks (use device -unlock -username <> for that)
  -username string
        Username to act upon
```

## `webadmin`
Manages the administrative users for the management web UI.
```sh
Usage of webadmin:
  -add
        Add web administrator user (requires -password)
  -del
        Delete admin user
  -list
        List web administration users, if '-username' supply will filter by user
  -lockaccount
        Lock admin account disable login for this web administrator user
  -password string
        Username to act upon
  -socket string
        Wag instance control socket (default "/tmp/wag.sock")
  -unlockaccount
        Unlock a web administrator account
  -username string
        Admin Username to act upon
```

## `upgrade`
Pin all ebpf programs, shutdown wag server and optionally copy in the new binary all while leaving the XDP firewall online.  

Note, this will not restart the server after shutdown, you will manually need to start the server after with your preferred service manager (`systemctl start wag`)
```sh
Usage of upgrade:
  -force
        Disable compatiablity checks
  -hash string
        Version hash from new wag version (find this by doing ./wag version -local)
  -manual
        Shutdown the server in upgrade mode but will not copy or automatically check the new wag binary
  -path string
        File path to new wag executable
  -socket string
        Wag socket location, (default "/tmp/wag.sock")
```
