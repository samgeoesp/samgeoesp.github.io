# Anatra and PC Setup

### 1. Risk Assessment

Complete your risk assessment form

___

### 2. User Lists

Ask Matt to get you added to the list of Anatra and Gaussian users.

Familiarise yourself with the 'Introduction to the Anatra HTC service':
https://research-software-skills-bath.github.io/intro-anatra/00_schedule.html

___

### 3. Install Software

Ensure the following essential software is installed on your PC (if not ask IT to install): CYLview, ChemDraw, Outlook, MobaXterm, OpenOffice Calc, Slack, Word, Powerpoint, Maestro, MacroModel, TightVNC

___

### 4. Set Up MobaXTerm

Open MobaXterm. This is used to open and manage terminal windows, and hence access the University HPC network (Anatra) and manage and transfer files between your local storage and Anatra.

To set up a balena session in MobaXTerm: click session -> SSH -> and fill in information the following:
 - remote host: anatra.bath.ac.uk
 - specify username: your bath id (e.g. ehef20)
 - port number: 22

To set up transfer from local storage to balena in MobaXTerm: click session -> SFTP -> and fill in information the following:
 - remote host: anatra.bath.ac.uk
 - specify username: your bath id (e.g. ehef20)
 - port number: 22

Alternatively, your H-drive can be accessed on Anatra via $BUCSHOME. For example files can be copied from Anatra to H-drive with `cp file $BUCSHOME`

These sessions, once set up, can be quick-accessed in the future from the side bar on the left.

Note: passwords do not appear (elab)

___

### 5. Getting Used to UNIX

The UNIX language is used to interact with the terminal.

Learn and get used to the following basic unix commands (ls, cp, rm, mv, cat, head, tail, cd, pwd, mkdir, rmdir) from this site: https://www.unixtutorial.org/basic-unix-commands
Also grep: grep string filename(s) = looks for the string in the files
Also nano: this is used to edit files. If editted, use ctrl + O -> enter to save, and ctrl + X to exit.
When changing directory, `cd` alone returns to the home directory, as does `cd ~/`, whilst `cd ..` moves back one directory.

An asterisk `*` is used as a wildcard, e.g. `ls *.gjf` will list all files ending in .gjf. Be careful with this, especially when removing files (please avoid using `rm *`).
The tab key can be used to auto complete words where the name is unambigious (e.g. ab won't autocomplete to abc if there is also a file abd in that directory)

___

### 6. Set up folders:

New PhDs:
- When you first set up Anatra you will need to email Abhishek (ak3353@bath.ac.uk) and get set up with a project. New PhD's will be given their own unix group (/scratch/projects/pra-000x) in the scratch directory. This is where 

New Masters/Project Students
- Ask Matt/PhD student to get you set up with a unix group. This will likely be Matt's unix group (pra-0002). When you have access to this directory, create a new folder with your username and work in there (e.g. /scratch/projects/pra-0002/sge28 ).

Now you can create any subdirectories for your project work. Also, create a scripts directory with the following command:

`mkdir scripts`

___

### 7. Setting up the .bashrc

In the simplest terms, the .bashrc is a script that is run whenever a terminal a loaded. You can put any command in the .bashrc file that you could type at the command prompt, and it will be run upon opening a terminal. This can save a lot of time by automatically loading and initialising certain modules and programmes (etc), rather than having to do this manually each time you start a new terminal.
** When editing your .bashrc, have two sessions open. This means that if there is an error in your .bashrc you can still access Anatra. **

Under "# User specific aliases and functions" add the following lines (if not already present) to the .bashrc (`nano ~/.bashrc`)

`module load gcc`

`module load slurm`

`module load gaussview`

`module load Anaconda3`

`module load TurboVNC`

`export PATH=/scratch/projects/pra-000x/scripts:$PATH`

Replace `/pra-000x/` with the path to your own scripts directory (use `pwd` whilst in your scripts directory).

Once changes are made the .bashrc the terminal must be reloaded for them to come into effect.

See `examples/bashrc_template.txt` for a basic template of a .bashrc.
___

### 8. Download Useful Resources

Clone the useful_resouces repository from GitHub with the following command: `git clone https://github.com/the-grayson-group/useful_resources.git`

Copy any useful scripts to your Scripts folder, e.g. q, gaussian16_submit.sh, stationary_points.sh, etc

Edit all scripts to replace any instance of `home/d/ehef20` with your own home directory (use `pwd` whilst in your home directory).
Edit all scripts to replace any instance of `home/d/ehef20/scratch` or `/path_to_scratch` with your own scratch direcotry (use `pwd` whilst in your scratch).

Activate each script by typing `chmod u+x [scriptname]` for each script. Once activated a script should turn green. These scripts should be usable from any directory.

___

### 9. Creating a Computational Chemistry Environment

Anatra has a different build to that of Balena in order to protect the new HPC and prevent downtime. Currently, this means that for certain modules to be loaded, we must create and environment for these (namely Goodvibes and Openbabel)

**One time creation of environment**

`conda create -n comp_chem`

`conda activate comp_chem`

`conda install -c patonlab -c conda-forge Goodvibes OpenBabel`

comp_chem is very much a suggested name - call this whatever is memorable/easy to type.

*Add the following line to your* `~/.bashrc` *after you load your modules.*

`conda activate comp_chem`

This will now load your environment every time you login meaning you can utilise both OpenBabel and  GoodVibes.


> In short, GoodVibes is a program used to extract useful thermochemical data from our calculations.
> OpenBabel is used for conversion of files between commonly used file types (e.g. .out to .gjf).
> 
> If there is further programs you wish to add to this environment then follow conda set up outline in that programs documentation. If you do not want any new programs in this environment remember to `conda deactivate` before setting these programs up.

___

### 10. Gaussian

Gaussian is a program we use for electronic structure modelling. Although we can use many of gaussian's functionalities in the command line, it is often useful to access gaussian using gaussview. This allows the visulation of molecules, and provides a useful interface for managing, running and analysing calculations.

*Warning*: never run a calculation directly through gaussview. Gaussian calculations should always be initiated in the command line using the relevant submission scripts (see job_submission folder)

Wiki of Gaussian error messages: https://docs.computecanada.ca/wiki/Gaussian_error_messages

The method of accessing Gaussview has changed from Balena. Below are instructions:

1. If you are on Windows, download TightVNC software. *NOTE*: If you are on a University of Bath device you will need to email IT service desk to download this software.
2. `vncserver -otp`

> This will generate a VNC session (anatra-01:x) a one time password that can be used to login to a VNC session.

3. Open TightVNC and connect to your session. Remote Host: anatra.bath.ac.uk:590x. You will then be prompted for your password.
4. This will now have now loaded a Red Hat Enterprise Linux window. Click Activities -> Terminal and use the `gview` command to open Gaussview.

This session will last for 6 hours. It is possible for you to lock this Linux session by inactivity. If this happens then hit the enter key and type in your university password to unlock the Linux session.

See `examples/gview_anatra.mp4` for a quick guide.  
___

### 11. Accessing Anatra remotely (off campus)

To access Anatra from your personal laptop or any device off campus you must first set up a VPN to the Bath network (see https://www.bath.ac.uk/guides/setting-up-vpn-on-your-device/). Once done, you can access balena at any time by connecting to the VPN and running the following in a terminal window: `ssh -X yourusername@balena.bath.ac.uk`
