# Scripting 
```
for m in hi salut ciao; do echo $m;done
i=3; while [ $i -gt 0 ]; do echo $i; let i-=1; done
ls /etc/hosts; echo $? //  $? hass the succcess or failure code of the last executed code
ls /etc/hosts && echo success
ls /etc/host || echo failure
cd mkt 2>/dev/null || { mkdir mkt && cd mkt; }
sudo yum install -y vim-enhanced 
echo 'abbr _sh #!/bin/bash' >> ~/.vimrc // now _sh will be replaced with shebang 
sed -i '1 i #!/bin/bash' my.sh
bash my.sh
mkdir ~/bin, mv test.sh ~/bin, chmod a+x ~/bin/test.sh, test.sh
$$ - The current PID
$? - Exit status of last command
!$ - last argument
$0 - Program Name
$1 ... - Argumemts
$#/$*/$@ - argument count, all arguments as string, all argumments as an array
sudo userdel -r sriparna // for a deleting an user
su -u99 // switching users
```
```
#!/bin/bash
echo "The program $0 have been executed with $# argumemts"
echo "The program $(basename $0) have been executed with $# arguments"
PID=${*:-"1"}
ps -fp $PID
```

```
#!/bin/bash

if [ "$#" -lt 1 ] ; then
       echo " You must provide the username: $0 <username>"
       exit 1

elif getent passwd "$1" ; then
        echo "The user $1 already exists on this system"
        exit 2
fi

while ! [ -n "$USER_PASSWORD" ]
do
        read -s -p "Enter a password for the new user $1: " USER_PASSWORD
done

sudo useradd -m "$1"
echo "$USER_PASSWORD" | sudo passwd --stdin "$1"
getent passwd "$1"
```


```
#!/bin/bash
create_user() {

        if [ "$#" -lt 1 ] ; then
                echo " You must provide the username: $0 <username>"
                exit 1
        elif getent passwd "$1" ; then
                echo "The user $1 already exists on this system"
                exit 2
        fi
        sudo useradd -m "$1"
        getent passwd "$1"
}

set_password() {

  while ! [ -n "$USER_PASSWORD" ]
  do
        read -s -p "Enter a password for the new user $1: " USER_PASSWORD
  done
  echo "$USER_PASSWORD" | sudo passwd --stdin "$1"
}

for u in "$@"
do
        create_user "$u"
        set_password "$u"
done
```