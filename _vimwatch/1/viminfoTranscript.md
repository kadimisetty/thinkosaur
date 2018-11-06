# Episode 4 - Viminfo
In earlier episodes, we saw how Sessions can restore complete workspace environments; truth is they still miss some crucial information - registers, marks, command line histories etc.  But viminfo’s got your back. 

Everytime Vim exits, it saves that information and more into a simple text file named viminfo. Once you re-open Vim, it reads that file again and thus is able to restore that information.


## Contents
So what does it save exactly? Let’s see:
1. History (Command line, Search, Input line history)
	- All commands entered into the command line are stored into a history table which is then written to viminfo on exit. The command line history, accessed by typing `q:` in normal mode, should be identical to what’s written in `viminfo`. ++DO: Compare viminfo to q:++  
	- Let’s run a quick echo command. See how it appears in the command line history but not in viminfo. That’s because viminfo is not updated on very change. By default, it’s only written to on exit. ++DO: Run ehco hello and compare viminfo. Then reopen vim and show that echo command in viminfo++ 
	- Vim does the same with search history as well. Let’s pull it up with the normal mode command`q/`. ++ Compare the history to viminfo++  viminfo is capable of holding not just the command line history but this search hostory as well. In fact it can also help restore your input line histiory as well.  ++CHECK++. ++SHOW: help history and list the 3 kinds++ 
	- Saving the search history can be really helpful as it’s quite common to repeat a search for the same pattern and it’s nice to be able to “arrow up” and reuse an already typed pattern.
	-  `n` in normal mode finds matches for the most recently used search pattern and the `&` command repeats the most recent substitution. Preserving the search history makes commands like these work even though you just started a fresh instance of Vim. Pretty neat huh?
2. Registers 
	- For those unfamiliar with registers, they are like multiple clipboards. You can save text in upto 26 register’s named a through z. Their contents can then be accessed from anywhere within the current Vim instance. Once you exit, any information you have in these registers gets written into vimifo. Let’s store some data into a register. `y"a` should yank this selection into register-a. ++DO: Put word in “a.++ The `registers` command  show us contents of all registers. Theres register a.. ++DO: registers and show that word++ Viminfo saves all of this. ++DO: Open viminfo and locate the text++ 
3. Marks
	-  Quick refresher on what Marks do - they let you save locations within a file using __m__ in normal mode. For example, `ma` saves this line’s location into mark-a. Let’s scroll down. Now __backtick-a__ should transport us to mark-a’s location. ++DO `a++ 
	- Similar to registers you got 26 slots, a-z, to store your marks in. The command `:marks` shows us if there’s any set yet. Here’s that location we put in _a_. ++DO: quit vim and run marks again++. mark-a. ++DO: Find that mark in viminfo file++ 
	- Just a note  - These marks are only avaiable when the file it was created in is in the active window. ++DO: Open two window. Create mark a. Show in marks++ Lets open a new window. Mark-a should not  beavailable here. ++DO: Open new window and :marks to show that mark’s not there++ If you wanna use marks to jump to a spot in a file that isn’t open, you can use file marks. These can be stored in uppercse “A-Z”. They not only hold the location but also the filenames. Both these marks are stored into viminfo.
		
	- Wait there’s more - viminfo makes a special set of marks possible. These are referred to by numbers _0 through 9_ and every time Vim exits, it stores the cursor’s position and filename from that moment into mark-0. Marks 0 to 9 kinda act like a queue. So, the previous contents of 0 get pushed down to 1, 1’s to 2, so on and so forth. This feature lets you pick up working from the exact spot you quit earlier! ++DO Pick a spot. close Vim and show you’re back at that spot++
	- Here’s great tip from the docs. Vim’s command line argument _c_ runs a Vim command after startup. So placing the normal mode command _jump to mark-0_ `"normal \`0"` in `vim -c normal \`0”` should open Vim from it’s previous location. Aliasing that to say `lvim` should resume your work even quicker -  `alias lvim='vim -c "normal '\''0"'`   ++DO `vim -c normal \`0”` ++ ++DO: alias lvim='vim -c "normal '\''0" ++
4. Buffer list
	- The `:buffers` command lists all open buffers. You can setup viminfo to save this list so they can be reloaded upon reopen.
	- ++REDO++ If you think about it, it kinda seems icky that there’s all these buffers from your previous work session - which is great when that’s what you want but if you intended to start working on something new, it could be a hassle - but worry not; they are only loaded when Vim was opened without being supplied  a file or  session name. ++CHECK++ This features is not even enabled by default anyway, so don’t worry about it.
5. Global Variable
	- Global variables can be persisted with viminfo - atleast as long as they are of a certain type and format.

## Read & Write
You don’t have to necessarily quit and open Vim in order to save and load from viminfo. The `:wviminfo` command or `:wv` writes to viminfo and `:rv` or `:rviminfo` reads from viminfo. Supplying a filename to these commands lets you use them with a different filename
++DOL Write and read from a different filename++

If you modified a register and attempt to read from _viminfo_, you will lose that data; so Vim, exercising it’s highly cautious nature, insists that you append the read command with a _bang_ to force writing over the current register. A bang at the end usually indicates  that your action could be destructive. ++DO save wviminfo, write to register and read from viminfo, show error, read from viminfo again++

Let’s put all that knowledge to use with another tip from the docs - To transfer the contents of registers between two different instances of Vim - we can write a viminfo from one and read it from the other. 
