# Essentials
```
ss -ntl
ls /usr/share/doc
mkdir -p vagrant/rhel8; cd vagrant/rhel8
vagrant init generic/rhel8
vagrant up
vagrant ssh
vagrant up 
```

```
touch file1
file file1
ls -l /etc/hosts1 /etc/hosts 2>output // redirecting stderr
ls -l /etc/hosts1 /etc/hosts &>output // redirecting stdout and stderr to a file
cat > story.txt <<END // HEREDOCS
echo hello | tee  [-a] file1 // to send the outout to console and to a file
echo "1.0.0.1 cf" | sudo tee -a  /etc/hosts
tail -n1 /etc/hosts
sudo yum repolist // to list the RHEL repo
sudo yum install -y vim nano bash-completion // to install vim and nano
vimtutor
cd - // takes you back to the previous directories
mkdir -p dir1/dir2 /// create the parent directory along with the child directory
rm -rf dir1
touch files{1..12}
ls files*
ls files?
cp /etc/hosts .
mv hosts myhosts
head -n2 /etc/passwd
tail -n2 /etc/passwd
wc -l /etc/services
less /etc/services
sudo grep Password /etc/ssh/sshd_config // searching a text in a file
sudo grep ^Password /etc/ssh/sshd_config // search for lines that begins with 'Password'
sudo grep -vE '^(#|$)' /etc/ssh/sshd_config | wc -l
grep ^root /etc/passwd
grep bash$  /etc/passwd // search of line having 'bash' at the end
<file type> <permissions> <link count><user group><size><modified time>
stat -c %a /etc/hosts
file types - regular, directory, link, pipe, block/chracter, socket
ls -ld dir1 // long listing of directory without listing the content
ls -l $(tty)
Read 100, Write 010, Execute 001
default permission of file 666 and for directory 777
umask,  umask 0, umask 002(default) // controlling the permission of files in the current shell
chmod -v 666 f3 // rw-rw-rw-
chmod -v a+w f3
umask 007
mkdir -p u/{d1,d2}
touch u/{d1,d2}/file
chmod -vR a+X u // change the ownership of all to x when it is already set to x for user and owner
chmod -vR +w u
chmod -vR a+w u
sudo chown root f1 // changing the ownership of the file
sudo chown root:root f1 // changing the owner and group of a file
sudo chgrp root f1 // changing the group ownership of a file
mkdir dir/subdir; ls -ld dir // hard link changes from 2 to 3
ls -ldi dir dir/. dir/subdir/..
ln -s /etc/services ports // creating a symbolic link
id
sudo usermod -aG wheel vagrant // adding the user to an additional group. You need to logout and login to see that the user has been added to additional group.
id vagrant
newgrp wheel // switching groups
sudo passwd root // set the passwor of root user
su
su - // with login shell
chmod -v u=w log // creating a write only file
mkdir -m 155 dir // creating a directory with only execute permission
chmod -v u=wx dir
sudo du -sh /etc // size of a file
sudo tar -cf etc.tar /etc
tar -tf etc.tar // content of a tar file
tar -xvf etc.tar
sudo yum install -y star
sudo star -c -f myetc.tar /etc
sudo star -t -f myetc.tar
star -x -f myetc.tar
tar -czf (gzip)
tar -cjf (bzip2)
gzip etc.tar // compression
gunzip etc.tar.gz // expansion
time bzip2 etc.tar // compression
bunzip2 etc.tar.bz2 // expansion
sudo tar -czf etc.tar.gz /etc // compession using tar command
tar -xf etc.tar.gz // expansion

```