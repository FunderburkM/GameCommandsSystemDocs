# General Overview

[Back to Readme](/README.md)  

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

## Console Command Line

You can also run commands from the console command line.  

![Command Args](/Resources/ConsoleCommand/CommandListOverview.JPG)  

Executing a command:  

![Run Console](/Resources/ConsoleCommand/CommandRun.JPG)  

Listing all Commands:

![List Console](/Resources/ConsoleCommand/CommandListAll.JPG)  
