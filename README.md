# Primitive fan control for CM3588 boards running OMV

CM3588 allows only connect 5V fan, so instead, we can connect 12V PWM fan
to 12V main power and 4th pin(PWM control) connect to GPIO 33.
Then scripts sets proper fan rpm depending of CPU temperature or NVME drive - takes highest.

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
with 1000, value must be betwen 1 and 25000) to sysfs:

```
# set speed to 1000 of 25000:
echo 1000 | sudo tee /sys/class/pwm/pwmchip3/pwm0/duty_cycle
```
then to calculate percentage divide value by 250

My Noctua fan needed only 30, while the fan that was delivered with the metal
case needed between 42 and 46 to reliably spin up.
