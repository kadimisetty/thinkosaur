# Episode 3 - Session Options
## Introduction
You can customize _what to save_ in a session. 

You’d do this by configuring the `sessionoptions` option. Options are like regular variables, the only difference being they hold Vim’s settings. 


### Options
So, heres how this works - you pick what you want to save and add up all the assoicated keywords separated with commas and put em into `sessionoptions`.



Here’s the first bunch of available choices:
-  Including the keyword `globals` saves all global variables. But they gotta be strings or numbers and have to be capitalized and followed by at least one lowercase letter.
- _localoptions_ saves options that are scoped locally to the  window or buffer.
-  This just saves __all__ options and mappings - global or otherwise.



- _tabpages_ stores all tabpages in the session, as opposed to just the current tabpage. Saving just the current tabpage  might actually be something you want, coz then you can load different sessions in each tabpage.
-  This saves all buffers, even the hidden ones.
- _folds_ saves all 
	- manually created folds; 
	- whether they’ve been opened/closed
	- and any local fold options as well.


-  This saves open help pages and
- _blank_ saves empty windows that don’t have an associated buffer yet. 



If you use Vim as a GUI, these help preserve position related information - 


_slash_ and _unix_, just like they do in `viewoptions`, keep your session files compatible with Windows.


- _curdir_ short for current directory saves the current directory as the working directory and
- _sesdir_ short for session directory, switches the working directory to where the session files are located. 

It does conflict to have both sesdir and curdir  enabled, so you might wanna pick just one. 

I don’t use sesdir, because I usually save my sessions files outside the project directory and it would be undesirable to have the working directory be somewhere else.



### Usage 
By default, `sessionoptions` holds this list-
> blank, buffers, curdir, help, tabpages, winsize, folds

 

I tend to agree with the defaults except I leave out `help`, because I personally don’t see much value in restoring help pages ,and I don’t use `winsize` either as pretty much all my Vim usage is within the console anyway.



## Conclusion

You can find more information in the docs using the help command.
 

Alright then, See you in the next episode!

## Errata
Blank windows are just empty windows. They do have an empty buffer associated with them.
