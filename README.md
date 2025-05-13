# DearPyGui-RaspberryPI  
Hi, I made this repository to assist those trying to install DearPyGui version 2.0.0 on their Raspberry PI but facing some significant hurdles. The solution lies in combing through a few issues on their official repository page. To make it more straightforward, I created a repo because I can't add changes to their wiki page. 

# Setup Instructions  

### Packages
I was trying to install the following packages in a separate environment on my RPi. 
```
dearpygui==2.0.0
numpy==2.2.4
pandas==2.2.3
python-dateutil==2.9.0.post0
pytz==2025.2
six==1.17.0
tzdata==2025.2
watchdog==6.0.0
```

Most of the packages should be able to install easily with the command `pip install <package-name>==<version>` but dearpygui package will cause issues as it is not available on PyPI (Python Package Index).
After countless hours of believing in the impossible and going through the issues posted on the [official GitHub respository](https://github.com/hoffstadt/DearPyGui), below is a possible fix.

#### Relevant Issues

[# Issue 1](https://github.com/hoffstadt/DearPyGui/wiki/Local-Wheel)  
From here, we learn that `pip install dearpygui` was never meant to work on RPi and we need to use local build wheels.

Even the [official wiki page](https://github.com/hoffstadt/DearPyGui/wiki/Local-Wheel) on generating local build wheels doesn't work smoothly and gives errors.
The correct sequence of commands are:

```
$ sudo apt update && sudo apt upgrade -y
$ sudo apt install -y \
cmake \
libglu1-mesa-dev \
libgl1-mesa-dev \
libxrandr-dev \
libxinerama-dev \
libxcursor-dev \
libxi-dev

$ git clone --recursive https://github.com/hoffstadt/DearPyGui && cd DearPyGui

$ python3 -m setup bdist_wheel --dist-dir ../dist
```

So, this should make a new directory containing the wheel file. In my case, it was named: `dearpygui-2.0.0-cp311-cp311-linux_aarch64.whl`.
With the environment active, run this command to install the dearpygui package:

```
pip install ./dist/dearpygui-2.0.0-cp311-cp311-linux_aarch64.whl --force-reinstall
```

You can confirm whether this package has been installed correctly or not with:

```
pip show dearpygui
```

# Running the code

[# Issue 2](https://github.com/hoffstadt/DearPyGui/issues/2342)

Now it may be possible to see errors like:

```
ModuleNotFoundError: No module named 'dearpygui._dearpygui'
```

To fix this, try shutting down and restarting the RPi.
