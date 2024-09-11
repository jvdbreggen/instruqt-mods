# Image Mode Basics

## Instruqt Track

Access the lab at [red.ht/im-rhel](red.ht/im-rhel)

This lab provides:
- A workstation with terminal access
- A registry to host images
- A bootc VM
- 60min of runtime

## Preparation

Follow these instructions to fast forward to using the bootc VM and skip the long build time during your demo.

Expect less than 10min of total prep (build time included).

### Step 1

- Click **Launch** on the bottom right, expect 3min of provisioning.
- Click **Start** on the bottom right when it appears.

### Step 2

This will build and push the bootc image, then build and run the bootc VM (~5min).

- Paste the following commands all at once in **Terminal** tab
```
podman build -t rhel.$INSTRUQT_PARTICIPANT_ID.instruqt.io:5000/test-bootc -f Containerfile
podman push rhel.$INSTRUQT_PARTICIPANT_ID.instruqt.io:5000/test-bootc

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

### Step 3

- Click **Check** to advance to *exercise 2* screen, you should now see the **VM Console** tab.
- Click **Check** to advance to *exercise 3* screen, you should now see both the **Containerfile** and **VM Console** tabs.

## Demo

Close the instruction pane on the right with the **>** icon on top.

At this point you have a working bootc VM and you may modify the bootc image as you please.

- Use the **Terminal** tab to build and push the image
- Use the **Containerfile** tab to edit the image file
- Use the **VM Console** tab for bootc commands

Demo scenarii are offered below to showcase bootc features and value, but you can design your own.

### 1. Bootc merges local configuration

Tab **VM Console** : Make a local change and show it
```
echo "Bootc FTW !" > /var/www/html/index.html
curl localhost
```
Tab **Terminal** : Update the bootc image and push it to registry
```
podman build -t rhel.$INSTRUQT_PARTICIPANT_ID.instruqt.io:5000/test-bootc -f Containerfile
podman push rhel.$INSTRUQT_PARTICIPANT_ID.instruqt.io:5000/test-bootc
```
Tab **VM Console** : Apply the update to the bootc VM
```
bootc upgrade
reboot
```
Show that the local change was retained
```
curl localhost
```

### 2. Bootc provides rollback capability

### 3. Bootc lets you switch to a different image
