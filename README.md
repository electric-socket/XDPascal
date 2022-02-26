# XDPascal
## The successor to XDPW: XDPascal.

### February 25, 2022

A while ago I worked on revising XDPW (XDPascal for Windows) after Vasily Tereshkov decided he was no longer interested. I made a number of changes. And I think they helped. However, I found another compiler, MadPascal, and I decided to use it. i cloned the repository for it at https://github.com/electric-socket/Mad-Pascal. It is a compiler for the Atari computer, but has many features usually only found in a full-service compiler like Free Pascal, such as conditional compilation. Things I wanted to put in XDPW, but they're already here. As with XDPW, it is written using essentially only Standard Pascal, but supports extended pascal, with units, include files, and other features. It is missing a few things like the IN operator, which I  can borrow th code for that from XDPW.

Right now, I have to clean up the code a little to get it to compile (Free Pascal requires enumerated constants to be in ascending order, string types (because they are dynamic) can't be in the variant part of a record, and a few other things. Then I have to strip out all the code generation, as I will have code generation to be separate from the lexer and the parser of the compiler.  The compiler should not know how the code generation works, it should just call the various functions to tell the code generator to set up or tear down a segment, e.g. something like (for a FOR statement):
```
    ForStatementPrelude;
    // various code to intialize control var
    SetForControlVar(ident);
    setForCompletionValue;
    skipoverForloop; // step over loop control
    setForLoopreturn; // come back here at end or continue
    incdecForControlvar(ident);
    testForloop;
    startForStatement;
    // statement or block goes here
    endforstatement; // loop back from here
    forPostlude; // anything that needs to be done after the FOR completes
    
```
Then I want to make sure the compiler is self-hosting, i.e. can it parse itself? Then add code generation.

So I decided to start over. The original code for XDPW is located at https://github.com/electric-socket/xdpw until I'm able to upload, then I'll start here. Sometimes you find it's worth rebuilding from scratch.
