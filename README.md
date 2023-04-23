# Advanced Game Commands Docs

Documentation for the Game Commands System Plugin! Available for 4.27, and Unreal Engine 5!  

[Unreal Marketplace Profile](https://www.unrealengine.com/marketplace/en-US/profile/M+Funderburk).  
[Discord Server](https://discord.gg/QHTTMQ6Pqw).  

![Thumbnail](/Resources/MainImage.png)  

## Table of Contents

* [Index](#index)  
* [Enabling the Plugin](#enabling-the-plugin)  
* [Disabling the Plugin](#disabling-the-plugin)  
* [Developer Settings](#developer-settings)  
* [Content Folder](#sample-content)  

## Index

* [Change Log](/ChangeLog.md)  
* [General Overview](/GeneralOverview.md)  
* [Explaining Commands](/CommandsExplained.md)  
* [Making your first Command!](/MakingYourOwnCommand.md)  

## Enabling the Plugin

Once you've downloaded the plugin, head to the Plugins section, and in the Installed section, Enable `Game Commands System`. You'll have to restart the project for it to be loaded.  

![Plugin](/Resources/EnablePlugin.jpg)  

## Disabling the Plugin

Either by disabling it like in the image above, or modifying your `.uproject` file (and/or by removing it from your Project/Plugins if you mounted it there), once you have disabled the plugin, there is one more step action needed.  

Head to your Project Settings / Asset Manager configuration panel, search for the entry called `GCSCommand`, and delete it.  

![Asset Settings](/Resources/AssetManagerSettings.JPG)  

## Developer Settings

If you go to Project Settings / Game Commands System, you can see the following options:  

![Settings](/Resources/DeveloperSettings.JPG)  

The Failure messages sections allow you to customize some of the failure request/execution messages when running commands. Forbidden Command Tags will be explained in another section, but as it suggests, it lets you block global tags for commands to not run when requested to.  

## Sample Content

You can find the plugin's content by going to `Game Command System Content` from the All content show section. If you can't see it after you've enabled the plugin, check the Settings button in the content browser and enable Show Engine/Plugin content (Show Engine content if the plugin is installed in your Engine, otherwise Plugin Content if the plugin is in YourProject/Plugins/GameCommandSystem).  

![Content](/Resources/FindContentFolder.JPG)  

In it, you'll find the following:  

![First Folder](/Resources/ContentFolder-1.JPG)  

Which contains the sample folder and the blueprint with all the available nodes exposed to BP!  

![Second Folder](/Resources/ContentFolder-2.JPG)  

Next layer in, we have the demo commands included with the Plugin, and The folder for Test materials!  

![Third Folder](/Resources/ContentFolder-3.JPG)  

The Test Level for trying out some of our commands!  
