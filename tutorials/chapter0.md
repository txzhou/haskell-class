# Introduction

[Haskell](http://www.haskell.org/haskellwiki/Haskell) is a purely functional language. Unlike Scala, there is no escape hatch--if you want to write imperative code in Haskell, you are forced to use the the IO and ST monads, and/or a higher-level library built atop these data types (technically, Haskell does include some functions, like unsafePerformIO, that allow you to subvert its purity. But these functions are used extremely rarely, as they don't mix well with Haskell's laziness.) This lack of escape hatch has been one reason why many discoveries of how to write functional programs have come from the Haskell community--in Haskell, the question, "how do I express this program using pure functions?" is not merely an intellectual exercise, it is a prerequisite to getting anything done!

Haskell is in many ways a nicer language for functional programming than Scala, and if you are serious about learning more FP, we recommend learning it. We recommend this even if you continue to program predominantly in Scala. 

## Part 0: Environment Setup

First things first. Lets all make sure we have Haskell installed and we have a decent text editor. To get Haskell we will use [Homebrew](http://brew.sh/), which is a package manager for OSX. Check to 


### Task (0a): Install Homebrew

Check to see whether you have Homebrew installed by opening up a Terminal (`/Applications/Utilities/Terminal.app/`) and enter the following code:

`which brew`

If you get back the pathname `/usr/local/bin/brew` then you are good to go. Otherwise you do not have Homebrew installed. Never fear though it's a cinch to set up, just enter the following into your Terminal:

`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

A tutorial should have roughly ten Tasks (a few more or less is ok depending on Task difficulty), labs can have more. Each Task should have some deliverable source code (often in the form of a function implementation). Tasks in Part 0 should generally correspond to project setup or data acquisition.

### Task (0b): Install GHC

With Homebrew installed, getting Haskell is a piece of cake, simply enter the following into your Terminal:

`brew install ghc`

GHC stands for ['Glasgow Haskell Compiler'](https://wiki.haskell.org/GHC). The 'Glasgow' comes from the fact that GHC was originally written by Kevin Hammond in 1989 at the University of Glasgow. A compiler is a computer program (actually a set of programs) that transforms source code written in a programming language (the source language) into another computer language (the target language), with the latter often having a binary form known as object code. The most common reason for converting source code is to create an executable program. GHC is an open source native code compiler for Haskell, and it is [itself written in Haskell](https://ghc.haskell.org/trac/ghc/wiki/Commentary/Compiler). If this sounds weird that's because it is. Compilers are cool.

### Task (0b): Fire up GHCi

One of the programs that comes with GHC is the interpreter GHCi. An interpreter (sometimes called a [REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop)) is an interactive computer programming environment that takes inputs, evaluates them, and returns the result in a call-and-reponse fashion very much like a [Unix shell](https://en.wikipedia.org/wiki/Unix_shell) (which is what your Terminal actually is). 

Start GHCi by simply typing `ghci` into your shell. You should see a prompt similar to the one below. 

```
cem3394$ ghci
GHCi, version 7.10.3: http://www.haskell.org/ghc/  :? for help
```

Take the tip and have a look at the help menu:

```
Prelude> :?
 Commands available from the prompt:

   <statement>                 evaluate/run <statement>
   :                           repeat last command
   :{\n ..lines.. \n:}\n       multiline command
   :add [*]<module> ...        add module(s) to the current target set
   :browse[!] [[*]<mod>]       display the names defined by module <mod>
                               (!: more details; *: all top-level names)
   :cd <dir>                   change directory to <dir>
   :cmd <expr>                 run the commands returned by <expr>::IO String
   :complete <dom> [<rng>] <s> list completions for partial input string
   :ctags[!] [<file>]          create tags file for Vi (default: "tags")
                               (!: use regex instead of line number)
   :def <cmd> <expr>           define command :<cmd> (later defined command has
                               precedence, ::<cmd> is always a builtin command)
   :edit <file>                edit file
   :edit                       edit last module
   :etags [<file>]             create tags file for Emacs (default: "TAGS")
   :help, :?                   display this list of commands
   :info[!] [<name> ...]       display information about the given names
                               (!: do not filter instances)
   :issafe [<mod>]             display safe haskell information of module <mod>
   :kind[!] <type>             show the kind of <type>
                               (!: also print the normalised type)
   :load [*]<module> ...       load module(s) and their dependents
   :main [<arguments> ...]     run the main function with the given arguments
   :module [+/-] [*]<mod> ...  set the context for expression evaluation
   :quit                       exit GHCi
   :reload                     reload the current module set
   :run function [<arguments> ...] run the function with the given arguments
   :script <filename>          run the script <filename>
   :type <expr>                show the type of <expr>
   :undef <cmd>                undefine user-defined command :<cmd>
   :!<command>                 run the shell command <command>

 -- Commands for debugging:

   :abandon                    at a breakpoint, abandon current computation
   :back                       go back in the history (after :trace)
   :break [<mod>] <l> [<col>]  set a breakpoint at the specified location
   :break <name>               set a breakpoint on the specified function
   :continue                   resume after a breakpoint
   :delete <number>            delete the specified breakpoint
   :delete *                   delete all breakpoints
   :force <expr>               print <expr>, forcing unevaluated parts
   :forward                    go forward in the history (after :back)
   :history [<n>]              after :trace, show the execution history
   :list                       show the source code around current breakpoint
   :list <identifier>          show the source code for <identifier>
   :list [<module>] <line>     show the source code around line number <line>
   :print [<name> ...]         show a value without forcing its computation
   :sprint [<name> ...]        simplified version of :print
   :step                       single-step after stopping at a breakpoint
   :step <expr>                single-step into <expr>
   :steplocal                  single-step within the current top-level binding
   :stepmodule                 single-step restricted to the current module
   :trace                      trace after stopping at a breakpoint
   :trace <expr>               evaluate <expr> with tracing on (see :history)

 -- Commands for changing settings:

   :set <option> ...           set options
   :seti <option> ...          set options for interactive evaluation only
   :set args <arg> ...         set the arguments returned by System.getArgs
   :set prog <progname>        set the value returned by System.getProgName
   :set prompt <prompt>        set the prompt used in GHCi
   :set prompt2 <prompt>       set the continuation prompt used in GHCi
   :set editor <cmd>           set the command used for :edit
   :set stop [<n>] <cmd>       set the command to run when a breakpoint is hit
   :unset <option> ...         unset options

  Options for ':set' and ':unset':

    +m            allow multiline commands
    +r            revert top-level expressions after each evaluation
    +s            print timing/memory stats after each evaluation
    +t            print type after evaluation
    -<flags>      most GHC command line flags can also be set here
                         (eg. -v2, -XFlexibleInstances, etc.)
                    for GHCi-specific flags, see User's Guide,
                    Flag reference, Interactive-mode options

 -- Commands for displaying information:

   :show bindings              show the current bindings made at the prompt
   :show breaks                show the active breakpoints
   :show context               show the breakpoint context
   :show imports               show the current imports
   :show linker                show current linker state
   :show modules               show the currently loaded modules
   :show packages              show the currently active package flags
   :show paths                 show the currently active search paths
   :show language              show the currently active language flags
   :show <setting>             show value of <setting>, which is one of
                                  [args, prog, prompt, editor, stop]
   :showi language             show language flags for interactive evaluation
```

That's a lot of stuff! Don't worry we won't need most of it anytime soon. One useful thing to know is that many of these commands can be run just with their first letter (e.g. `:q` instead of `:quit`). 

For now lets set multi-line mode (`:set +m`). This lets us define multi-line functions in the REPL, which will be useful in a minute.

### Task (0c): Install Sublime

We're almost ready to rock. The last thing we need is a decent text editor. A text editor is a program for editing source code. Text editors are not the same as word processing environments like Word or Pages, which add lots of extra formatting data to text that compilers like GHC don't understand. 

Picking a text editor is kind of like picking a car. There are lightweight agile ones like emacs or vim, and there are heavy feature-rich ones like Eclipse. For now let's go with [Sublime](https://www.sublimetext.com/), which is sort of like the Hyundai Elantra of text editors. Later when you are a Haskell ninja you can move over to [Yi](http://yi-editor.github.io/pages/installing/).

---

## Part 1: Writing Haskell Programs

Let's keep it old skool and start by creating a simple `Hello world!` program. 

### Task (1a): Hello World in Haskell

Open up a new file in Sublime and paste in the following code:

``` Haskell
module Main where   -- A comment, starting with `--` 

-- Equivalent to `def main: IO[Unit] = ...` in Scala
main :: IO ()
main = putStrLn "Hello world!!"
```

Then save the file with the name `Main.hs`. 

Already we can see some interesting language elements. Module declarations start with the keyword `module`, then are followed by a module name (here `Main`, but `Foo.Bar.Baz` and `Data.List` are also valid module names), then the keyword `where`. Like Java, module names must follow directory structure, so `Foo.Bar` must live in a file called `Bar.hs` inside the `Foo/` directory.
  
The entry point of a Haskell program is the `main` function. In Haskell, type signatures _precede_ the declaration of the value. So `main :: IO ()` is a type signature, preceding the definition of `main = putStr "Hello world!!"` on the subsequent line. We pronounce the `::` symbol as 'has type'.

Since Haskell is pure, it uses an `IO` type to represent computations that may interact with the outside world. Unlike Scala, `IO` is built into the language--you do not need to write your own `IO` type like we did in chapter 13. And there are a couple other differences in how we write type signatures--the `Unit` type from Scala is written as `()` in Haskell. (Like Scala, it has a single value, written `()`) And _type application_ uses juxtaposition, rather than square brackets. For comparison:

* `IO[Unit]` (Scala) vs `IO ()` (Haskell)
* `Map[String,Double]` vs `Map String Double`
* `Map[String,Map[K,V]]` vs `Map String (Map k v)`: In Haskell, type variables must begin with a lowercase letter. Names starting with uppercase are reserved for concrete types and data constructor names. 

### Task (1b): Compilation

We can compile `Main.hs` using GHC from a new Terminal window like so: 

```
cem3394$ ghc --make Main
[1 of 1] Compiling Main             ( Main.hs, Main.o )
Linking Main ...
```

First however you're going to have to make sure you're in the right directory. Ask your neighbor or teach yourself the `pwd`, `cd` and `ls` [commands in Unix](http://mally.stanford.edu/~sr/computing/basic-unix.html). 

What is linking? Ah, I was hoping you'd ask! Here's another Wikipedia [rabbit hole](https://en.wikipedia.org/wiki/Linker_(computing)) for you.

### Task (1c): Execution

Now that your `Main.hs` code is compiled, try and run it as an executable in your Terminal:

```
cem3394$ ./Main
Hello world!!
```

You should also try starting a REPL and loading your `main` fuction in this way:

```
cem3394$ ghci
GHCi, version 7.10.3: http://www.haskell.org/ghc/  :? for help
Prelude> :l Main
[1 of 1] Compiling Main             ( Main.hs, interpreted )
Ok, modules loaded: Main.
*Main> main
Hello world!!
*Main>
```

Congrats!! You've written and compiled your first Haskell program! You are now a Haskell progammer! In classic LinkedIn style you should put this skill on your page immediately and ask your neighbors for endorsements. Watch the recruiting emails come rolling in. 

---

## Resources

Include other relevant references (blog/SO posts, articles, books, etc) here.