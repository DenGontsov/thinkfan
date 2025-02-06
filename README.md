# Configuration and setup ThinkFan on Lenovo T14
# Add permissions
```
sudo chmod 666 /proc/acpi/ibm/fan
sudo chmod +r /sys/devices/platform/coretemp.0/hwmon/hwmon*/temp*_input
```

# Add and activate modules
```
Create file /etc/modprobe.d/thinkpad.conf and add folowing config
```
```
options thinkpad_acpi fan_control=1
```

```
# modprobe -r thinkpad_acpi
# modprobe thinkpad_acpi
# modprobe thinkpad_acpi fan_control=1
```

# Find all sensors on your device:
```
find /sys/devices/platform/coretemp.0/hwmon/hwmon*/temp*_input
```

# Create symlinks for your core temp sensors, because hwmon* - changes dynamically every time the OS boots, for example:
```
sudo ln -s /sys/devices/platform/coretemp.0/hwmon/hwmon*/temp1_input /opt/sensors/cpu/temp1_input
sudo ln -s /sys/devices/platform/coretemp.0/hwmon/hwmon*/temp2_input /opt/sensors/cpu/temp2_input
sudo ln -s /sys/devices/platform/coretemp.0/hwmon/hwmon*/temp3_input /opt/sensors/cpu/temp3_input
```

# Edit config file /etc/thinkfan.conf
```
## Thinkfan configuration
tp_fan /proc/acpi/ibm/fan

hwmon /opt/sensors/cpu/temp1_input
hwmon /opt/sensors/cpu/temp2_input
hwmon /opt/sensors/cpu/temp3_input

(0, 0, 40)
(1, 40, 44)
(2, 44, 48)
(3, 48, 52)
(4, 52, 56)
(5, 56, 60)
(6, 60, 64)
(7, 64, 68)
(127, 68, 32767)
```
# Restart and check 
```
sudo systemctl restart thinkfan
sudo systemctl status thinkfan
```
