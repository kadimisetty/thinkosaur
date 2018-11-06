# Globbing Basics

## Introduction
Globbing is an _incredibly_ useful shell feature that lets you filter filenames with simple __text patterns__. It comes from the phrase “Global Substitution” and ++Chuckle++ yes it is quite a funny sounding word.


## Make Case 
How exactly is it useful? 
Say you need to delete a few files in a directory that has many kinds of files, with various names and extensions. 
'' ls
You can do it the dumb way — using the delete command one at a time on every single filename; or the smart way — by handing the delete command a list of filenames that’ve been automatically generated from a pattern.  It's smart because it not only makes the task easier but also removes the quotient of human error. 

Since globbing is a shell feature it can be used with not just delete, but pretty much any command within the shell.


## Operators
In order to glob effectively, we’ll have to master glob operators — the individual symbols used to form glob patterns.

We’l start out by learning how to pick files of a certain “format” - say only python files. We look for commonalities in the desired filenames. All python files start with a string and end with `.py`. That can be written down with the glob pattern `*.py` I am gonna use `echo ` on this pattern to print out all matching filenames. 

echo is real simple – All it does is print it’s _arguments_ onto the screen; so its a great way to see the results of expanding a glob pattern.

''echo	*.py
The star operator here matches all filenames that begin with a string of any length and the `.py` part limits the list to only those that end with `.py` 
''SHOW RESULT OF echo *.py

Unlike star, the dot character is not a part of the glob language. In fact it cannot be generated from any glob operator. If you’re looking for a dot in a filename, you’re gonna __have to write it down__! Same with `/` as well. 

All this leads us to the following scenario — When attempting to match _all_ filenames within a directory using `*`
''echo *
you’ll miss any “hidden” files because atleast in the UNIX environment, hidden file’s names begin with `.`. To match them  we’ll have to write the dot explicitly `.*`.
'' echo .*

The `?` operator is another nice addition to your pattern vocabulary. It matches exactly _one single_ character.  So if you’re trying to get these to show up
'' a.py b.py c.py
for example, `?`s your guy!
'' ?.py
This would not match something like `example.py` because `example` has more way more than one character.


## Note
A few important points:
- Before using globbing in a destructive command like delete or move, I’d recommend trying that pattern with echo first, just to check for errors.
- Feel free to mix and match operators when constructing patterns.
	'' project-a.py project-b.py project-a.txt project-b.txt
	Let’s write a pattern that matches these files
	'' echo project-?.*
	The `?` operator matches a single character; get that outa the way, you’d be able to catch `a` here and `b` here. And the `*` after dot should match both the `py` and `txt` strings. Together this pattern is specific enough to give us what we want in this scenario. 
- Chances are most glob patterns you write are going to be for one-off tasks. If you're confident that the pattern's specificity is good enough, I'd recommend doing a quick check with echo and moving on. It's quite easy to get lost creating unnecessarily specific patterns that don’t really get used again.
- For those familiar with regular expressions, it might feel like glob patterns are pretty much the same syntax. But they differ in a lotsof ways, like with the dot character. Globbing was not extracted from regular expressions. In fact it predates em by a few good years. So be wary.
- Globbing is a shell feature and can only be used within a shell. What does that mean? - A program does not receive the globbing pattern itself. The shell does all the work and hands the program a generated list of matching filenames as _arguments_.

## Conclusion
That about covers the essentials. To sum it all up —
1. Globbing is a shell feature.
2. Glob patterns generate a list of matching filenames
3. The `*` operator matches strings of any length, inclduing 0.
4. and the `?` operator matches exactly _one_ character.


I hope you introduce what we learned into you workflow; I’m confident it will make your time working in the shell more pleasant and efficient.

Ok then, See ya later!
