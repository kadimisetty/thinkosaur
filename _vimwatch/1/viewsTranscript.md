# Episode 1 - Views - DONE
## Introduction
Imagine having to setup your workspace every time you return to your project; won’t that be such a hassle?

Let’s begin this series by exploring Views - the first of what  Vim has to offer in this regard.



## Views
### Definition
In Vim’s parlance, A _View_ is a collection of settings local to a single window; settings such as whether to display line numbers etc.


### Basic Usage
The `:mkview` command __creates__ a view that saves the settings of the current window and the `:loadview` command loads those saved settings _right_ back onto the window. 

With those two commands, Vim makes fiddling with window settings quick and harmless.

### Slots
You get 10 views to play with. The first - the __Unnamed View__, is what’s used by the mkview and loadview commands. 

The rest of them can be accessed by appending those two commands with a number between 1 to 9.


### Uses
Apart from their obvious use to restore a window’s environment after reopenings; I sometimes employ them to create and switch between tiny focussed states.

For example - 
%% DEMO: Open story.markdown It should have a spelling error.
%% :setlocal spell spell
%% :setlocal spelllang=en_gb
%% :setlocal spellsuggest=double

1. I’ll set out to make a view that focusses on checking this text for spelling mistakes.
2. `:mkview` backs up my current settings to the Unnamed View.
3. `:setlocal` locally applies a few _spellcheck related_ settings relevant to this particular text.
4. These spellcheck focussed settings can be captured into _View Number 1_ with the command `:mkview 1`.
5. `:loadview` switches the window back to it’s previous state and `:loadview 1` brings back the highlights on spelling mistakes. With the appropriate loadview commad I get to toggle between these two views.
6. Once I fix my spelling mistakes I now get to return to the previous state.
++SHOW: fix the sellcheck mistake++



### Storage
Did you notice how we didn’t explicitly save the views to a location on disk. Vim does that behind the scenes! 
But if you __have to__ know — they are saved into the `&viewdir` directory. 
++SHOW: echo &viewdir++
++SHOW: Open `&viewdir`/view1.vim and show the spell settings ++

Let’s go there. Here’s the spellcheck view we created earlier and these are are the spellcheck options we set.

Since files here aren’t automatically deleted; it is a good idea to occassionaly prune this directory, just to keep it clean.
++SHOW: remove that spellcheck view++
++SHOW: helpgrep location for this line++



### Options
Vim let’s you sorta decide what get’s put into views with `viewoption`. By default it is set to save
- __cursor__ - which saves the cursor’s position within the file and window, 
- __folds__ - which saves manually created folds, whether they have been opened or closed, and more fold related settings
- and __options__ - which unsurprisingly saves a ton of local options.

The other two - __slash__ and __unix__ aid with Windows compatibliity for view files. ++SHOW: :help viewoptions++
 Consult `:help :mkview` for a more descriptive list of what gets saved.
++SHOW: :help :mkview+




### Naming
If you want to save views to a specific location, like the current directory for example - pass a path as an argument to `:mkview`. Let’s create that spellcheck focussed view again with a better name this time - `:mkview spellview.vim`. You will have to use _source_ instead of loadview.
++SHOW: Source a named view file++

Calling the view `spellview` makes it easier to remember because a name is more indicative of the view’s purpose than a number.

++SHOW: Save the view as spell.vim++
++SHOW: Different states like folds.vim, spell.vim etc.++
++SHOW: :mkview existingviewfile++

When you source a view file with an empty buffer on screen, Vim just loads the file the view was initally created in. Like so.
++SHOW: Sourcing a viewfile assoc with unopened file++


## Conclusion
View’s are pretty neat, but they have a few rough edges - as listed in the docs here - so bear that in mind. 
++SHOW: helpgrep to the rough edges++ 

Here’s some reading material for the weekend. _Section 21.5_ provides a comprehensive explanation on Views. Check it out.
++SHOW: helpgrep section 21.5++

Well, thanks for watching. See you around!
