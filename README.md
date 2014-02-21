#Hot Key Sequences (HKS)

##Description

HKS is a utility that allows for arbitrary command(s) to be executed from a series of shortcut keys from anywhere inside the OS GUI.  The way it works is the HKS application is launched (ideally) using a keyboard shortcut, then HKS waits for a key or series of keys to be pressed and executes the action you have predefined for those keys.  For example, Vim could be started by pressing `<Windows>+<o>` then `<v>`.  And you could also start your favorite browser by pressing `<Windows>+<o>` then `<i>`.  `<v>` and `<i>` correspond to bash scripts named "v" and "i" in an HKS directory (see installation section below).  HKS will automatically choose the first matching command.  So if you have defined actions for `<i>` and `<ii>`, you will never be able to execute `<ii>` because `<i>` will always takes precedence.

###Features

* HKS reads commands from files placed in the folder `~/hks/myfolder/`, where `myfolder` is one of `{action, app, win, file, misc, copy, paste}`, so new commands can be added by simply adding files/folders.
* Persistent clipboard invoking `hks -a copy` and `hks -a paste`.  BEWARE: anything copied is saved to disk, so don't copy any sensitive information using `hks -a copy`!!
* Ability to limit functionality to specific applications.
* Spacebar closes HKS window.
* Backspace deletes last character.

##Installation

Copy the `hks` script to your executable path (or execute it via `/path/to/hks`).  Note: HKS has been tested on only CentOS 6.4+.

Make sure you have the following:
* `bash`
* `konsole` or `gnome-terminal`
* `stty`
* `dd` (for reading keyboard input)
* `xdotool` (optional, needed for certain features)
* `wmctrl` (now optional)

```bash
cd /desired/path/
git clone https://github.com/danbarrese/hks
mkdir -p ~/hks/{action,app,copy,file,misc,paste,win}
echo 'gnome-keybinding-properties' > ~/hks/app/ks
echo 'Keyboard Shortcuts' > ~/hks/app/.ks
cd /desired/path/hks
./hks -a app
echo 'Hello from HKS!' > ~/hkstest.txt
echo 'gvim ~/hkstest.txt' > ~/hks/file/test
./hks -a file
```

Set a system-wide keyboard shortcut to execute `hks -a app`, assuming `hks` is in your executable path.  In Gnome 2, use the `gnome-keybinding-properties` app.

##Add New Commands

```bash
cd /desired/path/hks
cd app  #or any of the other folders (e.g. action, win, file, etc.)
echo 'wmctrl -c :ACTIVE:' > /desired/path/hks/win/q
echo 'Quit active window' > /desired/path/hks/win/.q
```

##Use Application-Specific Commands

There are 2 ways.

The first is to use the `-w` option, which MUST come BEFORE the `-a` option for which it should affect.  To limit the command to the `konsole` application, use: `hks -w konsole -a app`.  To limit to `gvim`, use: `hks -w gvim -a app`.  And so on.

The second way is to use the `-a action` option.  `-a action` will get the name of the current application (I'll use "gvim" as an example) and look in the `~/hks/action/gvim/` folder for any commands.  If the `~/hks/action/gvim/` doesn't exist, it will automatically be created.  If no commands exist in the `~/hks/action/gvim/` folder, the HKS application will just quit.

## Update Log
* 2013-07-06: Created.
* 2013-08-24: Added persistent clipboard feature.
* 2014.02.21: Added support for gnome-terminal and backspace input.  Removed dependency on wmctrl (script will use it if available).
* 2014.02.21: Removed dependency on xdotool (script will use it if available).
