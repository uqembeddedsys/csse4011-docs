# Setting up MicroPython

This tutorial covers how to install MicroPython on the B-L475E-IOT01A board. It also provides instructions on how to flash the board too, along with key files on an Ubuntu machine.

## Step 0. Install the necessary libraries

The assumption this guide makes is that the arm-gcc-tool chain, stlink-tools, and Python3 are already installed. The other libraries that need to be installed are as followed as they handle the additional dependencies of MicroPython.

1. `sudo apt-get install libffi-dev`

2. `sudo apt-get install pkg-config`

## Step 1. Building MicroPython Repository

Clone repository as usual.

```bash
git clone https://github.com/micropython/micropython.git
```

This is then followed by building mpy-cross folder. This allows the Python code to be cross-compiled into a .mpy file. Run this in the root of the repository.

```bash
make -C mpy-cross
```

## Step 2. Building the STM32 MicroPython Port

Once Step 1 has been completed, you should be ready to build the STM32 port of MicroPython. Go to the STM32 port folder located in `ports/stm32`.

```bash
# Assuming root directory
cd ports/stm32
```

We then build the submodules that the STM32 library requires.

```bash
make submodules
```

We then build MicroPython to be compatible with our board.

```bash
make BOARD=B_L475E_IOT01A
```

Finally we flash the board using ST-LINK. This would update firmware.

```bash
make BOARD=B_L475E_IOT01A deploy-stlink
```

## Step 3. Testing the connection

To test/verify if the board has been flashed, connect to the board serially. This can be done with various software such as PuTTY; all that is needed is a baud rate of 115200.

```bash
/bin/screen /dev/ttyACM0 115200
```

Pressing enter or CTRL-C should bring out the familiar command line interface of Python. You should then be able to run typical Python code normally.

## Step 4. Flashing your first application

Flashing the board is simply a matter of using the `pyboard.py` file located in the `tools` folder. Go to the `tools` folder and create a `main.py` file. Using this code snippet below.

```python
# Machine is a MicroPython library.
from machine import Pin
import time

# Get PB14 and treat it as a GPIO Output pin
led_pin = Pin('PB14', Pin.OUT)
value = 1

# Toggle the pin every one second
while 1:
    led_pin.value(value)
    if value == 1:
        value = 0
    else:
        value = 1

    # Somewhat similar to HAL_Delay()
    time.sleep(1)
```

Flashing the board is a matter of calling the bash commands below inside the `tools` folder.

```bash
# Think of it as running the cp command as with most Linux systems. You are copying the main.py file and placing it inside the board with the same name. Device name is whatever appears on your machine.
python3 pyboard.py --device /dev/ttyACM0 -f cp main.py :main.py
```

Assuming everything follows, you should have a flashing LED that toggles every second.

## Notes

## Making development easier. Symlinks and env variables

Typing `python3 pyboard.py` is a bit cumbersome. We can make this easier by creating a symlink. In the tools folder, run the following.

```bash
# Replace /path/to to where the micropython repository lies
sudo ln -s /path/to/micropython/tools/pyboard.py /usr/bin/pyboard
```

The following command should then run.

```bash
pyboard --device /dev/ttyACM0 -f ls
```

Of course, naming the device is a bit tiring. If you interact with only one board, then we can set the appropiate environment variable.

```bash
# Assumes our board is located here.
export PYBOARD_DEVICE=/dev/ttyACM0
```

### Location and content of onboard files

You won't be able to see the contents inside the MCU using a typical File Explorer. Copying and pasting code like that would most likely not work. `pyboard` has commands that allows you to see what is stored there.

```bash
pyboard  --device /dev/ttyACM0 -f ls
```

Similarly we can see the content of what the `main.py` is using `cat`

```bash
pyboard --device /dev/ttyACM0 -f cat main.py
```
