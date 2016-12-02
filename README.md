j_timer
=======================

This is just a Vagrantfile that will create an Ubuntu VirtualBox VM to run Timetrap
(https://github.com/samg/timetrap).

To use this VM you need to have installed:
* Virtualbox - https://www.virtualbox.org
* Vagrant - https://www.vagrantup.com

===One way to get this working
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

===Another way to get this working
If you also have `git` and `git-bash` installed, then just open `git-bash`, navigate to
where you want your parent directory to be and run
```
git clone https://github.com/jjcad/j_timer
```
Then navigate into the `j_timer` directory and run `vagrant up`, wait for it download,
boot, and install your software on the first run, then `vagrant ssh` into the box.

===If you don't want to use ssh at all
You can have `vagrant up` also launch a Virtualbox GUI window that will give you a
command prompt if you so choose. If this is your desired way to interact with the VM then
simply open the Vagrantfile in a text editor, and switch `vb.gui = false` to `vb.gui = true`
and you should be all set. If you make this change after you have already run 
`vagrant up` and the VM is running right now, then you'll need to bring the VM down
using `vagrant halt` before launching it again with `vagrant up` in order to get that
window.
