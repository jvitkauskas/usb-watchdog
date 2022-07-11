# Linux info for cheap internal USB watchdog

![Device picture](internal-usb-watchdog.png)

## How it works

Device plugs into computer via internal USB header and acts as USB HID device. It seems to accept timeout value in hex and if timeout occurs (system crashes and it does not receive another timeout override in time), it shorts two wires which should be connected to the motherboard's reset pins essentially "pressing" reset button and restarting the system.

## Setting timeout

Easiest way is to write hex value to hid device using printf. According to information online, it should timeout after 5 minutes (0x1E is 30 in decimal and the device should multiply it internally by 10 seconds, so 30 * 10 seconds = 300 seconds = 5 minutes), but ovserved timeout is about 2 minutes. Maybe my device has a different implementation. Anyway, resetting timeout each minute does the job.

Example timeout reset command:

```bash
printf '\x1E\x00'>/dev/hidraw0
```

## Using it with systemd

Copy and enable service and timer

```bash
sudo cp watchdog-restart.service /etc/systemd/system
sudo cp watchdog-restart.timer /etc/systemd/system

sudo systemctl enable --now watchdog-restart.service
sudo systemctl enable --now watchdog-restart.timer
```

Now check if timer is running

```bash
sudo systemctl list-timers
```

### Using it with cron or anything else (untested)

Add a cron job for user root with command `printf '\x1E\x00' > /dev/hidraw0`
