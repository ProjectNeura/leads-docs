# User Manual

This user manual shall guide you through the whole reproduction process of LEADS VeC.

:::{tip}
This user manual is written in steps in order.
:::

## Connect the Wheel Speed Sensors

You must connect the wheel speed sensors to the designated pins.

| Position          | Pin |
|-------------------|-----|
| Left Front Wheel  | 2   |
| Right Front Wheel | 3   |
| Left Rear Wheel   | 4   |
| Right Rear Wheel  | 5   |
| Center Rear Wheel | 6   |

## Install the Operating System for the Raspberry Pi

Download and install the [Raspberry Pi Imager](https://www.raspberrypi.com/software).

It should look similar to this.

![rpi-imager](../_static/rpi-imager.png)

Select the "Raspberry Pi Device" accordingly. For the "Operating System", **Ubuntu Desktop 22.04 LTS or later** is
recommended. They are in "Other general-purpose OS".

![rpi-imager-os.png](../_static/rpi-imager-os.png)

Select the "Storage" accordingly, then click "NEXT".

If a dialog like the one below pops up, just say yes.

![rpi-imager-warning](../_static/rpi-imager-warning.png)

From then on, wait until this dialog shows up.

![rpi-imager-finish.png](../_static/rpi-imager-finish.png)

Click "CONTINUE" and make sure the SD card has been **successfully ejected** before you remove it.

Insert the SD card to the Raspberry Pi and power up. Follow the instructions to install the system.

:::{warning}
The installation is very likely to fail if you do not eject the SD card properly, or you click "Continue" too soon
before Wi-Fi is fully connected (you will see during the installation process on the Raspberry Pi).
:::

## Environment Setup

You may also find references [here](https://github.com/ProjectNeura/LEADS?tab=readme-ov-file#environment-setup).

:::{tip}
If you encounter "permission denied" errors, grant root permission.

```shell
sudo su
```

:::

### Install LEADS

Follow [this](https://github.com/ProjectNeura/LEADS?tab=readme-ov-file#leads) section to install everything.

If success, you should see a similar output.

```shell
[Level.INFO: 1] [<module>] [2024-05-11 20:32:26.899024] LEADS VeC
System Kernel: ...
Python Version: ...
User: ...
`frpc` Available: ...
Module Path: ...
LEADS Version: ...
LEADS VeC Version: ...
```

### Register LEADS VeC

```shell
leads-vec -r systemd run
```

You may close the program immediately after the window shows up.

```shell
systemctl --user daemon-reload
systemctl --user enable leads-vec
```

:::{warning}
Always use explicit startup until deployment. The logging information may not be seen in implicit mode. Stop the service
during setup.

```shell
systemctl --user stop leads-vec 
```

:::

### Install FRP (Optional)

Follow [this](https://github.com/ProjectNeura/LEADS?tab=readme-ov-file#frp) section to install FRP.

Should you have membership in the VeC Project, please ask your contact for the FRP server credentials.

### Install GStreamer (Optional)

We use GStreamer for streaming, follow [this](https://github.com/ProjectNeura/LEADS?tab=readme-ov-file#gstreamer)
section to install GStreamer.

Should you have membership in the VeC Project, please ask your contact for the YouTube stream key.

:::{tip}
To check what the cameras are, install `v4l-utils`.

```shell
apt install v4l-utils
v4l2-ctl --list-devices
```

You will find paths like "/dev/video0" under each device. Use the first path for each device. The camera argument passed
to the script for "/dev/video0" is `0`.
:::

### Reboot

Reboot the Raspberry Pi to apply changes.

```shell
reboot
```

When it starts again, you should see LEADS VeC automatically running.

## Ubuntu Settings

:::{tip}
These settings can possibly be found in various locations depending on the specific system version.
:::

To prevent the screen blanks off in the middle of driving, you should set "Screen Blank" to "Never".

![ubuntu-settings-power.png](../_static/ubuntu-settings-power.png)

To improve the experience, it is recommended to enable "Auto-hide the Dock".

![ubuntu-settings-dock](../_static/ubuntu-settings-dock.png)

## Configurations

To properly configure LEADS VeC, you will need to create a configuration file. Since the Systemd service uses
"/usr/local/leads/config.json" by default, it is recommended to use the same. Otherwise, make sure you refer to the
correct file anywhere LEADS VeC starts.

If you have correctly set up the environment, the file should already exist. Edit it using Vim.

:::{tip}
Install Vim first if it is not bundled with the system.

```shell
apt install -y vim
```

:::

```shell
vim /usr/local/leads/config.json
```

Press <kbd>I</kbd> to enter insert mode.

:::{tip}
To save and quit, press <kbd>Esc</kbd> and type in ":wq", then press <kbd>Enter</kbd>.
:::

For racing, daily test drives, and debugging, there are different configurations for each.

You may also add optional configurations. Learn more about the configurations
[here](https://github.com/ProjectNeura/LEADS?tab=readme-ov-file#configurations).

### Racing

```json
{
  "manual_mode": true,
  "fullscreen": true,
  "w_debug_level": "ERROR",
  "gps_receiver_port": "/dev/...",
  "wheel_speed_controller_port": "/dev/...",
  "power_controller_port": "/dev/...",
  "front_wheel_diameter": 20,
  "rear_wheel_diameter": 26
}
```

### Daily Test Drives

```json
{
  "fullscreen": true,
  "refresh_rate": 60,
  "w_debug_level": "WARN",
  "gps_receiver_port": "/dev/...",
  "wheel_speed_controller_port": "/dev/...",
  "power_controller_port": "/dev/...",
  "front_wheel_diameter": 20,
  "rear_wheel_diameter": 26
}
```

### Debugging

```json
{
  "fullscreen": true,
  "refresh_rate": 60,
  "gps_receiver_port": "/dev/...",
  "wheel_speed_controller_port": "/dev/...",
  "power_controller_port": "/dev/...",
  "front_wheel_diameter": 20,
  "rear_wheel_diameter": 26
}
```

### Determine Devices' Ports

Sometimes the ports may differ due to various hardware layouts. You can determine the port using the described
procedure.

1. Remove all USB devices
2. Plug in one device
3. List current ACM ports
   ```shell
   ls /dev/ttyACM*
   ```
   If no port is found, try AMA ports.
   ```shell
   ls /dev/ttyAMA*
   ```
4. Set the device to the newly appeared port
5. Repeat steps 2 to 4 for other devices

If you see "permission denied", apply this command to the corresponding device.

The dynamic symbol changes on every reboot. A more persistent identifier can be acquired through the following command.

```shell
ls -l /dev/serial/by-id/
```

For example, if the dynamic symbol has been determined to be "/dev/ttyACM0", you will find something similar to
`usb-FTDI_FT232R_USB_UART_AI03AC70-if00-port0 -> /dev/ttyACM0`.

## Update LEADS

```shell
pip-leads install --upgrade leads
```

## Start the Program

:::{warning}
Running as root is not recommended.
:::

The program should automatically start if it is correctly configured. If not, use the following command.

```shell
leads-vec -c /usr/local/leads/config.json run
```

## Fault Lights

See comments in "[devices.py](#leads_vec.devices)".

## Shortcuts

| Key            | Usage              |
|----------------|--------------------|
| <kbd>1</kbd>   | Turn on / off DTCS |
| <kbd>2</kbd>   | Turn on / off ABS  |
| <kbd>3</kbd>   | Turn on / off EBI  |
| <kbd>4</kbd>   | Turn on / off ATBS |
| <kbd>T</kbd>   | Time lap           |
| <kbd>Esc</kbd> | Quit               |

## Debug

If you run into any error, first search in the existing [issues](https://github.com/ProjectNeura/LEADS/issues). If it
has not been encountered, please post a new issue.