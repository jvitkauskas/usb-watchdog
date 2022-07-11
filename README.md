# usb-watchdog

cp watchdog-restart.* /etc/systemd/system
systemctl enable --now watchdog-restart.service
systemctl enable --now watchdog-restart.timer

systemctl list-timers

