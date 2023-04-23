# Commands Explained

[Back to Readme](/README.md)  
[Check the General Overview](/GeneralOverview.md)  
[Try making your own Commands!](/MakingYourOwnCommand.md)  

## Table of Contents

* [Overview](#overview)  
* [Default Panel](#defaults)  
* [Execute Functions](#execute-functionality)  
* [Overridable Functions](#overridable-functions)  
* [Sync and Async Commands](#sync-and-async)

## BP exposure

Command have plenty of functions exposed to BP, and you can spot many of them here:  

![Img](/Resources/Commands/CommandQueryFunctions.JPG)  

## Overview

Let's use one of the demo commands as a way to explain the system. Take a look at Print Level command.  

![Print Level](/Resources/Commands/PrintLevelSample.JPG)  

First, at the top right, you see the class. There are two main branches of commands, `GCSSyncCommand` which provides synchronous command functionality, and `GCSAsyncCommand`, which in turn handles asynchronous commands.  

### Defaults

Let's go over some of the more important concepts here. All variables are thoroughly commented at the code level, so some may be skipped here.  

#### Developer Comments

Each Class branch counts with a fair number of Developer comments, providing insights of how things work and how the user can interact with them.  

#### Auto Registration

`Toggle Add To Subsystem In Editor`  
When working with your command in Editor, this boolean lets you register the command into our system so that your In-Editor and PIE sessions can have this command if it isn't already loaded during this work session. This Variable is editor only and does not get considered in standalone/packaged.  

`Add to Subsystem On Load`  
When this class (either by C++ or BP) gets loaded during our system's initiation period (check [Main Overview](/GeneralOverview.md#how-to-register-your-commands)), do we want to automatically register this command right away? If false, we expect this command to be loaded,if at all, manually.  

#### Registration

`Is Developer Only Command`  
This command will not register to our system if this is a shipping build.  

#### Admin

`Command Description` and `Command Help`  
Utility text to provide insights on this command, very useful when needing to provide user-facing messages like in a chat system or an in-game console window.  

`Non-localized Command Values`  
Array of Name variables that we recognize as a compatible string to execute this command. This can be overridden in the function `CheckForCompatibleCommand`.  

`Command Tags`  
Gameplay tag container that allows to provide meta information on what this command is, what category it belongs to, among other things. It's really important as you can then control command execution through our [Developer Setting's globally forbidden tags](/README.md#developer-settings) and in our [Execute Commands](/GeneralOverview.md#running-commands) for specifying specific tags, such as say that you want your dynamic chat system to only be able to run commands that you've tagged as `Chat Commands` when inputting user-based text.  

#### Execution

`Allowed Net Configurations To Run`  
Controls where this command can run at runtime. As it names suggests, whether you can run this on Client, Server, or everywhere.  

`Allowed Execution Configurations to Run`  
Controls where this command can runtime generally speaking. Is it an Editor-time only command? A PIE only command? A developer-only command? A Shipping only command? Etc.  

## Execute Functionality

In the example above of `Print Level` command, we simply run the operation of Getting the level name and printing it to string, then specifying our Execute Result with a prompt enum, string, and output message.  

![Execute](/Resources/Commands/CommandExecuteSync.JPG)  

## Overridable Functions

You have multiple options to override besides the execute function, such as after the command was executed, check for valid input (where say you want to verify the input arguments after /YourCommand), as well as what does this command even accepts as compatible strings.  

![Override Functions in BP](/Resources/Commands/OverridableFunctions.JPG)  

These are also available in C++ as well with more overriding functionality options.  

## Sync and Async

`Demo_PrintLevel` is a synchronous command, as you can see that the Execute function requires an Execution Result struct, and the command gets set as completed as soon as that execute function finishes (whether in C++ or Blueprint). To further this, Synchronous commands have no concept of being canceled or stopped or manually finished as they only exist within the scope of one function. This is very useful for single operations, but what about enabling latent functionality? That's where Asynchronous `GCSAsyncCommand` commands Come in.  

Let's take a look then at `Demo_PrintWithDelay`, a simple command that prints the input string multiple times over a repeating delay.  

![Print Delay](/Resources/Commands/PrintDelaySample.JPG)  

Here, the Execute Function is void, which means that it can continue running in the following ticks without halting the rest of the system's executions.  

You can then see the differences between each of them here, where we run

```text
gcs.run /printdelay hello delay!
gcs.run /print hello world
```

![PrintSyncAndAsync](/Resources/Commands/SyncPrintAndAsyncPrint.JPG)  
