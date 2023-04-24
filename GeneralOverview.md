# General Overview

[Back to Readme](/README.md)  
[Commands Explained](/CommandsExplained.md)  

## Table of Contents

* [Getting The subsystem](#getting-the-subsystem)  
* [Registering Commands](#how-to-register-your-commands)  
* [Querying Commands](#querying-commands)  
* [Running Commands](#running-commands)  
* [Gameplay Tag Interactions](#command-tags)  
* [Console Command Support](#console-command-line)  

Our Command System is powered by an `UEngineSubsystem`, which is present throughout the vast majority of the engine's lifetime, all from the point the main Engine Class `UEngine` initiates till it shuts down. This object keeps and manages all of our command objects.  

In this page, we will go over how we can automatically load commands on initiation or on demand by game code, execute and manage commands from game code, and a small peek at the console commandline support.  

## Getting the subsystem

In Blueprints, you can get it this way:  
![BP Getter](/Resources/BlueprintGraph/SubsystemGetter.jpg)  
In C++, you can do:  

```cpp
    UGCSCommandSubsystem* Subsystem = UGCSCommandSubsystem::Get();
    //or
    UGCSCommandSubsystem* Subsystem = GEngine->GetSubsystem<UGCSCommandSubsystem>();
```

Keep in mind that the execution functions do not need to fetch the system in Blueprints, but do need it in C++.  

## How to Register your Commands

We go over some of the command's deeper intrinsics [here](/CommandsExplained.md), but commands can get loaded to our system by default. C++ classes get considered by default, such as the sample ++ command, `UGCSConsoleCommand`. Blueprint classes can get auto loaded based on `Asset Manager Settings`. Blueprint Command Classes Load Asynchronously.  

![Asset Settings](/Resources/AssetManagerSettings.JPG)  

You can add your paths and even specific assets to the manager rules for the commands you want to load automatically on engine startup.  

> One thing to note is that depending on how many references blueprints your commands require to load, the system may not be ready to start running commands on the first beginplay. If you try to execute a command while the command system is still loading assets, it'll give you a `still loading` error. If you want to make sure your first command runs only after the system is ready to load, check the following:  
![Loading](/Resources/BlueprintGraph/SubsystemLoadingDelegate.JPG)  

You can also register them on demand manually, say that you want to load all your In-Game Chat commands only when the chat widget has to first get spawned - you can register them here:  

![Registering](/Resources/BlueprintGraph/SubsystemAddRemoveCommands.JPG)  

**Notes on Registration**:  

You can also register types in editor as they get created/compiled they will be automatically registered, but please keep in mind that your standalone/packaged game will only auto add commands from the asset manager rules.  

## Querying Commands

We also count with several querying functions for finding the specific commands should you need to access their read data.  

![Querying](/Resources/BlueprintGraph/SubsystemQueryCommands.JPG)  

## Running Commands

You can totally run your commands from code!  

![Running In BP Graph](/Resources/BlueprintGraph/ExecutingAndStoppingCommands.JPG)  

Optional Must Have Tags allow you to filter through commands. Say that you have `GameCommands.Chat` commands. By Adding that tag this gameplay tag container, you ensure that any string input that matches a command must also be tagged as a `GameCommands.Chat` in order for it to run! [More Information here](/CommandsExplained.md#admin).  

The `Execute Game Command` functions are static and can be ran from anywhere! You also get plenty of options for stopping game commands, though keep note that the only game commands that can be stopped in practical terms are asynchronous commands.  

In C++, it'd be a similar case:  

```cpp

//You can build a gameplay tag container and then just &MyTagContainer otherwise
//  just nullptr. GCS_DEMOCOMMANDS is a native tag included with the plugin.
FGameplayTagContainer* OptionalTagsToHave = &FGameplayTagContainer(GCS_DEMOCOMMANDS);

//Make sure that MyListeningFunction's signature matches the dynamic delegate, 
//  void(UGCSCommandBase*, const FGCSCommandExecuteRecord&)
FGCSOnCommandExecutedSingle OptionalListenerBinding;
OptionalListenerBinding.BindUObject(this, &ThisClass::MyListeningFunction);

UGCSCommandBase* OutCommand = nullptr;
FGCsCommandExecuteResult Exec = UGCSCommandSubsystem::Get()->ExecuteGameCommand(
    OptionalContextObject,
    TEXT("/print hello world"),
    OptionalListenerBinding,
    OptionalTagsToHave,
    OutCommand
);

//If you don't care about the optional elements nor binding listening, here's how that looks:
UGCSCommandBase* OutCommand = nullptr;
FGCsCommandExecuteResult Exec = UGCSCommandSubsystem::Get()->ExecuteGameCommand(
    nullptr,
    TEXT("/print hello world"),
    {},
    nullptr,
    OutCommand
);

```

Command Execution with `Optional Context Object` is important depending on the command, for more information, check [this page](/CommandsExplained.md).  

## Command Tags

Commands have a Gameplay Tag Container to help categorize and identify _types_ of commands. This gameplay tag is used in multiple ways:  

* As overviewed in the images above, you also Query Commands By Tag, Stop Commands by Tag.  
* When Executing commands, you can also add gameplay tag execution rules, so running `/invite PlayerName` input as a string can only be ran if the found command has a Chat gameplay tag.  
* Project Setting Forbidden Tags: [Overviewed here](/README.md#developer-settings) can provide project-wide tags to block from being able to execute. This is a simple way to control which commands can be disabled.  
* Subsystem Blocked Tags: In addition to Project Forbidden tags, you can add and remove blocked tags as needed. Say that party invite chat commands are only available in specific levels, you can use these functions to control that.  
![BlockedTags](/Resources/BlueprintGraph/DynamicallyBlockedTags.JPG)  

## Console Command Line

You can also run commands from the console command line.  

![Command Args](/Resources/ConsoleCommand/CommandListOverview.JPG)  

> Correction: GCS.Run's description is incorrect, and can execute any type of registered command from the console, not just synchronous commands. You can see this in action in the image below.  

Executing a command:  

![Run Console](/Resources/ConsoleCommand/CommandRun.JPG)  

Listing all Commands:

![List Console](/Resources/ConsoleCommand/CommandListAll.JPG)  
