# VLC Volume Control Script

This script enables you to control the volume of VLC Media Player using DBus, automating the process of increasing the volume by 5%, with a maximum limit of 100%. It is designed to work with sxhkd, allowing you to control VLC's volume directly from your keyboard using custom keybindings.

## Features:

- **Increase Volume**: The script increases VLC's volume by 5% every time it’s executed.
- **Volume Cap**: If the volume exceeds 100%, it is capped at 100%.
- **Hotkeys**: Integrate with `sxhkd` to control the volume using custom keybindings.

## Versions of Used Software:
The following versions were used during the development and testing of this script:

- **VLC Version**: 3.0.21 Vetinari (revision 3.0.21-0-gdd8bfdbabe8)
- **sxhkd Version**: 0.6.2
- **DBus Version**: 1.16.0
- **xdotool Version**: 3.20211022.1
- **bc Version**: 1.07.1
- **Kernel Version**: 6.12.7-arch1-1
- **dwm Version**: dwm-6.5

## Prerequisites:
- **VLC**: Make sure VLC is running with DBus support enabled.
- **DBus**: This script interacts with VLC via the DBus interface.
- **`xdotool`**: Used for simulating key presses in VLC.
- **`sxhkd`**: Hotkey daemon to trigger actions using custom keyboard shortcuts.
- **`bc`**: Used for basic calculations to increase the volume.

## Installation:

1. **Install Dependencies**:
    Ensure that the following packages are installed on your system:

    - `sxhkd` (for managing hotkeys)
    - `xdotool` (for simulating key presses)
    - `bc` (for basic arithmetic operations)

    You can install them using your package manager, for example:

    - For Ubuntu/Debian-based systems:
    ```bash
    sudo apt install sxhkd xdotool bc
    ```

    - For Arch Linux:
    ```bash
    sudo pacman -S sxhkd xdotool bc
    ```

2. **Download and Set Up the Script**:
    Download the `vlc-volume-control.sh` script and place it in a directory of your choice, e.g., `$HOME/.local/bin/`.

    Here's a sample command to make the script executable:
    ```bash
    chmod +x $HOME/.local/bin/vlc-volume-control.sh
    ```

3. **Add the Hotkeys**:
    Add the following bindings to your `sxhkdrc` file (usually located in `~/.config/sxhkd/sxhkdrc`):
    ```bash
    Next
        xdotool search --class "vlc" key space

    Home
        $HOME/.local/bin/vlc-volume-control.sh

    Insert
        xdotool search --class "vlc" key Down

    End
        xdotool search --class "vlc" key n

    Delete
        xdotool search --class "vlc" key p
    ```

4. **Reload `sxhkd`**:
    After modifying the `sxhkdrc` file, reload `sxhkd`:
    ```bash
    pkill -USR1 -x sxhkd
    ```

5. **Test the Script**:
    Test the script independently by running:
    ```bash
    $HOME/.local/bin/vlc-volume-control.sh
    ```

    This should print the current volume, increase it by 5%, and adjust the volume if it exceeds 100%.

6. **Test the Hotkey**:
    Press the `Home` key (or the key you configured in `sxhkd`) to trigger the volume increase.

---

## Troubleshooting:

If the script isn't working as expected, try the following:

1. **Verify VLC and DBus Connection**:
    Check if VLC is running and has DBus support. Run the following command to check the current volume of VLC:
    ```bash
    dbus-send --print-reply --dest=org.mpris.MediaPlayer2.vlc \
    /org/mpris/MediaPlayer2 org.freedesktop.DBus.Properties.Get \
    string:"org.mpris.MediaPlayer2.Player" string:"Volume"
    ```

    If you get a response with the volume level, then DBus is working properly.

2. **Ensure `xdotool` is Able to Interact with VLC**:
    Verify that `xdotool` can find the VLC window. Run:
    ```bash
    xdotool search --class "vlc"
    ```

    It should return a window ID. If it doesn’t, make sure VLC is running and check the class name of the window.

3. **Debug the Script**:
    Add debugging output to the script to see what’s going wrong:
    ```bash
    echo "Current VLC volume: $current_volume"
    echo "New VLC volume after 5% increase: $new_volume"
    ```

4. **Verify `sxhkd` is Running**:
    Ensure that `sxhkd` is running and the configuration file (`sxhkdrc`) is being loaded properly. You can reload `sxhkd` after changes:
    ```bash
    pkill -USR1 -x sxhkd
    ```

---

## Notes:
- This script increases the VLC volume by 5% on each execution. If the volume exceeds 100%, it will be capped at 100%.
- The script interacts with VLC through DBus, so VLC must be running with DBus support enabled.
- You can customize the hotkeys for VLC control in the `sxhkdrc` file.

---

## Example Execution:

Here's an example of how the script works when executed:

```bash
### First run:
$ manage_vlc_volume.sh
Current VLC volume: 0.75
New VLC volume after 5% increase: 0.80
method return time=1735785386.948852 sender=:1.2515 -> destination=:1.2762 serial=548 reply_serial=2
VLC volume set to 0.80.

### Second run:
$ manage_vlc_volume.sh
Current VLC volume: 0.800003
New VLC volume after 5% increase: 0.850003
method return time=1735785391.188227 sender=:1.2515 -> destination=:1.2764 serial=551 reply_serial=2
VLC volume set to 0.850003.
