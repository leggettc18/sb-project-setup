# Silverblue Project Setup

This script is intended for users of Fedora Silverblue and VS Code to assist in the container based workflows.

Say you run the script with the following arguments:

```
sb-project-setup -n test -p .
```

This will do the following:

1. Create a toolbox called "test"
2. Create a new directory in the current directory called "test" (if you provide a different path it will put the directory there instead. Think of it as a prefix.)
3. Create a file called `test.code-workspace` in the directory provided by the `-p` argument (the current directory, in this case), with the following contents.

```
{
    "folders": [
        {
            "path": "test"
        }
    ],         
    "settings": 
    {
        "terminal.integrated.shellArgs.linux": ["-c", "flatpak-spawn --host toolbox enter -c test"]}
    }
```

This will set it up so that if you open that file as a workspace with the Visual Studio Code flatpak, it will open up in your project's directory, and the built-in terminal will automatically load up a bash prompt inside a toolbox created specifically for your project. You will need to launch the terminal first and click allow at the prompt before this will happen though. After that if you kill the terminal and start a new one it will launch the toolbox.

## Known Issues

1. The terminal opens up in the home directory of your toolbox. This is because VS Code tries to automatically open the terminal up to `/var/home` which does not exist in the toolbox, so it defaults to the home directory. One possible workaround is to apply workspace settings in VS Code to set the starting directory to your project's directory starting with `/home` instead of `/var/home`. I may be able to fix this at some point, but if you have a solution ready feel free to open a pull request.
2. If the code-workspace file already exists, the script will not edit it currently. This is being worked on, but for now you could just copy and past the `"settings"` example above into your code-workspace file, substituting your project name in for `test`.
3. This currently does not work in the OSS version of Visual Studio Code flatpak. This appears to be a problem with the flatpak specifically. You will get the following error: `Portal call failed: org.freedesktop.DBus.Error.ServiceUnknown`. I do not have a workaround for this at the current time.

If you have a solution to any of these, feel free to open a pull request!