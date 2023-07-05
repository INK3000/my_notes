In command line:

```bash
cd <folder-that-is-in-PATH>
touch start_dbeaver.sh
```

In `start_dbeaver.sh` paste:

```bash
#!/bin/bash
cd /home/ink/Downloads/dbeaver/
nohup ./dbeaver > /dev/null 2>&1 &
```

In command line type:
```bash
chmod +x start_dbeaver.sh
```

that is all!

[[Linux]] 
[[CLI]]
[[Terminal]]
[[dmenu]]
