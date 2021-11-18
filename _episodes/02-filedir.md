---
title: "Navigating Files and Directories"
teaching: 30
exercises: 10
questions:
- "How can I move around the cluster filesystem"
- "How can I see what files and directories I have?"
- "How can I make new files and directories."
objectives:
- "Create, edit, manipulate and remove files from command line"
- "Translate an absolute path into a relative path and vice versa."
- "Use options and arguments to change the behaviour of a shell command."
- "Demonstrate the use of tab completion and explain its advantages."
keypoints:
- "The file system is responsible for managing information on the disk."
- "Information is stored in files, which are stored in directories (folders)."
- "Directories can also store other directories, which then form a directory tree."
- "`cd [path]` changes the current working directory."
- "`ls [path]` prints a listing of a specific file or directory; `ls` on its own lists the current working directory."
- "`pwd` prints the user's current working directory."
- "`/` on its own is the root directory of the whole file system."
- "Most commands take options (flags) that begin with a `-`."
- "A relative path specifies a location starting from the current location."
- "An absolute path specifies a location from the root of the file system."
- "Directory names in a path are separated with `/` on Unix, but `\\` on Windows."
- "`..` means 'the directory above the current one'; `.` on its own means 'the current directory'."
---

The part of the operating system responsible for managing files and directories
is called the **file system**.
It organizes our data into files,
which hold information,
and directories (also called 'folders'),
which hold files or other directories.

The NeSI filesystem looks something like this:

![The file system is made up of a root directory that contains sub-directories
titled home, nesi, and system files](../fig/NesiFiletree.svg)

Several commands are frequently used to create, inspect, rename, and delete files and directories.
To start exploring them, we'll go to our open shell window.

First, let's find out where we are by running a command called `pwd`
(which stands for 'print working directory'). Directories are like *places* — at any time
while we are using the shell, we are in exactly one place called
our **current working directory**. 
Commands mostly read and write files in the
current working directory, i.e. 'here', so knowing where you are before running
a command is important.

```
{{ site.remote.prompt }} pwd
```
{: .language-bash}

```
/home/<username>
```
Those using Jupyter will see something like this instead:
```
/home/<username>/.jupyter/jobs/<jupyter-jobid>
```
{: .output}

> ## Default directory
>
> We will assume that your `pwd` command returns your user's home directory (`/home/` followed by your username).
> If `pwd` returns something different, you will need to navigate to your home directory by running the command `cd`
{: .callout}

To understand what a 'home directory' is,
let's have a look at how the file system as a whole is organized.

At the top is the **root directory**
that holds all the files in a filesystem.
We refer to it using a slash character, `/`, on its own;
this character is the leading slash in `/home/<username>`.

We know that our current working directory `/home/<username>` is stored inside `/home`
because `/home` is the first part of its name.
Similarly,
we know that `/home` is stored inside the root directory `/`
because its name begins with `/`.

In the NeSI file system you will have access to several different locations.
#### Home Directory

User-specific files such as configuration files, environment setup, source code, etc.

<table style="width: 966px; height: 90px;">
<tbody>
<tr>
<td style="width: 109.453px;"></td>
<td style="width: 300.703px;">Location</td>
<td style="width: 167.562px;">Default Storage</td>
<td style="width: 142.734px;">Default Files</td>
<td style="width: 89.3594px;">Backup</td>
<td style="width: 155.188px;">Access Speed</td>
</tr>
<tr>
<td style="width: 109.453px;">Home</td>
<td style="width: 300.703px;"><code>/home/&lt;username&gt;</code></td>
<td style="width: 167.562px;">20GB</td>
<td style="width: 142.734px;">1,000,000</td>
<td style="width: 89.3594px;">Daily</td>
<td style="width: 155.188px;">Normal</td>
</tr>
</tbody>
</table>

#### Project Directory

Persistent project-related data, project-related software, etc.

<table style="width: 966px; height: 90px;">
<tbody>
<tr>
<td style="width: 109.453px;"></td>
<td style="width: 300.703px;">Location</td>
<td style="width: 167.562px;">Default Storage</td>
<td style="width: 142.734px;">Default Files</td>
<td style="width: 89.3594px;">Backup</td>
<td style="width: 155.188px;">Access Speed</td>
</tr>
<tr>
<td style="width: 109.453px;">Project</td>
<td style="width: 300.703px;"><code>/nesi/project/&lt;projectcode&gt;</code></td>
<td style="width: 167.562px;">100GB</td>
<td style="width: 142.734px;">100,000</td>
<td style="width: 89.3594px;">Daily</td>
<td style="width: 155.188px;">Normal</td>
</tr>
</tbody>
</table>

#### Nobackup Directory

Persistent project-related data, project-related software, etc.
<table style="width: 966px; height: 90px;">
<tbody>
<tr>
<td style="width: 109.453px;"></td>
<td style="width: 300.703px;">Location</td>
<td style="width: 167.562px;">Default Storage</td>
<td style="width: 142.734px;">Default Files</td>
<td style="width: 89.3594px;">Backup</td>
<td style="width: 155.188px;">Access Speed</td>
</tr>
<tr>
<td style="width: 109.453px;">Nobackup</td>
<td style="width: 300.703px;"><code>/nesi/nobackup/&lt;projectcode&gt;</code></td>
<td style="width: 167.562px;">10TB</td>
<td style="width: 142.734px;">1,000,000</td>
<td style="width: 89.3594px;">None</td>
<td style="width: 155.188px;"></td>
</tr>
</tbody>
</table>

> ## Slashes
>
> Notice that there are two meanings for the `/` character.
> When it appears at the front of a file or directory name,
> it refers to the root directory. When it appears *inside* a path,
> it's just a separator.
{: .callout}

## Listing the contents of directories

As you may now see, using a bash shell is strongly dependent on the idea that
your files are organized in a hierarchical file system.
Organizing things hierarchically in this way helps us keep track of our work:
it's possible to put hundreds of files in our home directory,
just as it's possible to pile hundreds of printed papers on our desk,
but it's a self-defeating strategy.

The command to prints the names of the files and directories is `ls` followed by the desired directories path. 
However, `ls` can be run without an argument, in which case it will list the contents of the directory you 
are currently located in.

We will now list the contents of the `project` directory we we will be working from. We can
use the following command to do this:

```
{{ site.remote.prompt }} ls /nesi/project/nesi99991
```
{: .language-bash}

```
 resbaz2021
```

You should see a directory called `resbaz2021`, and possibly several other directories. For the purposes of this workshop you will be working within `/nesi/project/nesi99991/resbaz2021`

## Moving about

The command to change locations is `cd` followed by a
directory name to change our working directory.
`cd` stands for 'change directory',
which is a bit misleading:
the command doesn't change the directory;
it changes the shell's idea of what directory we are in.
The `cd` command is akin to double clicking a folder in a graphical interface to get into a folder.

We will now move into the`project` directory we saw above. We can
use the following of commands to get there:

```
{{ site.remote.prompt }} cd /nesi/project/nesi99991/resbaz2021
```
{: .language-bash}

You will notice that `cd` doesn't print anything. This is normal. Many shell commands will not output anything to the screen when successfully executed.
But if we run `pwd` after it, we can see that we are now
in `/nesi/project/nesi99991/resbaz2021`.

```
{{ site.remote.prompt }} pwd
```
{: .language-bash}

```
/nesi/project/nesi99991/resbaz2021
```
{: .output}

## Creating files

As previously mentioned, it is general useful to organise your work in a hierarchical file structure to make managing and finding files easier. It is also is especially important when working within a shared directory with colleagues, such as a project, to minimise the chance of accidentally effecting your colleagues work. So for this workshop you will each make a directory using the `mkdir` command within the workshops directory for you to personally work from.

```
{{ site.remote.prompt }} mkdir <username>
```
{: .language-bash}

## General Syntax of a Shell Command

Now that you have all created your own directory to work from lets use the `ls` command to list contents of `/nesi/project/nesi99991/resbaz2021` again.

We have previously used Shell commands with arguments, however, this time we will also use what are known as **options**, **flags** or **switches**, which is part of the general syntax of not just `ls` but all Shell commands

Consider the command below as a general example of a command,
which we will dissect into its component parts:

```
$ ls -l /nesi/project/nesi99991/resbaz2021
```
{: .language-bash}

![General syntax of a shell command](../fig/shell_command_syntax.svg)

`ls` is the **command**, with an **option** `-l` and an
**argument** `/nesi/project/nesi99991/resbaz2021`.
Options, either start with a single dash (`-`) or two dashes (`--`), and they change the behavior of a command.
Often options will have a short and long format e.g. `-a` and `--all`
[Arguments] tell the command what to operate on (e.g. files and directories).

When you run this command you should see something like this output (though likely with more lines):

```
total 1
drwxrws---+ 2 usr123 nesi99991 4096 Nov 15 09:01 usr123
drwxrws---+ 2 usr345 nesi99991 4096 Nov 15 09:01 usr345
```
{: .output}

Here we can see that the `-l` option has used what is know as the "long listing format",
and has listed all the files in alphanumeric order, which can make finding a specific file easier.
It also includes information about thefile size, time of its last modification, and permission and ownership information.

Sometimes options and arguments are referred to as **parameters**.
A command can be called with more than one option and more than one argument, but a
command doesn't always require an argument or an option.

Each part is separated by spaces: if you omit the space
between `ls` and `-l` the shell will look for a command called `ls-l`, which
doesn't exist. Also, capitalization can be important.
For example, `ls -s` will display the size of files and directories alongside the names,
while `ls -S` will sort the files and directories by size.

Another userful option for `ls` is the `-a` option, lets try using this option together with the -l option:

```
$ ls -la
```
{: .language-bash}

```
total 1
drwxrws---+  4 usr001 nesi99991   4096 Nov 15 09:00 .
drwxrws---+ 12 root   nesi99991 262144 Nov 15 09:23 ..
drwxrws---+  2 usr123 nesi99991   4096 Nov 15 09:01 usr123
drwxrws---+  2 usr345 nesi99991   4096 Nov 15 09:01 usr345
```
{: .output}

You might notice that we now have two extra lines for directories `.` and `..`. These are hidden directories which the `-a` option has been used to reveal, you can create a hidden directories or files by beggining their filenames with a `.`.

These two specific hiddent directories are special as they will exist hidden inside every directory, with the `.` hidden directory reprenting your current directory and the `..` hidden directory reprenting the **parent** directory above your current directory.

## Relative paths

You may have noticed in the last command we did not specify an argument for the directory path.
Until now, when specifying directory names, or even a directory path (as above),
we have been using what are know **absolute paths**, which work no matter where you are currently located on the machine
since it specifies the full path from the top level root directory.
When you use a relative path with a command
like `ls` or `cd`, it tries to find that location from where we are,
rather than from the root of the file system.
In the previous command, since we did not specify an **absolute path** it ran the command on the relative path from our current directory 
(implcitly using the `.` hidden directory), and so listed the contents of our current directory.

To specify the **absolute path** to a directory it is necessary to 
use its entire path from the root directory, which is indicated by a
leading slash. The leading `/` tells the computer to follow the path from
the root of the file system, so it always refers to exactly one directory,
no matter where we are when we run the command.

The simplest method of moving up to the parent directory is to use the previously mentioned hidden `..` directory:

```
$ cd ..
```
{: .language-bash}

`..` is a special directory name meaning
"the directory containing this one",
or more succinctly,
the **parent** of the current directory.
Sure enough,
if we run `pwd` after running `cd ..`, we're back in `/nesi/project/nesi99991`:

```
$ pwd
```
{: .language-bash}

```
/nesi/project/nesi99991
```
{: .output}

Depending on your default options,
the shell might also use colors to indicate whether each entry is a file or
directory.

> ## Clearing your terminal
>
> If your screen gets too cluttered, you can clear your terminal using the
> `clear` command. You can still access previous commands using <kbd>↑</kbd>
> and <kbd>↓</kbd> to move line-by-line, or by scrolling in your terminal.
{: .callout}

> ## Listing in Reverse Chronological Order
>
> By default, `ls` lists the contents of a directory in alphabetical
> order by name. The command `ls -t` lists items by time of last
> change instead of alphabetically. The command `ls -r` lists the
> contents of a directory in reverse order.
> Which file is displayed last when you combine the `-t` and `-r` flags?
> Hint: You may need to use the `-l` flag to see the
> last changed dates.
>
> > ## Solution
> > The most recently changed file is listed last when using `-rt`. This
> > can be very useful for finding your most recent edits or checking to
> > see if a new output file was written.
> {: .solution}
{: .challenge}


Let's try moving to your personal directory from before. Last time we used `cd`, we used
the absolute path from the root, but it is often easier to use the relative path.

## Tab completion

Sometime file paths can also be very long, making typing out the path tedious.
One neat trick you can use to save yourself time is to use something called **tab completion**.
If you start typing the path in a command and their is only one possible match,
if you hit tab the path will autocomplete (until there are more than one possible matches).

For example, if you type:

```
$ cd res
```
{: .language-bash}

and then press <kbd>Tab</kbd> (the tab key on your keyboard),
the shell automatically completes the directory name for you (since there is only one possible match):

```
$ cd resbaz2021/
```
{: .language-bash}

However, that command would only take you `/nesi/project/nesi99991/resbaz2021`.
You want to move to your personal working diretory. If you hit <kbd>Tab</kbd> once you will
likely see nothing change, as there are more than one possible options. Hitting <kbd>Tab</kbd>
a second time will print all possible autocomplete options.

So now let complete te relative path to your personal directory in this `cd` command:

```
$ cd resbaz2021/<username>
```
{: .language-bash}

Check that we've moved to the right place by running `pwd`.

> ## Absolute vs Relative Paths
>
> Starting from `/Users/amanda/data`,
> which of the following commands could Amanda use to navigate to her home directory,
> which is `/Users/amanda`?
>
> 1. `cd .`
> 2. `cd /`
> 3. `cd /home/amanda`
> 4. `cd ../..`
> 5. `cd ~`
> 6. `cd home`
> 7. `cd ~/data/..`
> 8. `cd`
> 9. `cd ..`
>
> > ## Solution
> > 1. No: `.` stands for the current directory.
> > 2. No: `/` stands for the root directory.
> > 3. No: Amanda's home directory is `/Users/amanda`.
> > 4. No: this command goes up two levels, i.e. ends in `/Users`.
> > 5. Yes: `~` stands for the user's home directory, in this case `/Users/amanda`.
> > 6. No: this command would navigate into a directory `home` in the current directory if it exists.
> > 7. Yes: unnecessarily complicated, but correct.
> > 8. Yes: shortcut to go back to the user's home directory.
> > 9. Yes: goes up one level.
> {: .solution}
{: .challenge}

> ## Relative Path Resolution
>
> Using the filesystem diagram below, if `pwd` displays `/Users/thing`,
> what will `ls -F ../backup` display?
>
> 1.  `../backup: No such file or directory`
> 2.  `2012-12-01 2013-01-08 2013-01-27`
> 3.  `2012-12-01/ 2013-01-08/ 2013-01-27/`
> 4.  `original/ pnas_final/ pnas_sub/`
>
> ![A directory tree below the Users directory where "/Users" contains the
directories "backup" and "thing"; "/Users/backup" contains "original",
"pnas_final" and "pnas_sub"; "/Users/thing" contains "backup"; and
"/Users/thing/backup" contains "2012-12-01", "2013-01-08" and
"2013-01-27"](../fig/filesystem-challenge.svg)
>
> > ## Solution
> > 1. No: there *is* a directory `backup` in `/Users`.
> > 2. No: this is the content of `Users/thing/backup`,
> >    but with `..`, we asked for one level further up.
> > 3. No: see previous explanation.
> > 4. Yes: `../backup/` refers to `/Users/backup/`.
> {: .solution}
{: .challenge}

> ## `ls` Reading Comprehension
>
> Using the filesystem diagram below,
> if `pwd` displays `/Users/backup`,
> and `-r` tells `ls` to display things in reverse order,
> what command(s) will result in the following output:
>
> ```
> pnas_sub/ pnas_final/ original/
> ```
> {: .output}
>
> ![A directory tree below the Users directory where "/Users" contains the
directories "backup" and "thing"; "/Users/backup" contains "original",
"pnas_final" and "pnas_sub"; "/Users/thing" contains "backup"; and
"/Users/thing/backup" contains "2012-12-01", "2013-01-08" and
"2013-01-27"](../fig/filesystem-challenge.svg)
>
> 1.  `ls pwd`
> 2.  `ls -r -F`
> 3.  `ls -r -F /Users/backup` -->
>
> > ## Solution
> >  1. No: `pwd` is not the name of a directory.
> >  2. Yes: `ls` without directory argument lists files and directories
> >     in the current directory.
> >  3. Yes: uses the absolute path explicitly.
> {: .solution}
{: .challenge}

> ## Unsupported command-line options
> If you try to use an option (flag) that is not supported, `ls` and other commands
> will usually print an error message similar to:
>
> ```
> $ ls -j
> ```
> {: .language-bash}
>
> ```
> ls: invalid option -- 'j'
> Try 'ls --help' for more information.
> ```
> {: .error}
{: .callout}

### Getting help

`ls` has lots of other **options**. You can use `ls --help` as shown in the output or by using the `man` (manual) command.

```
$ man ls
```
{: .language-bash}

```
Usage: ls [OPTION]... [FILE]...
List information about the FILEs (the current directory by default).
Sort entries alphabetically if neither -cftuvSUX nor --sort is specified.

Mandatory arguments to long options are mandatory for short options, too.
  -a, --all                  do not ignore entries starting with .
  -A, --almost-all           do not list implied . and ..
      --author               with -l, print the author of each file
  -b, --escape               print C-style escapes for nongraphic characters
      --block-size=SIZE      scale sizes by SIZE before printing them; e.g.,
                               '--block-size=M' prints sizes in units of
                               1,048,576 bytes; see SIZE format below
  -B, --ignore-backups       do not list implied entries ending with ~
  -c                         with -lt: sort by, and show, ctime (time of last
                               modification of file status information);
                               with -l: show ctime and sort by name;
                               otherwise: sort by ctime, newest first
  -C                         list entries by columns
      --color[=WHEN]         colorize the output; WHEN can be 'always' (default
                               if omitted), 'auto', or 'never'; more info below
  -d, --directory            list directories themselves, not their contents
  -D, --dired                generate output designed for Emacs' dired mode
  -f                         do not sort, enable -aU, disable -ls --color
  -F, --classify             append indicator (one of */=>@|) to entries
...        ...        ...
```
{: .output}

To navigate through the `man` pages,
you may use <kbd>↑</kbd> and <kbd>↓</kbd> to move line-by-line,
or try <kbd>B</kbd> and <kbd>Spacebar</kbd> to skip up and down by a full page.
To search for a character or word in the `man` pages,
use <kbd>/</kbd> followed by the character or word you are searching for.
Sometimes a search will result in multiple hits. If so, you can move between hits using <kbd>N</kbd> (for moving forward) and <kbd>Shift</kbd>+<kbd>N</kbd> (for moving backward).

To **quit** the `man` pages, press <kbd>Q</kbd>.
> ## Manual pages on the web
>
> Of course, there is a third way to access help for commands:
> searching the internet via your web browser.
> When using internet search, including the phrase `unix man page` in your search
> query will help to find relevant results.
>
> GNU provides links to its
> [manuals](http://www.gnu.org/manual/manual.html) including the
> [core GNU utilities](http://www.gnu.org/software/coreutils/manual/coreutils.html),
> which covers many commands introduced within this lesson.
{: .callout}

> ## Exploring More `ls` Flags
>
> You can also use two options at the same time. What does the command `ls` do when used
> with the `-l` option? What about if you use both the `-l` and the `-h` option?
>
> Some of its output is about properties that we do not cover in this lesson (such
> as file permissions and ownership), but the rest should be useful
> nevertheless.
>
> > ## Solution
> > The `-l` option makes `ls` use a **l**ong listing format, showing not only
> > the file/directory names but also additional information, such as the file size
> > and the time of its last modification. If you use both the `-h` option and the `-l` option,
> > this makes the file size '**h**uman readable', i.e. displaying something like `5.3K`
> > instead of `5369`.
> {: .solution}
{: .challenge}
<!-- ```
$ ls -s Desktop/shell-lesson-data/data
total 116
 4 amino-acids.txt   4 animals.txt   4 morse.txt  12 planets.txt  76 sunspot.txt
 4 animal-counts     4 elements      4 pdb         4 salmon.txt
$ ls -S Desktop/shell-lesson-data/data
sunspot.txt  animal-counts  pdb        amino-acids.txt  salmon.txt
planets.txt  elements       morse.txt  animals.txt
```
{: .output}

Putting all that together, our command above gives us a listing
of files and directories in the root directory `/`.
An example of the output you might get from the above command is given below:

```
$ ls -F /
```
{: .language-bash}

```
Applications/         System/
Library/              Users/
Network/              Volumes/
```
{: .output}

Now lets create a file to go into our new folder.

`nano` is a minimal command line text editor, you will probably have other more convenient means of editing files on the cluster, but being able to use a command line text editor will probably come in handy.

Enter the command `nano` followed by a filename, this will create, then open the file. If you are sharing this directory with other people make sure you don't use the same filename as them. 

```
{{ site.remote.prompt }} nano myFile.txt
```
{: .language-bash}

TODO: More about vim.

```
{{ site.remote.prompt }} ls
```
{: .language-bash}

```
directoryname  filename.txt
```
{: .output}

#TODO: Suitable directory to list files in.

These three commands are the basic commands for navigating the filesystem on your computer:
`pwd`, `ls`, and `cd`. Let's explore some variations on those commands. What happens
if you type `cd` on its own, without giving
a directory?

```
$ cd
```
{: .language-bash}

How can you check what happened? `pwd` gives us the answer!

```
$ pwd
```
{: .language-bash}

```
/home/usr123
```
{: .output}

It turns out that `cd` without an argument will return you to your home directory,
which is great if you've gotten lost in the filesystem.

> ## Two More Shortcuts
>
> The shell interprets a tilde (`~`) character at the start of a path to
> mean "the current user's home directory". For example, if Nelle's home
> directory is `/home/nelle`, then `~/data` is equivalent to
> `/home/nelle/`. This only works if it is the first character in the
> path: `here/there/~/elsewhere` is *not* `here/there/Users/nelle/elsewhere`.
>
> Another shortcut is the `-` (dash) character. `cd` will translate `-` into
> *the previous directory I was in*, which is faster than having to remember,
> then type, the full path.  This is a *very* efficient way of moving
> *back and forth between two directories* -- i.e. if you execute `cd -` twice,
> you end up back in the starting directory.
>
> The difference between `cd ..` and `cd -` is
> that the former brings you *up*, while the latter brings you *back*.
>
{: .callout}

#TODO: talk about mv to move into directory.

#TODO: mention `nano mydirectory/myfile.txt` as alternative.

[Arguments]: https://swcarpentry.github.io/shell-novice/reference.html#argument -->