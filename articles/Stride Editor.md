Before starting. I want to apologize for my English as is not my native language. I also used massively Grammarly
Hope you will understand everything.

# New Stride Editor

The purpose of this text is to present, my opinion on how we should create the new editor.

## First. What do we want?

We want an editor that helps with creating a stride game.
As normally, we should be able to create a game without the editor [#986](https://github.com/stride3d/stride/issues/986).
So now, what is painful to do without the editor.
The response is pretty simple: Stride specific files.

- An scene editor for scenes
- Material editor and previewer.
- Animation editor.
- Profiler
- Graphics compositor editor.
- UI Editor 
- Assets creation

### What we shouldn't put in the editor.

#### Scripting in editor.

I think coding inside a game editor is a newbie feature.
Most developers, Already use a full-featured ide (Visual Studio, VS Code, Rider, ...)
For me, scripting in the editor is interesting when you do fast prototyping or When the game engine uses a custom language 
or project system (.sln)

Anything that relies on editing non-specific stride files shouldn't be inside the game editor.

Ok cool, we define what we want, now.

## Second. How to implement it?

To develop our new editor, we need a UI framework.
so what we have?

### UI Framework

#### HTML based UI
- PRO
  - Very beautifull editor.

- CONS
  - Additional Builds steps and dependencies (NodeJS, npm).
  - Contributors need more knowledge (Mainly Rendering framework React, VueJS, ...).
  - Not everyone is a web developer.
#### C++
- PRO
  - Lots of proven solutions.
  - Fast
- CONS
  - C++ (Noob will not like but we can helper API in C#)

#### C#

So what we have in C#?
- [MAUI / Xamarin forms](https://github.com/dotnet/maui)
- [Avalonia](http://avaloniaui.net/)
- [Uno](https://platform.uno/)

This solution uses Xaml, supports all desktop platforms, and can support backend integration in Stride.
So they are the same, except for few things.

Avalonia uses a custom Xaml flavor.
MAUI uses a rendering method that needs more work for backend integration.

They can all work with MVVM libraries.
And they have all the same problem, I think Xaml styling is a lot more complicated than CSS.

To conclude, it to the community to decide what framework to choose.

### Extension

I love this part, I passed a lot of time to think how to make the best extension and I think I arrive at something very promising.

First what core of our editor?
To answer this question we take Visual Studio. What we have when we install visual studio with no workload.

- Windows creation project contains a template to create an empty solution
- Base project system (.sln)
- Github / Azure DevOps components.
- Text editor
- Empty Intellisense system.
- Most common window (Output, errors, terminal, etc...)
- Commands

Except for Github windows, any other part of Visual Studio can be extended.

In our case, we want the editor to edit stride projects so we can embed the stride project system.
Now let define the core of this new editor.

Core core components : 

- Stride Project System
  - Create, Edit, Load projects
  - Navigate throw assets API
- Windowing System.
  - Docking
  - Commands
  - Menu
  - Custom windows?
  - Events
  - Drag and drop
- Base property window (Like in visual studio)
 
 Core components (technically these components can be a plugin that will default developed):
 - Assets explorer
 - Scene editor + preview
 - Game preview
 - Stride UI editor
 - Material editor and previewer.
 - Animation editor.
 - Profiler
 - Graphics compositor editor.
 - Etc... anything that is needed all time.

#### UI Extension

Now let's see, how our users will extend UI.
We have 3 options.

First, writing XAML control like in any XAML framework. this option is the most low-level option and allows all possible customization.
Second, an attributes-based UI like [Odin](https://odininspector.com/) plugin in Unity. this option is the most hight level option and doesn't allow customization.
And finally, code that creates UI, like in  [Unity](https://docs.unity3d.com/Manual/editor-EditorWindows.html)
These three options can be all implemented at the same time.

### Code Reloading and Extension System.

One missing feature of the current editor is a game live preview. Adding code reload with solve partially this problem but I will explain why later.

But before enter to madness let see the current state of [assembly unload in .net core](https://docs.microsoft.com/en-us/dotnet/standard/assembly/unloadability)
Assembly unload has been implemented from .net core 3.0 with the capabilities from domain unload from .net framework or mono.
Except for one thing. domain unload is forced and AssemblyLoadContext is cooperative. so this puts some constraints on how the plugin should be designed.

#### Editor plugin.

First, we will talk about the editor plugin. this plugin is load from an editor like visual studio.
The first step is to define a plugin.

The plugin will be represented by two assemblies.
The first assembly will mostly contain POCO and Interfaces and represent data that will transit.
this assembly will allow other plugins to extend your plugin our use it.
And will even allow optional dependencies.

The second assembly contains the logic.
this assembly will expose a class that will implement interfaces from the first assembly to the editor.

For the implementation, the editor will need to find and load a plugin.
I will detail this in another discussion but basically, they will one AssemblyLoadContext by plugins and some nested AssemblyLoadContext.
So AssemblyLoadContext will allow to unload or update plugin at runtime.

To find plugins, we can simply use nuget.org and use some custom tags in nuspec.
That will resolve some problems like the editor version, stride version.
 
### Project plugin.

Project plugins are load from the project itself.
The code will the same that you will use in editor plugins but you will not need to define interfaces as this type of plugin is not intended to be extended.

Now, we have a good vision of the extensibility system. But I want to apologize for not giving any code or image to better explain everything but I will try to complete this documentation if people agree.

### Hot Reload

Now let's talk about scripts hot-reload.
Flax engine has written an [article](https://flaxengine.com/blog/flax-facts-16-scripts-hot-reload/) on how work they hot-reload.
To summarize, the editor saves game state,  unload game assembly, load new game assembly, restore game state.
Technically with AssemblyLoadContext, we can achieve the same. but some game scenarios may not work correctly.
Anything game that uses thread may not support hot reload as Assembly in .net core is cooperative.
AssemblyLoadContext can't force stop thread. any strong reference needs to dispose to allow assembly unload.

## Stride changes

For sure, to come to this editor alive, we need to touch stride code.
The only big issue is stride project loading that will need to revamp. 

## Conclusion

For sure, this documents doesn't cover all on how to create a new editor but I am sure it will be a help
I hope this wasn't too difficult to read.
I also think a new editor shouldn't be a prioritized project as we need to stabilize stride.
Like, resolve issues with no workaround.
Switch binding to silk.net 
Finish the transition to .net 5 and 6.
Refactoring extension code of stride.
