---
title: "Added Heating Experiments"
questions:
- "How to do the added heating experiments?"
objectives:
keypoints:
- ""
---

## Added Heating

The version of CESM that we are using (CESM 2.1.1) has code and a namelist configuration in the atmosphere that performs atmospheric nudging (also sometimes called relaxation) for zonal wind (U), meridional wind (V), temperature (T), specific humidity (Q), and pressure (PS).  We will use this functionality, modified by Dr. Swenson, to add a constant, 3D, idealized heating to temperature (T).

## Create, Setup and Configure Using a Script

We will setup our case using a Unix c-shell script.  This script will execute all the same commands that we typically perform by hand.

A script is a good method to keep track of what we did to setup our experiment and make sure we can reproduce it.  

First, we will make a place in our home directory for experiment scripts and then copy the script to that location.

From your home directory:

~~~
$ mkdir cases_scripts
$ cd cases_scripts
$ cp ~kpegion/cases_scripts/addheat.csh . 
~~~
{: .language-bash}


Next we will take a look at the script and see what it does.

~~~
$ vi addheat.csh
~~~
{: .language-bash}

### The set commands

At the top, the `set` set variables for the script to use.  You will need to set these correctly for your run, specifically, set the following:

`refcase`: set this to the case you used for Assignment #2. This is a B1850 Pre-industrial control simulation and we will branch from this case for this experiment.

`expname`: set this to the case name you want to use for your added heating experiment

The following are already set correctly, but I want you to know what they mean in case you wish to use this script for something else and need to change it in the future:

`project`: make sure the project number is set to the correct one.

`model`: Make sure this is set to the version of the model you wish to use, it should be your `/glade/work/USERNAME/cesm2.1.1`.  The variables $USER will resolve to your username.

`caseroot`: The path to your case

### The `rm` commands

This deletes the CASEROOT, so you are guaranteed to be starting fresh with your new case.
The second `rm` command deletes your `/glade/scratch/USERNAME/CASENAME`.

Remember the `rm` command deletes without question, so be careful here not to delete a case you want to keep. 

### Create and setup the newcase

The next few lines run the `create_newcase` and `case.setup` scripts. 

### Modify Source code

Remember to modify source code, you need to put it in your `SourceMods` directory.
The next line copies the new source code for the added heating experiment to the `SourceMods` directory fo your new case.

### `xmlchange` commands

All the xmlchange commands to setup and configure your case

### Modify the namelist

The next set of lines labelled `# Variables to output` modifies the `user_nl_cam` namelist to change the output frequency for the atmosphere and set the namelist to configure the options for the added heating (all the `Nudge` options)


### Setup your initial conditions

Since `GET_REFCASE` is set to `false`, the user needs to copy the initial conditions to the run directory.  The next line labelled `# Initial conditions for Branch run` does that for you.

### Build and Submit your run

The last few lines of the code execute the build and submit scripts.

## How to run the script

Make the script executable.

~~~
$ chmod +x addheat.csh
~~~
{: .language-bash}

Run the script

~~~
$ ./addheat.csh
~~~
{: .language-bash}

It will take a few minutes to run since it has to go through all the steps we normally do by hand, including the build step.  Once it is complete, your job will be submitted to the queue and you can take a look using 

~~~
$ qstat -u USERNAME
~~~
{: .language-bash}
