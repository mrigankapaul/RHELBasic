## Shell Basic
Kernel - Direct Access to HW, Memory management, Scheduling, etc...<br>
Shell - Interface to kernel level services.<br>
### Unix Commands
```
uname -a
top
whoami
echo $BASH_VERSION
ps // to determine the current shell.
ps -p $$ // to determine the current shell.
echo $0 // to determin the current shell.
ping 8.8.8.8 > /tmp/pingtemp & // run the command in the background.
kill <pid> // to kill a process
yum install zsh -y // to install zsh on centos
zsh // switching to zsh
ps -ef | egrep 'bash|zsh' // to display the process along with parent process.
pstree // parent to child relationship
cat /etc/passwd | grep root // to determine the login shell.
cat /etc/passwd
adm:x:3:4:adm:/var/adm:/sbin/nologin // /sbin/nologin prevents a user to get a login shell.
echo $SHELL // will hold the path of the login shell.
You can have upto 7 sesssion with ALT+<F1>...<F7> // use ttty to determine the users logged in the current session.
stdin - fd0, stdout - fd1, stderr - fd2
set -o noclobber // to avoid clobbering of an existing file.
echo 'Warning! This is second' >| /tmp/lsout // to forcefully clobber an existing file.
set +o noclobber // allows clobbering of file (default)
echo "This is an end text" >> /tmp/lsout // append to end of file
ls mriganka /root 2>/tmp/err // redirecting output to standard error.
env
echo $defcon // expansion of variable.
echo \$defcon
Child process inherits a copy of env. variable.
export PS1='' // to customize the prompt.

more /etc/profile
ls -l /etc/profile.d/
more ~/.bash_profile

bash -l // open a bash shell as a login shell
type vi // external binaries
type cd // in built
cat ~/.bash_history // stored the history
env | grep HIST
HISTSIZE, HISTFILESIZE, HISTFILE, HISTCONTROL=ignoredups
alias ls='ls --color=auto'
unalias ls
ping 8.8.8.8 > /tmp/pingfile &
sleep 300 & sleep 600 &
fg
fg [jobnumber]
ctrl + z and then bg
```

### Using YUM you can install the tools with the following RPM packages:
```
yum install proctools # same as brew package
yum install psmisc # for pstree
yum install vnstat # for vnstat
yum install ncdu # for ncdu
yum install initscripts # for ipcalc - should already have this, I think?
yum install htop # for htop
yum install ack # for ack
yum install lsof # for lsof
```