#Image Mode Basics

## Instruqt Track
[red.ht/im-rhel](red.ht/im-rhel)

## Preparation

Expect 10min of total prep

### Step 1
Paste the following commands in *exercise 1*

```
podman build -t rhel.$INSTRUQT_PARTICIPANT_ID.instruqt.io:5000/test-bootc .
podman push rhel.$INSTRUQT_PARTICIPANT_ID.instruqt.io:5000/test-bootc
```

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
Advance to *exercise 2* to access the *VM Console* tab

## Demo
