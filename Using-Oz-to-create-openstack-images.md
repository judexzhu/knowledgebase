- Clone latest git repository containing OZ templates on VM test machine.

```$ git pull https://github.com/alces-software/oztemplates.git```

- Switch to `ruan-openstack` branch.

```$ cd oztemplates && git checkout ruan-openstack```

- To start building an image, enter the `bin` directory, run the `centos6-alces-openstack.sh` script, and name the image.

```$ ./centos6-alces-openstack.sh ruan-memtester```

The script will then start OZ downloading the image, if there are any problems, the logs are placed at `/tmp/alces-imageinit.YYYYDDMMHHMM`

- Once installed, you will be propmpted to connect to a screen session - you can log in as root and our usual mathematical password. The image is now built with a base installation of the tools required to install Alces Gridware packages via `alces packager`, any other dependencies via `yum`. You can connect to the screen session (which is a `virsh-console` session) as instructed to configure the IP address and connect via SSH to it's IP Address on the DMZ network. 

```$ screen -r alces-imageinit.30712

CentOS release 6.7 (Final)
Kernel 2.6.32-573.el6.x86_64 on an x86_64

localhost.localdomain login: root
Password:
[root@localhost ~]# ifconfig eth0 | grep 'inet addr'
          inet addr:10.101.0.99  Bcast:10.101.255.255  Mask:255.255.0.0```

Now you can SSH to the Image Built:

```$ ssh root@10.101.0.99 
Last login: Fri Sep  4 18:09:32 2015
[root@localhost ~]# alces packager install apps/memtester/4.3.0
Installing base/apps/memtester/4.3.0

 > Preparing package sources
        Download --> memtester-4.3.0.tar.gz ... OK
          Verify --> memtester-4.3.0.tar.gz ... OK

 > Preparing for installation
           Mkdir ... OK (/usr/src/alces/packager/src/apps/memtester/4.3.0/gcc-4.4.7)
         Extract ... OK

 > Proceeding with installation
         Compile ... OK
           Mkdir ... OK (/opt/gridware/pkg/el6/apps/memtester/4.3.0/gcc-4.4.7)
         Install ... OK
          Module ... OK

Installation complete.
[root@localhost ~]#
```

- Once you have completed the build and installation of any packages to be installed or configured within the image, you should then run the script at `oztemplates/bin/alces-imageinit-cloudify.sh` - this will then proceed to shutdown the image, install all the required `cloud-init` packages and prepare the image for deployment as an openstack instance. At the end the image will be sparsified and compressed to make moving to the remote server easier.

```oztemplates/bin/alces-imageinit-cloudify.sh ruan-memtester```

