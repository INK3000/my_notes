1. Download from https://www.python.org/downloads/
2. Move archive to home directory 3.11 `mv Python-3.12.tar ~/`
4. Unpack it `tar -xvf Python-3-12.tar`
5. `cd Python-3-12`
6. `./configure --prefix=/home/ink/.python3.12 --enable-optimizations `
7. `make -j4`  # j4 is cores quantity
8. `sudo make altinstall`
9. add to .zshrc `export PATH=/home/ink/.python3.12/bin:$PATH`
10. `source ~/.zshrc` or `omz reload` 
11. `python3.12` to start python

[[Linux]]