# Linuxcnc 2.8 Config for the 4590 Router

## System Setup

Add the user to the dialout group:

```
sudo usermod -a -G dialout $USER
```

Install the dialout rules for the STM32 Virtual Com Port (VCP):

```
sudo cp 99-stm32-vcp.rules /etc/udev/rules.d/
```

## Make the screen not go dark

append this to `~/.bashrc`:

```
export DISPLAY=:0.0
xset s off
xset s noblank
xset dpms 0 0 0
xset -dpms s off
```

## Auto login

Edit `/etc/lightdm/lightdm.conf`:

```
# in this section
[Seat:*]
# ... set:
autologin-user=cnc
autologin-user-timeout=0
```



## There are 2 custom components

- `mpgbtns.comp`, a realtime component, reads the mpg and buttons from the extra parallel port

Install the MPG component prior to running with: `sudo halcompile --install mpgbtns.comp`

You'll see this on success:

```
Compiling realtime mpgbtns.c
Linking mpgbtns.so
cp mpgbtns.so /usr/lib/linuxcnc/modules/
```

- `lcdbtns.cmp`, a userspace component, reads the buttons, pots from the button panel around the display an outputs the current pendant state


Install the LCD Buttons user space component with: `sudo halcompile --install lcdbtns.comp`

You'll see this on success:

```
gcc -I/usr/include -I/usr/include/linuxcnc -URTAPI -U__MODULE__ -DULAPI -Os  -o lcdbtns /tmp/tmp0VRVT7/lcdbtns.c -Wl,-rpath,/lib -L/lib -llinuxcnchal
```
