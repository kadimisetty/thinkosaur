# Sessions
## Introduction
Sessions are the real deal.



## Sessions
### Definition
They can not only remember the layout of the entire workspace but can also save a variety of global settings.

Before we begin, there are two points I’d like to clarify -
1. Saving a session _does not_ save changes to disk. That still remains our responsibility. The `:wall` command, which writes all changed buffers to the disk, should help with that.
2. Sessions are not saved automatically.



### Create and Load Sessions
Sessions are created with the `mksession` command that captures the current session into the file `Session.vim` within the current directory.  Sessions have to be saved to a file; so theres always gonna be a Session file to keep track of.
++ls to show Session.vim++
Since a session file is merely a bunch of Vim commands, it can be invoked simply by _sourcing_ it.
++:source Session.vim ++ 
The file’s name can be customised by passing a pathname as an argument to `:mksession`. 
++:mksession BeanstalkSession.vim++ 
++source BeanstalkSession.vim++


### Usage 
Let’s create a session for this website.
- This project contains three web pages and associated stylesheets. 
	++ Open a small project ++
	++ Project contains index/about/404.html, stylesheet.css ++
- `:mksession` saves this session to this directory.
	++ Create a session ++
	++ Show the Session.vim file ++
- Let’s see if sourcing it brings back the layout we had earlier -
	++ Close everything ++
	++ Start Vim ++
	++ :source Session.vim ++
	++ Reopen the session ++
	Yup, exactly where we left it.

It’s quite common to want to invoke the session right as you as begin working. Keeping that in mind, Vim provides a handy command line argument “Capital S” that preloads Vim with that session.
++ CLOSE project again ++
++ Reopen this time from the command line ++
++ $ vim -S Session.vim ++

Being able to persist the workspace layout not only saves me the time it takes to recreate it; but also affords me the opportunity to fine-tune the layout.
++ Open a new tab and setup for styles ++ 
So, I just discovered that I like having stylesheets in a separate tabpage. Let’s update the layout and save this Session over the previous one.
++ :mksession! ++


#### Clutter
Sessions are saved into files that are technically irrelevant to the context of the project. So it is justified in considering them - clutter. My recommended solution to this problem is to have source control ignore the session file. In git, this can be accomplished by listing it in `.gitignore`. It’s usually located in the repo’s root directory.
	++ vim .gitignore ++
	++ Add Session.vim ++
	++ Add `*Session.vim` to ignore files ending with Session.vim++

But, if you’re the only person working on the project, feel free to include the Session file in the repository. This lets you “sync” your workspace on multiple machines. 

Another common usage pattern is to save all Session files into a single directory situated separately from all projects. A directory like say - `.vim/sessions` for example; from where you can pick and load sessions on demand. They can be differentiated by naming them after the projects that they’ve been created from. I recommend using the `ProjectnameSession.vim` format. This is the usage pattern that I personally use.


#### Switching Projects
With Sessions, Vim allows you to work on projects located in different locations without moving away from the current directory. This is possible, because all session files have a `cd` command that switches Vim into the relevant project’s directory. 
++Open a Session file and show the `cd`++

Let’s check if this works.
++`cd` to a blank directory. `pwd & ls`++
++Open WebpageSession.vim `pwd & ls`++
++Open BeanstalkSession.vim `pwd & ls`++
++Close to current dir. `pwd & ls`++
Once you exit Vim, you’re back where you started.


#### Complexity
If you just want to stick with the basics - all you have to remember is that `:mksession` creates a session and `vim -S` loads that session.
++ :mksession ++
++ :q++
++ vim -S++

This tip takes advantage of the fact that the default name `Session.vim` used by `:mksession` also happens to be the same name Vim looks for, by default, when using the command line argument `-S`.



## Conclusion
Section 21.4 is a great place to read up on Sessions. Check it out sometime. 


Alright then, till next time.
