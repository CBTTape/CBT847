# CBT847
Converted to GitHub via [cbt2git](https://github.com/wizardofzos/cbt2git)

This is still a work in progress. 
Due to amazing work by Alison Zhang and Jake Choi repos are no longer deleted.

```
//***FILE 847 is from Sam Golob, who debated with himself whether   *   FILE 847
//*           or not to include this file.  It is up to you to      *   FILE 847
//*           determine whether it is valuable to you or not.       *   FILE 847
//*                                                                 *   FILE 847
//*           THIS FILE IS BEING PLACED HERE, TO LEARN FROM.        *   FILE 847
//*                                                                 *   FILE 847
//*           One thing about the COPYMODS program, is that it      *   FILE 847
//*           effortlessly reads tapes with leading tape marks,     *   FILE 847
//*           showing the data that is afterwards.  See below.      *   FILE 847
//*                                                                 *   FILE 847
//*           A LOADLIB has been added to have executable code.     *   FILE 847
//*           All the programs in the load library were assembled   *   FILE 847
//*           from the source code included here.                   *   FILE 847
//*                                                                 *   FILE 847
//*           The purpose of this file is to show you ONE WAY to    *   FILE 847
//*           add A LOT OF OPTIONS to an existing program, and to   *   FILE 847
//*           keep them all straight.  (No "spaghetti code")        *   FILE 847
//*                                                                 *   FILE 847
//*           This file contains many stages in the development     *   FILE 847
//*           of the COPYMODS program on CBT File 229.  All of      *   FILE 847
//*           these versions are working programs, with the         *   FILE 847
//*           possible exception of versions 54 and 55, which ran   *   FILE 847
//*           out of base registers when I fixed a bug              *   FILE 847
//*           retroactively.  You can assemble them all, and run    *   FILE 847
//*           them all, and use them all.  However, the latest      *   FILE 847
//*           version (member COPYMODS) is (I think) the best one   *   FILE 847
//*           to actually use for your work.                        *   FILE 847
//*                                                                 *   FILE 847
//*           email:   sbgolob@cbttape.org                          *   FILE 847
//*                                                                 *   FILE 847
//*           This file illustrates (in much detail) the process    *   FILE 847
//*           of adding 87 extra versions to the COPYMODS program   *   FILE 847
//*           from CBT File 229.  As of this writing, COPYMODS      *   FILE 847
//*           (which is used to copy and map tapes) is at version   *   FILE 847
//*           level 088, having started as a simple tape copying    *   FILE 847
//*           program to make multiple copies of non-labeled (NL)   *   FILE 847
//*           tapes in one job run.  COPYMODS is now a mighty and   *   FILE 847
//*           powerful tool for dealing with tapes, and since I     *   FILE 847
//*           still have all the version levels (or most of them)   *   FILE 847
//*           that were used as intermediate stages in the process  *   FILE 847
//*           of its development, I didn't want them to get lost.   *   FILE 847
//*           People can see how to put 50 new options into a       *   FILE 847
//*           program, without the code getting lost in a tangle    *   FILE 847
//*           of spaghetti.                                         *   FILE 847
//*                                                                 *   FILE 847
//*           Successive incremental versions of the COPYMODS       *   FILE 847
//*           program are included in this file. It is recommended  *   FILE 847
//*           to use SUPERC or some other comparison program to     *   FILE 847
//*           see the differences between each version and the      *   FILE 847
//*           next.  I did not label each line of code with a       *   FILE 847
//*           version number or indicator.  SUPERC will show the    *   FILE 847
//*           changes between versions in enough detail.            *   FILE 847
//*                                                                 *   FILE 847
//*           An important part of the structure of the COPYMODS    *   FILE 847
//*           program is now its PARM table, which scans either     *   FILE 847
//*           PARM or SYSIN control cards, to set its many options  *   FILE 847
//*           automatically, in one pass.  Options can be           *   FILE 847
//*           displayed at the top of the program, first, the way   *   FILE 847
//*           they were originally set by the defaults, and the     *   FILE 847
//*           PARM and SYSIN cards, and second, after they were     *   FILE 847
//*           changed for consistency with each other, by the       *   FILE 847
//*           program itself.  You can set defaults, and words      *   FILE 847
//*           which stand for multiple bit combinations, both to    *   FILE 847
//*           turn them on and to turn them off, by making entries  *   FILE 847
//*           in the PARM table.                                    *   FILE 847
//*                                                                 *   FILE 847
//*           One nice thing about the COPYMODS program is that     *   FILE 847
//*           it effortlessly reads tapes with leading tape marks,  *   FILE 847
//*           such as VSE tapes, and tells you what data lies on    *   FILE 847
//*           the tape, after the tape marks.  COPYMODS has an      *   FILE 847
//*           option to strip off all leading tapemarks from the    *   FILE 847
//*           beginning of the tape, when making copies.  I didn't  *   FILE 847
//*           get this right, until somewhere in the version 70s    *   FILE 847
//*           (at version 74).  Now, you can follow the process     *   FILE 847
//*           yourself.                                             *   FILE 847
//*                                                                 *   FILE 847
//*           COPYMODS has full support for ASCII tapes.  You can   *   FILE 847
//*           do almost anything with ASCII tapes as you can with   *   FILE 847
//*           IBM (EBCDIC) tapes.  Also, not easy to put in.  You   *   FILE 847
//*           can also watch the development process here.          *   FILE 847
//*                                                                 *   FILE 847
//*           COPYMODS can map a tape without copying it, using     *   FILE 847
//*           its PARM (or SYSIN control) of READ.  If READ is      *   FILE 847
//*           in effect, no output DD names are required.           *   FILE 847
//*                                                                 *   FILE 847
//*           COPYMODS can STRIP labels off a SL tape, saving them  *   FILE 847
//*           in an FB-80 file.  So COPYMODS can make NL tapes      *   FILE 847
//*           from SL tapes while copying them.  In addition,       *   FILE 847
//*           COPYMODS can write back these labels to the NL tape   *   FILE 847
//*           (from the file it previously created) and re-create   *   FILE 847
//*           an SL tape from the NL tape.  (You can edit the       *   FILE 847
//*           labels in the meanwhile.)  I don't know of any        *   FILE 847
//*           other tape tool that can do this.  STRIP support      *   FILE 847
//*           was added in version 50.  One thing that STRIP will   *   FILE 847
//*           do.  It doesn't work by counting (1st, 3rd, 4th,      *   FILE 847
//*           6th files, etc.).  STRIP actually DETECTS whether a   *   FILE 847
//*           tape file is a label, and whatever its file number    *   FILE 847
//*           is, it will eliminate that label file from the        *   FILE 847
//*           output tapes.                                         *   FILE 847
//*                                                                 *   FILE 847
//*           COPYMODS uses a very simple copying mechanism (of     *   FILE 847
//*           course it's EXCP).  COPYMODS reads a block of a       *   FILE 847
//*           tape into a 64K buffer (in the program, so it does    *   FILE 847
//*           not have to be GETMAINed).  COPYMODS then writes      *   FILE 847
//*           the contents of the buffer to as many as 16 output    *   FILE 847
//*           tapes, if DD names OUT1 thru OUT16, or any            *   FILE 847
//*           combination of these, are coded in the JCL.  Once     *   FILE 847
//*           you have the contents of a tape block in a buffer,    *   FILE 847
//*           you can do anything with it while you have it, and    *   FILE 847
//*           with COPYMODS, I have tried to do more and more       *   FILE 847
//*           and more.  There are now 49 options, and you can      *   FILE 847
//*           even INIT up to 16 tapes, too, at one time.           *   FILE 847
//*                                                                 *   FILE 847
//*           I think that by following the development of the      *   FILE 847
//*           COPYMODS program incrementally through its many       *   FILE 847
//*           version levels, you can learn a lot.  If somebody     *   FILE 847
//*           wants to see this, at least it's there for you to     *   FILE 847
//*           look at, and learn from.                              *   FILE 847
//*                                                                 *   FILE 847
//*           Good luck.                                            *   FILE 847
//*                                                                 *   FILE 847
```
