j_timer
=======================

# Note about migrating from a previous version
If you're hoping to migrate from an older version, then there are a few steps you need to take that have not been written up yet. If you choose to do a `vagrant destroy` without following the correct procedure (which has yet to be written), then you will lose your timetrap entries that already exist. Obviously, this is not advisable. 


# Everything below here is what is normally in the README.md file
This is just a Vagrantfile that will create an Ubuntu VirtualBox VM to run Timetrap
(https://github.com/samg/timetrap).

To use this VM you need to have installed:
* Virtualbox - https://www.virtualbox.org
* Vagrant - https://www.vagrantup.com

### One way to get this working
Once the above two apps are installed create a directory in which your VM will live.
Then you can download the Vagrantfile into that direcotry.
Then open up a terminal application like `cmd.exe`, `powershell`, `cygwin`, `git-bash`,
etc. and navigate to the directory you created and in which the Vagrantfile resides.
Now run:
```
vagrant up
```
This will launch the Virtualbox on your behalf using the settings in the Vagrantfile.
It will take some time since it has to do a number of things, potentially including
downloading a copy of Ubuntu. It will also go through the trouble of installing timetrap
plus all of timetrap's dependencies, of which there are several, but not a crazy amount.
It's mostly just the dev versions of `sqlite3` and `ruby`, plus some additional rubygems.

Once you're returned to a command prompt without any error messages you're ready to use
the machine. If you're using `cygwin`, `git-bash` or something like that, then you might
be able to just run:
```
vagrant ssh
```
and find yourself logged into the machine. If you used `cmd.exe` or `powershell` you
might want to use an ssh client like PuTTY to get into the machine.

### Another way to get this working
If you also have `git` and `git-bash` installed, then just open `git-bash`, navigate to
where you want your parent directory to be and run
```
git clone https://github.com/jjcad/j_timer
```
Then navigate into the `j_timer` directory and run `vagrant up`, wait for it download,
boot, and install your software on the first run, then `vagrant ssh` into the box.

### If you don't want to use ssh at all
You can have `vagrant up` also launch a Virtualbox GUI window that will give you a
command prompt if you so choose. If this is your desired way to interact with the VM then
simply open the Vagrantfile in a text editor, and switch `vb.gui = false` to `vb.gui = true`
and you should be all set. If you make this change after you have already run 
`vagrant up` and the VM is running right now, then you'll need to bring the VM down
using `vagrant halt` before launching it again with `vagrant up` in order to get that
window.

### Vagrant cheatsheet
```
vagrant up #Launches a VM
vagrant ssh #Will SSH you into a terminal session in the VM if you're in a POSIX-compatible shell.
vagrant halt #Shuts a VM down. You should do this before shutting your host computer off or putting it to sleep.
vagrant destroy #Will destroy a botched VM.
/vagrant #The directory in the VM that corresponds to the directory on your host system that the VM lives in.
#You can pass through files from the host to the VM and vice-versa through this directory.

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
t out --at '2017-01-01 10:00' #Will automatically check you at this time.

t list #Will show you a list of all your timesheets aka billing codes.

t today #Will generate a report for time spent in the current sheet for just today.
t today all #Same as above, but includes all the timesheets you've worked in today.

t yesterday #Same as above, but for yesterday.
t yesterday all #Same as above, but for yesterday.

t week #Same as above, but since Sunday of this week.
t week all #Same as above, but since Sunday of this week.

t today all -f csv #Same as above, but CSV formatted.
t week all -f csv #Same as above, but CSV formatted.

t backend #Gets you into the sqlite backend in case you need to fix stuff using SQL.

t display all --start 'YYYY-MM-DD' --end 'YYYY-MM-DD' #Displays all timesheet entries between two timestamps.
```

### Linux command line cheatsheet
```
| #Pipes the output from the command preceding this into the input for the command following it.
> #Pipes the output from the command preceding this into some connection, like a file.
grep #Can be used to search through text.
less #Like 'more' on Windows, it shows a screen of things that you can look through.
```

### Examples:
```
t list | grep oper #Will search all of your teamsheets for those containing the text 'oper' and print them out.

t --help | less #Makes it easier to read through Timetrap's help documentation.

t week all -f csv > thisWeek.csv #Creates a CSV containing all of your 't in' and 't out' information for all timesheets for the week.
```
