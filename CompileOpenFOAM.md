# How to compile OpenFOAM

I've tried compiling many ways, but below are the instructions that gave me a working copy. There shouldn't be any missing steps, but if something comes across as strange, challenge it!

# Compiling OpenFOAM 2.1.1 with the system OpenMPI-1.4.3 and gcc-4.3.4

We're going to use the system OpenMPI and gcc 4.3.4. 

## Organizing everything
Let's start by making sure all of our files are actually kept in our projects directory rather than our home directory. Wherever I have `username`, you should substitute in your username (for example, I would use spal1313).

First, let's link a folder in our PROJ directory into our HOME directory:
-  Add `export PROJ=/projects/username` to your .my.bashrc
-  run `source $HOME/.bashrc`
-  run `mkdir $PROJ/OpenFOAM`
-  link the newly made file into your home directory by running `ln -s $PROJ/OpenFOAM $HOME/OpenFOAM`. You can check that this worked by running `touch $HOME/OpenFOAM/test`, and if it didn't work, try reversing the last two arguments to `ln`.

Now, go ahead and download all of the source files onto your machine. I like to create a `source` directory, but that's just me. `tar xvf` them into your `$HOME/OpenFOAM` folder. You should now have `$HOME/OpenFOAM/OpenFOAM-2.1.1` and `$HOME/OpenFOAM/ThirdParty-2.1.1` folders.

Add the following to the bottom of your .my.bashrc file:

<pre><code>export FOAM_INST_DIR=$HOME/OpenFOAM
foamDotFile=$FOAM_INST_DIR/OpenFOAM-2.1.1/etc/bashrc
[ -f $foamDotFile ] && . $foamDotFile</code></pre>

and run the command `source ~/.bashrc` .

## Prepare the system for programs to be used

Let's now prepare the system to use the needed programs. Create a file named in `$HOME/useOpenFOAM.sh` and include the following in it:

<pre><code>#!/bin/bash
. /curc/tools/utils/dkinit
use Moab > /dev/null
use Torque > /dev/null
use .zlib-1.2.5 > /dev/null
use .flex-2.5.35 > /dev/null
use .gcc-4.3.4 > /dev/null
use OpenMPI-1.4.3 > /dev/null</code></pre>

Then add `source ~/useOpenFOAM.sh` to the bottom of your .my.bashrc file.

Tell OpenFOAM to use this system OpenMPI by changing line 84 in $WM_PROJECT_DIR/etc/bashrc to SYSTEMOPENMPI.

## Begin the compile

Now, log into a compile node (for example `ssh janus-compile1`). If you're already in a compile node, run `source ~/.bashrc` just to be safe.

Run `foamSystemCheck`. If you get any errors, or your system can't find the file, then something has gone wrong.

Move into the source code by running `cd $WM_PROJECT_DIR`, and kick off the compile with `./Allwmake 2>&1 | tee make.log`.

## Testing Everything

Ryan created the following super helpful test instructions:

To test the installation, you can run `foamInstallationTest`.

Then create a directory for the tutorials within `mkdir -p $FOAM_RUN`.

Copy the tutorials to the run directory with `cp -r $FOAM_TUTORIALS $FOAM_RUN`

Try to run laminar flow in a cavity:
- `cd $FOAM_RUN/tutorials/incompressible/icoFoam/cavity`
- `blockMesh`
- `icoFoam`
