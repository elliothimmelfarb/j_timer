j_timer
=======================

### Updating from an older version
The older version of this project stored the Timetrap database inside of the VM. The
current revision stores the database in the project directory on the host system, and
makes `/home/vagrant` into an additional directory in addition to `/vagrant`.

Before updating to run on this version, you need to copy your Timetrap database out of
the existing `/home/vagrant` location and into the presently synced `/vagrant` location.
```
cp /home/vagrant/.timetrap.db /vagrant
```
Then you can destroy or re-provision the VM using vagrant and it will find your old
database.

### What this does
This is just a Vagrantfile that will create an Ubuntu VirtualBox VM to run Timetrap
(https://github.com/samg/timetrap).

To use this VM you need to have installed:
* Virtualbox - https://www.virtualbox.org
* Vagrant - https://www.vagrantup.com

If you are running on Windows it is recommended that you use the `git-bash` application
that comes with Git for Windows. This is different from the Github Desktop application.
* Git for Windows - https://git-for-windows.github.io/

You can probably also get this working using Powershell, cmd.exe, or cygwin, along with an
ssh application like PuTTY, but you're on your own for figureing that out.

### Running using Git-Bash
Openning the `git-bash` application will give you a minimal `bash` command line. It only
has a few features, but it does include the bare minimum needed to make this work.

First, navigate to the directory you want this to live in, then run the following command
to obtain the code:
```
# This creates a new 'j_timer' subdirectory in your current working directory and
# downloads this repository to that location.
git clone https://github.com/jjcad/j_timer

# Enter the new j_timer subdirectory
cd j_timer

# Start the virtual machine. The first time you ever do this may take awhile since vagrant
# may have to download a copy of Ubuntu before it can launch the VM. Allow yourself an hour 
# for this. This will also run the 'provision' block of code on the first run which will
# install sqlite, ruby, and timetrap for you.
vagrant up
```

### After the machine has returned to a plain command prompt
Now you can ssh into the machine and start using it. If it asks for a password, enter 
`vagrant`.
```
# SSH into the machine using vagrant
vagrant ssh
```

### If you don't want to use ssh 
You can alternatively have `vagrant up` launch a Virtualbox GUI window that will give you a
command prompt if you so choose. If this is your desired way to interact with the VM then
simply open the Vagrantfile in a text editor, and switch `vb.gui = false` to `vb.gui = true`
and you should be all set. If you make this change after you have already run 
`vagrant up` and the VM is running right now, then you'll need to bring the VM down
using `vagrant halt` before launching it again with `vagrant up` in order to get that
window.

# SUPER IMPORTANT
The first time you launch a fresh Vagrant VM the VM will default to using UTC. Obviously,
this is a problem for a time-tracking application where you would expect it to be using
local time. As such, on the first run it is highly suggested that you change the system's
timezone. If you travel and want it to use the local timezone you can re-run this to change
the timezone for as long as you're there, and then run it again when you get back to your
original timezone.
```
sudo dpkg-reconfigure tzdata
```

### Vagrant cheatsheet
```
vagrant up #Launches a VM
vagrant ssh #Will SSH you into a terminal session in the VM if you're in a POSIX-compatible shell.
vagrant halt #Shuts a VM down. You should do this before shutting your host computer off or putting it to sleep.
vagrant destroy #Will destroy a botched VM.

/vagrant #The directory in the VM that corresponds to the directory on your host system that the VM lives in.
#You can pass through files from the host to the VM and vice-versa through this directory.
/home/vagrant #The directory you are defaulted to when you ssh in.
#This direcotry is also synced with the host system's directory, so you shouldn't have to
#use /vagrant unless you really want to.

# The next two are very useful since they are a bit faster than vagrant up/halt.
vagrant suspend #Will freeze the VM's state and bring it down.
vagrant resume #Will unfreeze a VM's state and make it run again.
```
There's a lot more to Vagrant than this, but the above is the basics necessary
to interact with the VM. Check out https://www.vagrantup.com for more in-depth tutorials.

### Timetrap cheatsheet
```
t sheet oper.bdev.strt #Will check you into a sheet named 'oper.bdev.strt'
t in Work on the thing #Will start a timer in the sheet you're checked into with the comment message 'Working on the thing.'
t out #Will stop the timer for the sheet you're checked into.
t in --at '10 minutes ago' Work on something else #Will start a timer, backlogging it to 10 minutes ago, with the comment message 'Working on something else.'
t out --at '5 minutes ago' #Will check you out from the current sheet's timer as of 5 minutes ago.
t in --at '2017-01-01 09:00' Working on New Years Day. #Will automatically clock you in at that time. You can also put a discreet time in the past.
t out --at '2017-01-01 10:00' #Will automatically check out you at this time.

t list #Will show you a list of all your timesheets aka billing codes.

t today #Will generate a report for time spent in the current sheet for just today.
t today all #Same as above, but includes all the timesheets you've worked in today.

t yesterday #Same as above, but for yesterday.
t yesterday all #Same as above, but for yesterday.

t week #Same as above, but since Sunday of this week.
t week all #Same as above, but since Sunday of this week.

t today all -f csv #Same as above, but CSV formatted.
t week all -f csv #Same as above, but CSV formatted.

t backend #Gets you into the sqlite backend in case you need to fix stuff or do some special research using SQL.

t display all --start 'YYYY-MM-DD' --end 'YYYY-MM-DD' #Displays all timesheet entries between two dates or timestamps.
```

### Linux command line cheatsheet
```
| #Pipes the output from the command preceding this into the input for the command following it.
> #Pipes the output from the command preceding this into some connection, like a file.
grep #Can be used to search through text.
less #Like 'more' in cmd.exe, it shows a screen of text that you can scroll through.
```

### Examples:
```
t list | grep oper #Will search all of your teamsheets for those containing the text 'oper' and print them out.

t --help | less #Makes it easier to read through Timetrap's help documentation.

t week all -f csv > thisWeek.csv #Creates a CSV containing all of your 't in' and 't out' information for all timesheets for the week.

t week all -f csv | nano - #Pipes the csv report in the nano text editor so you can manually change the csv before saving it.
t week all -f csv | vim - #Pipes the csv report in the vim text editor so you can manually change the csv before saving it.
```

