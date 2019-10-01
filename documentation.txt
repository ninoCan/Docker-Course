We are now going learn to create a docker container and see how this is much
more convenient than the virtual machine in term of time spent setting things
up.

Installation:
For Ubuntu distros the official repos for docker use a older version. Hence,
to install docker we need to go through a bit winded process.
Update the database
   $ sudo apt-get update
Install the relevant dependencies
   $ sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent     software-properties-common
Update the pgp key
   $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
     sudo apt-key add -
Add the new repos to the list
   $ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
                     $(lsb_release -cs)  stable"
Finally, install docker.


RUN A DOCKER
On my locale machine, I previously installed docker but the service was
inactive, this was easily solved by running:
   $ sudo systemctl start docker.service
Now docker is ready to be used.
To create a new container on docker we can run, for example:
   $ sudo docker run --name nginx-container -p 80:80 -d nginx
This is basically it. With a oneliner we performed all the work that took us
hours with the virtual machine. Moreover, we do not even need to have nginx
installed on the local machine, docker takes care of it on its own!
Let us analyze in detail what we just did:
The "--name" flag gives a name to container we are going to create.
The "-p 80:80" forwards the local port 80 to the container port 80
The "-d" flag makes it running in the background and "nginx" just specify the
image running on the container(it is basically the app running on it).
To see if everything is working as it should let us first examine the ip
tables by the command:
   $ ip addr
   Example output on screen:
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group
    default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host valid_lft forever preferred_lft forever
    2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel
    state UP group default qlen 1000 link/ether 08:00:27:59:d3:71
    brd ff:ff:ff:ff:ff:ff inet 10.0.2.15/24 brd 10.0.2.255
    scope global dynamic enp0s3 valid_lft 633sec preferred_lft 633sec
    inet6 fe80::a00:27ff:fe59:d371/64 scope link 
    valid_lft forever preferred_lft forever
    3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue
    state UP group default link/ether 02:42:72:fc:50:2a brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
    valid_lft forever preferred_lft forever inet6 fe80::42:72ff:fefc:502a/64
    scope link valid_lft forever preferred_lft forever
    7: vethc5a0d73@if6: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc
    noqueue master docker0 state UP group default 
    link/ether fe:43:e6:ba:0c:eb brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet6 fe80::fc43:e6ff:feba:ceb/64 scope link valid_lft forever
    preferred_lft forever
    9: vethbb89dfd@if8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc
    noqueue master docker0 state UP group default 
    link/ether 2e:25:0e:56:22:ef brd ff:ff:ff:ff:ff:ff link-netnsid 1
    inet6 fe80::2c25:eff:fe56:22ef/64 scope link valid_lft forever
    preferred_lft forever

We can notice that this as already created an additional (3: docker0) port to
the web with ip 172.17.0.1. This address can be contacted from the browser,
and gives back the nginx front page. The sheer beaut of simplicity! 

The major upperhand is that we can run multiple instances on the fly, eg:
    $ docker run --name nginx-8080 -p 8080:80 -d nginx
    $ sudo docker run --name nginx-localhost -p 127.0.0.1:8888:80 -d nginx

More about "docker run" flags and examples at:
https://docs.docker.com/engine/reference/commandline/run/
LIST, STOP, RESTART & REMOVE CONTAINERS
To list the running container simply run:
   $ sudo docker ps
This command will not count inactive containers, but this can be remedied by:
   $ sudo docker ps -a
Up to now, we have only started the container. To stop a running container run
   $ sudo docker stop <name1> <name2> ...
Once stopped, and only when inactive, the created containers can be removed:
   $ sudo docker rm <name-inactive-container1> ...

More about docker command flag and examples at:
https://docs.docker.com/engine/reference/commandline/ps/
https://docs.docker.com/engine/reference/commandline/stop/
https://docs.docker.com/engine/reference/commandline/start/
https://docs.docker.com/engine/reference/commandline/rm/
CONTAINER ENVIRONMENT VARIABLES

The container we created for the nginx application simply called the app
after creating the container. But sometimes we would like to (or we ought to)
call apps that need more than a simple call to be activated. The docker
command take this into account seamlessly.
