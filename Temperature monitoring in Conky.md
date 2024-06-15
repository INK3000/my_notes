add to your conky file (for example:/home/ink/.config/conky/conky_shortcuts_live_maia)

```
${voffset 20}${goto 40}${color}${font Bitstream Vera Sans:bold:size=8}Temperature
${voffset 10}${goto 40}${color}${font Bitstream Vera Sans:size=8}CPU$alignr${hwmon 2 temp 1}°C
${goto 40}${color}${font Bitstream Vera Sans:size=8}GPU$alignr${exec nvidia-smi --query-gpu=temperature.gpu --format=csv,noheader}°C

```

to determine which `hwmon` directory corresponds to your CPU, you can follow these steps:

### Step 1: List All `hwmon` Directories

First, list all the `hwmon` directories:

```bash
ls /sys/class/hwmon/
```

This will display something like:
```
hwmon0  hwmon1  hwmon2
```
### Step 2: Identify the Device

For each `hwmon` directory, check the `name` file to identify the associated device.

#### Example Commands:
```bash
cat /sys/class/hwmon/hwmon0/name
cat /sys/class/hwmon/hwmon1/name
cat /sys/class/hwmon/hwmon2/name
```

### Step 3: Check for GPU-related Names

Look for names that indicate a GPU. Common names for NVIDIA GPUs might include "nvidia", while AMD GPUs might be indicated by "radeon" or "amdgpu". For example:
```bash
cat /sys/class/hwmon/hwmon0/name
```

Might return:
```
coretemp
```

This indicates it is related to the CPU.

[[Conky]]
[[Manjaro]]
[[Linux]]