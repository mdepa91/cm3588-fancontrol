# Primitive fan control for CM3588 boards running Armbian

The current Armbian image (as of 2024-11-02) for the CM3588 doesn't enable the
fan control node in the Device Tree, so instead, we can set the GPIO chip
connected to the fan connector to PWM mode and use that to control fan speed.

## Installation

```
sudo cp fanctl.service /etc/systemd/system
sudo cp fanctl /usr/local/sbin
sudo systemctl daemon-reload
sudo systemctl enable --now fanctl
```

## Coil Whine / Modifying Speed

If after starting the service, the fan emits coild whine and doesn't spin up,
you need to increase the minium speed. Either change it in the script directly
and restart the service, or stop the service and echo the values (multipiled
with 1000, value must be betwen 1 and 100000) to sysfs:

```
# set speed to 50 of 100:
echo 50000 | sudo tee /sys/class/pwm/pwmchip0/pwm0/duty_cycle
```

My Noctua fan needed only 30, while the fan that was delivered with the metal
case needed between 42 and 46 to reliably spin up.
