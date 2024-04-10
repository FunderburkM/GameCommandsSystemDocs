# Console Widget Support

We now have a widget system available for runtime, packaged, and shipping builds and systems. This also gives the developer a starting point for implementation if they wanted to do an implementation with say a god mode system, mod system, or even just chat commands!  

![](/Resources/Widgets/ConsoleOverview.jpg)  

To test this, when in the LVL_GCS_Test level, you can open the widget with `tab`.  

![](/Resources/Widgets/LevelToggle.jpg)

## Settings


For Settings information, check our Project Settings  

![](/Resources/DeveloperSettings.JPG)  

Here, we establish what commands are allowed to be used by the console widget system. For information on the tag system, check [Overview Page](/CommandsExplained.md#commands-explained) and [Tag Overview](/GeneralOverview.md#command-tags).  

## Rich Text System

As in the image above, you see that we have a ` Rich Text Clauses ` map that lets the user specify if the output should specify any tags when formatted via  

![](/Resources/BlueprintGraph/Graph_FormatOutputLog.jpg)  

These get referenced via the Rich Text Data Table. Our Default widget uses the one located in our Widgets Folder and applied to our Rich Text Block widgets.  

![](/Resources/Widgets/WidgetsFolder.jpg)  

Here are a couple of tutorials regarding Rich Text:  

* [Official Documentation](https://dev.epicgames.com/documentation/en-us/unreal-engine/umg-rich-text-blocks-in-unreal-engine)  
* [Youtube Video](https://www.youtube.com/watch?v=9M4rjznF7Ys)  

## Widgets

The Main widget is `W_GCS_Console`. This manages the `W_GCS_CommandList` [which in turn manages the `W_GCS_CommandListEntry` per the hint system, explained below] and the `W_GCS_ConsoleEntry` which gets populated per the Output Log explained [here](/CommandOutputLog.md).  

![](/Resources/Widgets/Graph_Widget%20OutputLog.jpg)  

We utilize a Text Box that allows the user/developer enter text. On commit, we analyze it and execute commands.  

![](/Resources/Widgets/ChatEntryBoxOverview.jpg)  

In conjunction with `W_GCS_CommandList`, we have a tooltip widget that gives the auto complete / hint effect.  

![](/Resources/Widgets/Graph_UpdateTooltip.jpg)  

The tooltip command list gets generated in response:  

![](/Resources/Widgets/Widgets_CreateTooltips.jpg)  