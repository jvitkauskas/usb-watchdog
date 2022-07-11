# usb-watchdog

![Device picture](internal-usb-watchdog.png)

Copy and enable service and timer

```bash
sudo cp watchdog-restart.service /etc/systemd/system
sudo cp watchdog-restart.timer /etc/systemd/system

sudo systemctl enable --now watchdog-restart.service
sudo systemctl enable --now watchdog-restart.timer
```

Check if timer is visible

```bash
sudo systemctl list-timers
```
