# User Manual

:::{warning}
This user manual assumes that the proper environment has been set and the necessary dependencies have been installed.
Improper use of this program may damage your device.
:::

## Configuration

```json
{
  "refresh_rate": 60,
  "manual_mode": true,
  "gps_receiver_port": "/dev/ttyAMA0",
  "wheel_speed_controller_port": "/dev/ttyAMA1",
  "power_controller_port": "/dev/ttyAMA2"
}
```

### Determine Devices' Ports

The ports may differ due to various drivers. You can determine the port using the described procedure.

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

## Start the Program

```shell
python-leads -m leads_vec -c /usr/local/leads/config.json --xws run
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
| <kbd>esc</kbd> | Quit               |
