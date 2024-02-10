# test

```bash
#!/bin/bash

# Auto-nuke password
AUTO_NUKE_PASSWORD="YOUR_PASSWORD"

# Delay before auto-nuke (seconds)
AUTO_NUKE_DELAY=10

# Wipe mode (0: secure, 1: fast)
AUTO_NUKE_WIPE_MODE=0

if [ "$1" == "$AUTO_NUKE_PASSWORD" ]; then

    echo "Auto-nuke initiated..."

    # Calculate the start time
    START_TIME=$(date +%s)

    # Wait for the specified delay
    sleep $AUTO_NUKE_DELAY

    # Get the UUID of the Kali USB drive
    USB_DRIVE=$(blkid -t LABEL=kali_usb | awk '{print $2}' | sed 's/UUID=//')

    if [ $AUTO_NUKE_WIPE_MODE -eq 0 ]; then
        # Secure wipe (slower)
        echo "Secure wiping..."
        dd if=/dev/zero of=/dev/$USB_DRIVE bs=1M
    else
        # Fast wipe (less secure)
        echo "Fast wiping..."
        dd if=/dev/urandom of=/dev/$USB_DRIVE bs=1M count=1
    fi

    # Calculate the end time
    END_TIME=$(date +%s)

    # Calculate the total time taken for wiping
    TOTAL_TIME=$((END_TIME - START_TIME))

    echo "Wipe complete. Total time taken: $TOTAL_TIME seconds."

else
    echo "Invalid password."
fi
```
