# Learn AppArmor

## Install AppArmor

Is AppArmor installed?
```
apparmor_status

apt-get install apparmor
apt-get install apparmor-utils
```

## Run AppArmor

```
systemctl start apparmor
systemctl enable apparmor
```


## List all profiles

```
aa-status
```


## First try

```
# works as expected
ping google.com

aa-autodep ping

# The profile is created but not yet created
aa-status
aa-status | grep ping

# Activate ping profile
aa-enforce ping

# Next try - dont work anymore (also root can not use ping anymore)
ping google.com

```

## Delete the rule

```
cd /etc/apparmor.d/
rm bin.ping
reboot now

# better solution
ln -s /etc/apparmor.d/root.apparmor.myscript.sh /etc/apparmor.d/disable
apparmor_parser -R /etc/apparmor.d/root.apparmor.myscript.sh
```



## Second rule

Create a script - hello
```
#!/bin/bash
echo "Hello, World!" > /tmp/hello.txt
cat /tmp/hello.txt
rm /tmp/hello.txt
```


```
# Create a new profile for the hello script
aa-genprof ./hello

# Run the hello script in an new terminal
./hello

# Return to the genprof terminal
S
E
E

aa-status
```


## Enable Profile (set in enforced mode.)

```
apparmor_parser -q /etc/apparmor.d/usr.sbin.nginx
```


