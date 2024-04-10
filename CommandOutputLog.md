# Command Output Log

With 1.1, we've added a convenient log dedicated to the commands system! It operates separate to the engine's output log, and can be used to display, register, and categorize information. It's also what we use for the [Console Widget System](/ConsoleWidget.md).  

![](/Resources/Widgets/ConsoleOverview.jpg)  

## Usage

There are five main functions in question:  

![](/Resources/BlueprintGraph/Graph_OutputLog.jpg)  

* Register To Output Log Event: This registers a listener callback for any responses to `AddOutputStructToLog`.  
* Unregister From Output Log Events: This unregisters any previously registered event from `RegisterToOutputLogEvents`.  
* Get Output Log: Returns all the entries in the output log information.  
* Clear Output Log: Removes all current entries in the output log collection.  
* Add Output Struct To Log: Creates a new log entry. Running this function will add an entry to the collection and broadcast for listeners regisstered via `RegisterToOutputLogEvents`.  

Here's an example of it being used in the [Demo command](/DefaultCommands.md#sample-commands) `/printdelay`  

![](/Resources/Commands/PrintDelaySample.JPG)  

and in C++, an example from our [Default command](/DefaultCommands.md#internal-commands) `help`:  

![](/Resources/Commands/outputlogcall.jpg)  

## Log Cleanup

For Projects working in the editor, Logs can be ran at both runtime and editor time, just like the rest of the system. Likewise, Commands ran during PIE sessions will be cleared at the end of PIE to keep the clutter down. However, in packaged builds, they will persist for the entire runtime session.  

For information on how we use this for console widget functionality, check the [Console Widget System](/ConsoleWidget.md) page. 


## Settings

For Settings information, check our Project Settings, such as timestamp permissions and rich text configurations for different output types.    

![](/Resources/DeveloperSettings.JPG)  