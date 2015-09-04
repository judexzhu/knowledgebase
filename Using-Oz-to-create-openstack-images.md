Clone latest git repository containing OZ templates on VM test machine.

```$ git pull https://github.com/alces-software/oztemplates.git```

Switch to `ruan-openstack` branch.

```$ cd oztemplates && git checkout ruan-openstack```

To start building an image, enter the `bin` directory, run the `centos6-alces-openstack.sh` script, and name the image.

```$ ./centos6-alces-openstack.sh ruan-memtester```

The script will then start OZ downloading the image, if there are any problems, the logs are placed at /tmp/alces-imageinit.YYYYDDMMHHMM 
