# Extended Update Support

## Instruqt Track

Access the lab at [www.redhat.com/en/interactive-labs/introduction-to-extended-update-support](https://www.redhat.com/en/interactive-labs/introduction-to-extended-update-support)

This lab provides :
- A RHEL 9 server with terminal
- Access to EUS repositories

Time limit : 50min

## Preparation

Follow these instructions to limit update times and remove hyperscaler repos for a smooth demo.

Expect around 5min of total prep (mostly update time).

<details open>
        
### Step 1

- Click **Launch** on the bottom right, expect 3min of provisioning.
- Click **Start** on the bottom right when it appears.

### Step 2

- Paste the following commands all at once in **Terminal** tab
```
dnf config-manager --disable

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
        
XXX        
```bash

```

</details>

### 2. System Purpose helps you track usage
