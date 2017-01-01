# Internet of Insecurity Lab

## How to clone this branch
* `git clone https://github.com/jonzeolla/lab-InternetofInsecurity`
  * Clone the latest revision of the lab-InternetofInsecurity repo.

## Contributing
1. [Fork the repository](https://github.com/jonzeolla/lab-InternetofInsecurity/fork)
1. Create a feature branch via `git checkout -b feature/description`
1. Make your changes
1. Commit your changes via `git commit -am 'Summarize the changes here'`
1. Create a new pull request ([how-to](https://help.github.com/articles/creating-a-pull-request/))

## Related Events
### Nothing yet

## How to setup the lab
### Configure the lab manually
This assumes that you have git installed on either your Linux or Windows box (instructions [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)) and that you have cloned it to your Deskto
p.

#### Linux
From the top level of the source code repository run:
`./setup.sh`

### Use the provided Virtual Machine
A VM will be provided for the lab (TODO).  Only **VMWare hypervisors** have been tested with the following configuration.
##### Login Credentials
* Username:  iot
* Password:  P@ssword

### Configure a new VM for distribution
1. Install Kali in a VM[1]
2. Login as root, then open a terminal
3. (optional) Take a snapshot
4. Run the following commands:

    ```
    apt-get -y update && apt-get -y upgrade
    apt-get -y install open-vm-tools-desktop fuse
    history -c && gnome-session-quit
    ```
5. VM Tools should now be active, allowing you to easily copy and paste into the VM.  Login as root again, then open a terminal and run the following commands:

    ```
    echo -e "# Add additional sbin and bin directories to \$PATH\nexport PATH=\$PATH:\${HOME}/bin:/sbin:/usr/sbin:/usr/local/sbin\n\n# Include .bashrc if it exists\nif [ -f "\${HOME}/.bashrc" ]; then\n  . "\${HOME}/.bashrc"\nfi\n" > /etc/skel/.bash_profile
    echo -e "# Kali rolling repos\ndeb http://http.kali.org/kali kali-rolling main contrib non-free\n#deb-src http://http.kali.org/kali kali-rolling main contrib non-free" > /etc/apt/sources.list
    echo -e "\n\n# Add additional sbin and bin directories to \$PATH\nexport PATH=\$PATH:\${HOME}/bin:/sbin:/usr/sbin:/usr/local/sbin\n" >> /etc/skel/.profile
    echo "BkvrbAbvPsBhRQmF2ZKu3aeME3mZ1FtoYKbz8oCnNRF7mmZKXgK4iRdWcJarUZaLjTuFvk1IXiF57yEuIhjBzNv5RpIAA4JBK3Pk" > /etc/scis.conf
    useradd -m -p $(openssl passwd -1 P@ssword) -s /bin/bash -c "SCIS Internet of Insecurity User" -G sudo iot
    history -c && gnome-session-quit
    ```
6. Login as iot and rearrange the Favorites shortcuts as appropriate
7. Open a terminal and run the following commands:

    ```
    sudo systemctl disable rsyslog;sudo systemctl stop rsyslog
    sudo logrotate -f /etc/logrotate.conf
    sudo rm -rf /var/log/*1 /var/log/*old /var/log/*gz
    cd ${HOME}/Desktop
    git clone --recursive https://github.com/jonzeolla/lab-InternetofInsecurity
    cd lab
    ./setup.sh
    exitcode=$?
    sudo systemctl enable rsyslog
    if [[ ${exitcode} == 0 ]]; then rm ~/.bash_history; history -c && shutdown -P now; else echo "There was an issue while installing"; fi
    ```
8. Create the OVA. On a Mac using VMware Fusion, this looks something like:

    ```
    cd /Applications/VMware\ Fusion.app/Contents/Library/VMware\ OVF\ Tool/
    ./ovftool --acceptAllEulas /path/to/VM.vmx /path/to/VM.ova
    ```

## Updating this branch  
### Linux
If you'd like to update this branch, open a terminal and `cd` into the repo (if you are following the lab, this is `${HOME}/Desktop/lab-InternetofInsecurity/`) and then run:  

```
git pull
./setup.sh
```
 * It is possible that you will need to first run `git reset --mixed`, depending on if the `git merge` can be successful without manual intervention.  Note that running this command will reset your index, but not the working tree.  If you don't know what that means, and would like to, read [this](https://git-scm.com/docs/git-reset).  

## Some other good materials
### General Hardware / Software
* https://github.com/pwnieexpress/blue_hydra
* https://greatscottgadgets.com/ubertoothone/

### Android
* https://developers.google.com/nearby/
* https://developers.google.com/beacons/

### Apple iOS
* https://developer.apple.com/ibeacon/

### Misc
* https://www.youtube.com/watch?v=aVun9cLZRUA
* https://www.youtube.com/watch?v=0FfvLxW_cZg
* http://blog.beaconstac.com/2015/10/rfid-vs-ibeacon-ble-technology/
* https://kontakt.io/blog/beacon-security/


[1]:  I typically make sure to create VMs as harware version 10 under Compatibility because I've found it fixes some issues with transferring VMs between VMware Fusion and ESXi 5.5.

