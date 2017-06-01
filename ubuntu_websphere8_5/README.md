ubuntu_websphere8.5
==================

Needed vagrant plugins:

- vagrant-vbguest

Install: vagrant plugin install <plugin-name>

Ubuntu/WebSphere 8.5 Virtual Machine creation using vagrant and ansible 

You need to download IBM Installation Manager 1.8.5 and copy to was8_5_install

The scripts expect the file to be:

Download below file from 

https://www-945.ibm.com/support/fixcentral/swg/downloadFixes?parent=ibm%7ERational&product=ibm/Rational/IBM+Installation+Manager&release=1.8.3.0&platform=Linux&function=fixId&fixids=1.8.5.0-IBMIM-LINUX-X86_64-20160506_1125&includeRequisites=1&includeSupersedes=0&downloadMethod=http&login=true

agent.installer.linux.gtk.x86_64_1.8.5000.20160506_1125

If newer versions are available, the scripts and maybe also record files need to be adapted

The download requires an IBM ID, also for the installer this ID is needed. The install scripts source the user from the file was8_5_install/ibm_user.sh

The file needs to look like this and must be created before running vagrant up:

```
IBM_USER=<your ibm user id>
export IBM_USER
IBM_PWD=<your ibm user id password as cleartext>
export IBM_PWD
```

After installation the VM must be restarted (vagrant reload), then Websphere can be started:

- login as user vagrant (default pw: vagrant), e.g. using vagrant ssh
- run: 
```
vagrant@ubuntu-was8:~$ IBM/WebSphere/AppServer/profiles/rplan-profile/bin/startServer.sh server1
```
- access admin console: https://<server_ip>:9043/ibm/console (authentication is disabled, use any username (e.g. admin)

TODO: 
/etc/security/limits.d/90-nofile.conf with
vagrant soft nofile 40000
vagrant hard nofile 40000


