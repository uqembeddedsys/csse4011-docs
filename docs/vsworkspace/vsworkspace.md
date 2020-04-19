# Setting up vs-code

## Create a workspace

In vs-code go to File -> Open Folder and select the ei-freertos folder.

The folder should be visible in the Explorer section of vs-code.

File -> Save Workspace As -> name the workspace. e.g. csse4011lib.code-workspace

Note, you may create a workspace under ei-freertos or under your code folder, by default the configuration tools are set up for ei-freertos to be the workspace folder, however you can easily change this by modifying the generate_cpp_properties.py file. 

Now, whenever you open up vscode, open this workspace file to resume where you left off.

## Install the C/C++ extension

You will need this for IntelliSense (autocompletion) and browsing your code. Go to the extensions tab, search for C/C++ and install it. Restart vscode.

## Create workspace settings

```
make vscode
```
In vscode, under explorer, click the refresh explorer button, a new folder should appear under the root folder called .vscode. This folder contains a settings file that modifies some vscode behaviour as well as a properties file which contains the folders where your code should look for files.
