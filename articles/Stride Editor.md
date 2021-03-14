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
