# Image Mode Basics

## Instruqt Track

Access the lab at [red.ht/im-rhel](red.ht/im-rhel)

This lab provides:
- A workstation with terminal access
- A registry to push/pull images
- A bootc VM

## Preparation

Follow these instructions to fast forward to using the bootc VM and skip the long build time.

Expect less than 10min of total prep

### Step 1

Launch the track, expect 3min of provisioning.

Click **Start** on the bottom right whan available.

Paste the following commands in **Terminal** tab

This will build and push the bootc image
```
podman build -t rhel.$INSTRUQT_PARTICIPANT_ID.instruqt.io:5000/test-bootc .
podman push rhel.$INSTRUQT_PARTICIPANT_ID.instruqt.io:5000/test-bootc
```

This will build and run the bootc VM
```
podman run --rm --privileged \
        --volume .:/output \
         --volume ./config.json:/config.json \
        registry.redhat.io/rhel9/bootc-image-builder:latest \
        --type qcow2 \
        --config config.json \
         rhel.$INSTRUQT_PARTICIPANT_ID.instruqt.io:5000/test-bootc

cp qcow2/disk.qcow2 /var/lib/libvirt/images/bootc-vm.qcow2

virt-install --name bootc \
 --disk /var/lib/libvirt/images/bootc-vm.qcow2 \
--import \
--memory 2048 \
--graphics none \
--osinfo rhel9-unknown \
--noautoconsole \
--noreboot

virsh start bootc
```

### Step 2

Advance to *exercise 2* screen to access the **VM Console** tab.

Advance to *exercise 3* screen to have both extra tabs : **Containerfile** and **VM Console**

## Demo

Close the instruction pane on the right with the **>** icon on top.

At this point you have a working bootc VM and you may modify the bootc image as you please.

- Use the **Terminal** tab to build and push the image
- Use the **Containerfile** tab to edit the image file
- Use the **VM Console** tab for bootc commands

Demo scenarii are offered below to showcase bootc features and value

### 1. Bootc merges local configuration

Add a local change
```
echo "Bootc FTW !" > /var/www/html/index.html
curl localhost
```
Update the bootc image and push it, then apply it to the VM
```
bootc upgrade
reboot
```
Show that the configuration is retained
```
curl localhost
```

### 2. Bootc provides rollback capability

### 3. Bootc lets you switch to a different image
