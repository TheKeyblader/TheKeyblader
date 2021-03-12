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
