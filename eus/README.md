# Extended Update Support

## Instruqt Track

Access the lab at [www.redhat.com/en/interactive-labs/introduction-to-extended-update-support](https://www.redhat.com/en/interactive-labs/introduction-to-extended-update-support)

This lab provides :
- A RHEL 9 server with terminal access
- EUS repositories

Time limit : 50min

## Preparation

Follow these instructions to limit update times and remove hyperscaler repos for a smooth demo.

Expect around 5min of total prep (mostly update time).

<details open>
        
### Step 1

- Click **Launch** on the bottom right, expect 2min of provisioning.
- Click **Start** on the bottom right when it appears.

### Step 2

- Paste the following commands all at once in **Terminal** tab
```
dnf config-manager --disable google*

subscription-manager release --set 9.2
dnf update -y

subscription-manager release --set 9
```

It's ready !

</details>

## Demo

Now close the instruction pane on the right with the **>** icon on top.

At this point you have a RHEL 9.2 system that is up to date as of 9.2 release. The release is not set.

Here is an example demo scenario that showcases EUS and system purpose, but you can design your own.

### 1. Extended Update Support provides fixes for a set minor release

<details open>
        
Before you start, display the current release (9.2)       
```bash
cat /etc/redhat-release
```

Show that many updates are available but will update the system to the latest release
```bash
dnf list --updates
```

Show available releases        
```bash
subscription-manager release --list
```

Set the release at 9.2 and show that no more updates are available.         
```bash
subscription-manager release --set 9.2
dnf update
```

Display the current repos providing updates and disable all of them        
```bash
subscription-manager repos --list-enabled
subscription-manager repos --disable *
```

Find EUS repos available for this system     
```bash
subscription-manager repos | grep -i eus
```

Enable BaseOS and AppStream EUS repos       
```bash
subscription-manager repos --enable rhel-9-for-x86_64-baseos-eus-rpms --enable rhel-9-for-x86_64-appstream-eus-rpms
```

Show the updates coming from EUS repos. Accept all updates         
```bash
dnf update
```

Display the current release that is unchanged yet up-to-date (9.2)
```bash
cat /etc/redhat-release
```

</details>

### 2. System Purpose labels usage in SCA mode

<details open>
        
With SCA enabled, **subscription-manager status** returns puzzling output and this is normal      
```bash
subscription-manager status
```

Show System Purpose integration        
```bash
subscription-manager syspurpose --help
```

Show how to set system *Usage*        
```bash
subscription-manager syspurpose usage --list
subscription-manager syspurpose usage --set Production
```

Show how to set system *SLA*        
```bash
subscription-manager syspurpose service-level --list
subscription-manager syspurpose service-level --set Premium
```

Display the result and explain that it is used for reporting tools like the **Subscriptions Service** in Console.       
```bash
subscription-manager syspurpose
```

### 3. Follow-up with the Subscription Service demo

[Instruqt Track](https://red.ht/subscriptions-lab) 
</details>
