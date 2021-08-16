# steps

- `git clone --recursive https://github.com/nmpribeiro/computer_graphics_hw0.git`
- `cd computer_graphics_hw0`
- `mkdir build`
- `cd build`
- `cmake ..`
- `make -j`
- `./hw0`

## Cross platform support

It was only tested in MacOS. Should work in Linux. Windows is completely untested but shouldn't be hard.

My system already had GLM and GLUT, so I am not entirely sure you will need dependencies. If so, just add them by searching homebrew in MacOS or whatever package manager is available in your system.
