# How to compile OpenFOAM

I've tried compiling many ways, but below are the instructions that gave me a working copy. There may be some missing steps, so if something comes across as strange, challenge it!

# Compiling OpenFOAM 2.1.1 with the system OpenMPI-1.4.3 and gcc-4.3.4

We're going to use the system OpenMPI and gcc 4.3.4. 

Let's start by making sure all of our files are actually kept in our projects directory rather than our home directory. Wherever I have `username`, you should substitute in your username (for example, I would use spal1313).

First, let's link a folder in our PROJ directory into our HOME directory:
-  Add `export PROJ=/projects/username` to your .my.bashrc
-  run `source $HOME/.bashrc`
-  run `mkdir $PROJ/OpenFOAM`
-  link the newly made file into your home directory by running `ln -s $PROJ/OpenFOAM $HOME/OpenFOAM`

Now, go ahead and download all of the source files onto your machine. I like to create a `source` directory, but that's just me. `tar xvf` them into your `$HOME/OpenFOAM` folder.
