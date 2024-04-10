# Default Commands Explained


## Internal Commands  

The Plugin comes with a few default commands:  

* `/console`. Lets the user execute regular engine console commands, such as `stat unitgraph`.  
* `stop`. Lets the user stop any running commands. Has two modes:  
    * `stop` with no parameters will try cancelling ANY executing commands.  
    * `stop commandname commandname2` will try cancelling commands entered. For example, `stop /printdelay yourothercommand` will try to only stop `/printdelay` and `yourothercommand` should they be running.  
* `help` Lists information on the currently registered commands into the system. It has two modes:  
    * `help` with no parameters will list all the commands and the base description.  
    * `help commandname commandname2` will search for the commands entered, provide their description as well as the help tool tip. For example, `help stop /printdelay` would log two entries to the [Output log](/CommandOutputLog.md) and include the information in the execution result.    

For Settings information, check our Project Settings  

![](/Resources/DeveloperSettings.JPG)  

## Sample Commands

The plugin also comes with a few sample commands to get you started on familiarizing yourself with the framework.  

![](/Resources/ContentFolder-2.JPG)  

You can check their internals for information and/or just run the `help` command while in the editor/in game.  

## Widget Console Commands

We also added two commands for interacting with the [Widget System](/ConsoleWidget.md). 

* `exit` searches for any active console widgets and removes them from the viewport. See `GameCommandSystem/Content/Sample/GCS_Demo_ExitConsoleWidget`.  
* `clear` searches for any active console widgets and clear the entries. It can also intake either `output` or `log` to also clear the [Output Log](/CommandOutputLog.md) History. See `GameCommandSystem/Content/Sample/GCS_Demo_ClearConsoleWidget`.    