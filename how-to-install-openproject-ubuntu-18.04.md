# How to install OpenProject on Ubuntu 18.04

## Installation instructions


```sh
wget -qO- https://dl.packager.io/srv/opf/openproject-ce/key | sudo apt-key add -

# For OpenProject Stable/7
sudo wget -O /etc/apt/sources.list.d/openproject-ce.list \
  https://dl.packager.io/srv/opf/openproject-ce/stable/7/installer/ubuntu/18.04.repo

# For OpenProject Stable/8
sudo wget -O /etc/apt/sources.list.d/openproject-ce.list \
  https://dl.packager.io/srv/opf/openproject-ce/stable/8/installer/ubuntu/18.04.repo

sudo apt-get update
sudo apt-get install openproject
```

Note: You may need to `sudo apt install wget apt-transport-https` for the above instructions to work.

## Configurations
Perform command bellow and follow its instructions in order to be able to use OpenProject:

```sh
sudo openproject configure
```

Note: in case of you want to reconfigure OpenProject, run `sudo openproject reconfigure` instead.


## Email notifications [Additional]


Install `ssmtp` package if your system doesn't have it:
```sh
sudo apt install ssmtp
```

Then edit the configuration file:
```sh
sudo vim /etc/ssmtp/ssmtp.conf
```

Adjust and add as necessary the  following parameters:

```sh
root=username@gmail.com

# Change it from postmaster to the machines admin’s Email.

mailhub=smtp.gmail.com:587

# Your mail server in our case this is Gmail so we have to specify the port as 587, for regular SMTP servers this is usually not necessary.

hostname=username@gmail.com

# Usually the name of the machine is automatically filled by the package setup, if the machine has a mailbox this should be fine, but if it doesn’t or the name is not the same as the mailbox adjust accordingly.

UseSTARTTLS=YES

# Enable TLS for secure session communication.

AuthUser=username

# The username of the sending mailbox.

AuthPass=password

# The password of the sending mailbox..

FromLineOverride=yes

# Sends the hostname instead of root[root@hostname.FQDN].
```

### Confirming setup
```sh
echo "Test message from Linux server using ssmtp" | sudo ssmtp -vvv your-email@some-domain.com

# or do the following command if you get content from text file
ssmtp -vvv your-email@some-domain.com < ssmtp_test_message.txt
```

_Congratulations! You did it. ;)_
