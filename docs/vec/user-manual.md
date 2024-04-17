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

## Start the Program

```shell
cd /usr/local/leads
python3.12 -m leads_vec -c config.json --xws run
```

## Fault Lights

See comments in ["devices.py"](#leads_vec.devices).

## Shortcuts

| Key            | Usage              |
|----------------|--------------------|
| <kbd>1</kbd>   | Turn on / off DTCS |
| <kbd>2</kbd>   | Turn on / off ABS  |
| <kbd>3</kbd>   | Turn on / off EBI  |
| <kbd>4</kbd>   | Turn on / off ATBS |
| <kbd>T</kbd>   | Time lap           |
| <kbd>esc</kbd> | Quit               |
