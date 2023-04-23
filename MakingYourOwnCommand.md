# Making your own Command

[Back to Readme](/README.md)  
[Go to Commands Explained](/CommandsExplained.md)  
[Check the General Overview](/GeneralOverview.md)  

As explained in the Commands Page, there are two base types of commands we can inherit from:  

* GCSSyncCommand for synchronous commands  
* GCSAsyncCommand for asynchronous commands  

To make a new Command class in blueprints, Open the Add new Blueprint Class Widget and select one of our types:  

![Create](/Resources/Commands/MakeNewClass.JPG)  

Once Opened, configure your default settings  

![Settings](/Resources/Commands/DefaultSettings.JPG)  

The Main parts you'll want to customize are the `AddToSubsystemOnLoad`, `IsDevelopmentOnlyCommand`, the Command description/help text variables, the Non Localized Values and Command Tags for command identification, and your Allowed Execution settings!  

You can also customize Execution Messages for user-facing content.  

Now, implement your Execute Functionality (whether synchronous or asynchronous), and you're done!  

To check if your command has automatically registered during this engine session, go to the output log and type `gcs.listall`  

![List All](/Resources/ConsoleCommand/CommandListAll.JPG)  

and see if your command is there. If it wasn't added automatically, you can toggle `ToggleAddToSubsystemInEditor`. Keep in mind that for your game to load it you must either call the Register Command By Class Function in the subsystem or have it registered under Asset Manager Settings. [Visit How To Register your commands](/GeneralOverview.md#how-to-register-your-commands).  

The concept is the same in C++, though keep in mind that you'll have to recompile the project in order for the new C++ command to be considered by the system.  
