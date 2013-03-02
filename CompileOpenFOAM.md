# How to compile OpenFOAM

I've tried compiling many ways, but below are the instructions that gave me a working copy. There may be some missing steps, so if something comes across as strange, challenge it!

# Compiling OpenFOAM 2.1.1 with the system OpenMPI-1.4.3 and gcc-4.3.4

We're going to use the system OpenMPI and gcc 4.3.4. 

Let's start by making sure all of our files are actually kept in our projects directory rather than our home directory. Wherever I have `username`, you should substitute in your username (for example, I would use spal1313).

First, let's link a folder in our PROJ directory into our HOME directory:
-  Add `export PROJ=/projects/username` to your .my.bashrc
-  run `source $HOME/.bashrc`
-  run `mkdir $PROJ/OpenFOAM`
-  link the newly made file into your home directory by running `ln -s $PROJ/OpenFOAM $HOME/OpenFOAM`. You can check that this worked by running `touch $HOME/OpenFOAM/test`, and if it didn't work, try reversing the last two arguments to `ln`.

Now, go ahead and download all of the source files onto your machine. I like to create a `source` directory, but that's just me. `tar xvf` them into your `$HOME/OpenFOAM` folder.

Add the following to the bottom of your .my.bashrc file:

<pre><code>export FOAM_INST_DIR=$HOME/OpenFOAM
foamDotFile=$FOAM_INST_DIR/OpenFOAM-2.1.1/etc/bashrc
[ -f $foamDotFile ] && . $foamDotFile</pre></code>

and run the command `source .bashrc` .

Let's now prepare the system to use the needed programs. Create a file named in `$HOME/useOpenFOAM.sh` and include the following in it:

<pre><code>#!/bin/bash
. /curc/tools/utils/dkinit
use Moab > /dev/null
use Torque > /dev/null
use .zlib-1.2.5 > /dev/null
use .flex-2.5.35 > /dev/null
use .gcc-4.3.4 > /dev/null
use OpenMPI-1.4.3 > /dev/null</code></pre>

Then add `source ~/openFoamUse.sh` to the bottom of your .my.bashrc file.

Tell OpenFOAM to use this system OpenMPI by changing line 84 in $WM_PROJECT_DIR/etc/bash to SYSSTEMOPENMPI
