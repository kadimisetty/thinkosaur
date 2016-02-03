# Episode 1 - Job control
Lets learn how to multitask inside a shell using Job Control techniques.

## Job Control
## Introduction
Whats a job? Another name for it is __process-group__ — which is just a collection of related processes, which is anything from a simple program like the date command or one as complicated as a command line text editor.
''date
''vim

++PAUSE++++

### Foreground jobs
We’ll employ the  _sleep_ program as an example of a job that can run for a specified duration. `sleep 5` makes the shell “sleep” for 5 seconds.
'' sleep 5

Did you notice how the prompt, where we type in commands, was not displayed until sleep finished. That’s normal! Once sleep begins execution it can now be referred to as the __foreground job__. After it finishes, the shell goes back to being the foreground job and is able to display the prompt again.

++PAUSE++++


### Suspended Jobs
Let me show you how to freeze or _suspend_ a job. We’ll use that 5 second sleep job again. You can suspend it using the special terminal character  _Ctrl Z_. 
'' sleep 5
'' Ctrl-Z while executing

`jobs` should list all current jobs, including suspended ones.
''jobs
There! Let’s suspend another sleep job; this time we’l set it up for 10 seconds.
''sleep 10
''Ctrl Z
''jobs
There’s those two.  Using the foreground command  __fg__ you can _resume_ a job’s execution by bringing it back into the foreground. Let’s revive that 10 second sleep job.
''fg
See! 

You can think of suspending a job as __pausing__ it so you can go do something else for a little bit. Once you’re ready to return, you can resume that earlier job bringing it back to the foreground.


### Background Jobs
Were you ever in the middle of a task that you had no idea would take that long, like an installation procedure that just seems to keep on going. That is annoying because you can’t work on anything else untill it finishes and who knows when that’s gonna happen. 
Yes, you could suspend it, do your thing and get back but – let me tell you about the __background__ command which can do one better. Unlike fg, the background command can run that long task asynchronously in the background, thereby allowing us to do other things on the prompt again. 

To see how it works, let’s use a long sleep job as an example,  then  `bg`  to have it run in the background.
'' sleep 10

Gotta suspend it first with Ctrl-Z
'' ctrl-z
'' jobs
There it is suspended. 
Now instead of resuming it with fg we do 
'' bg
See how we still have the prompt. Don;t let that decieve you 
'' jobs
Because sleep is running in the background right now as we speak.

#### The `&` command
The shortcut __&__ can save us a step here. Place it at the end of a command to run it in the background from the get go, without having to suspend it first and send it to the background later.
'' sleep 20 &
'' jobs

##### Notify
Let me run another job in the background. I wanna show you something.
'' sleep 3 &
'' show notification
When a job is finishes execution in the background, the shell is kind enough to let us know with a notification. Although if you’d rather not see these notifications they can be turned ‘em off with.
'' unsetopt notify
'' sleep 3 &
'' see no notification
Now these notifications are withheld untill a new prompt line is displayed.

##### tostop
Watch out for this too - When a background job writes to the terminal screen, 
'' (sleep 2; echo DONE-SLEEPING) &
it can get pretty confusing as it’ll look like text appeared out of thin air. Good thing you can set the `tostop` terminal setting variable to avoid this. The shell already suspends any jobs that try to read from the screen but with `tostop` it will also suspend those that try to write to screen.
''stty tostop
'' (sleep 2; echo DONE SLEEPING) &
See! 


### Job Number
All jobs have an associated index number, which can come handy when picking between multiple jobs. Lets suspend two sleep jobs so I can show you where to find those numbers.

'' sleep 5  CtrlZ
''	sleep 10 CtrlZ
''	jobs #Show the index number
It is possible to refer to a particular job when using a job control command like fg or bg, by following that command with a job ID number.

''Show the plus sign
IF the number is omitted, the recently suspended one is used; the one with a plus sign.


### Kill Jobs
To stop or kill a job; whether it’s suspended or running in the background we use the kill command.  Kill does require an index number to identify the job. 
'' kill 1

When we do a job listing this time, that job should not be here.
''jobs
It’s been killed.


### Shortcuts with %
You’re gonna end up using fg _a lot_; Save a keystroke by using the `%` command instead. Lemme suspend a few jobs first and see what it does.

'' Send vim, sleep 5 to background
'' jobs
That should do it. Now without consulting the job listing, it is going to be hard to remember what ID to use in the fg command. 

`%` is perfect for this. Just spell out the name of the job, like so.
''% vi
and it will be brought to the foreground. Isn’t that easier? 

By the way, `%` can also be used in other job control commands by placing it to the end. 
'' vim Ctrl-Z
Ctrl-Z
'' jobs
Jobs
'' kill %vim
'' jobs
See no vim!. 


There are a lot of shortcuts based on `%`. Consult your shell’s documentation for more.

++[Show in slide]
++1. `%number` The job with the given number.
++2. `%string` Any job whose command line begins with string.
++3. `%?string` Any job whose command line contains string.
++4. `%%` Current job.
++5. `%+` Equivalent to ‘%%’.
++6. `%` Previous job.



### Resources and quitting shell
Couple of points —
1. A background job still consumes pretty much the same amount of resources it would have if it were a foreground job. 
2. Also, if you have any jobs running when you quit your shell, they’l get killed along with it. 
	'' sleep 5 &
	'' exit
	So heed this warning!
	'' SHOW the warning


## Conclusion
Here's a summary about Job Control today-
1. A `job` is just a running program
2. `Ctrl-Z` suspends a running program
3. `fg` brings it to the foreground
4. `bg` runs it in the background
5. `jobs` shows a listing of all jobs
6. `kill` stops or kills a job.
The terminology and some variable names are specific to the shell I use _zsh_ and might vary depending on what shell you have; but the concepts still remain the same.

### Use cases
Let me leave  you with two specific examples of how I use job control techniques in my workflow -
1. I use a command line text editor and often need to jump back to the shell for a quick task or two. It’s inconvenient to me to have to open a new terminal window so I use `Ctrl-Z` to suspend my editor, finish my tasks, and then restore my editing session with the foreground command.
2. Once in a while you know how we have to open GUI programs from the command line. Like say I’m opening Google Chrome with some flags set. It’s silly to have the entire terminal locked up while that program is running. You can run it by sending it to the background right away though - `chrome &` see not an issue at all this time.

These techniques will make a fine addition to your command line kung fu and I can’t wait to see how you use them to improve your workflow. Ok then! See you next time!
