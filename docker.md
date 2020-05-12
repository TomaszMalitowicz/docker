
# How to docker on ubuntu.

`sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common`

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo apt-key fingerprint 0EBFCD88

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

   sudo apt update
   sudo apt upgrade
   sudo apt-get install docker-ce docker-ce-cli containerd.io
   sudo docker info
   sudo docker run hello-world


   docker run ubuntu echo test
# ti - terminal interactive
   docker run -ti ubuntu:14.10 echo "I'm Ubuntu"
   docker run -ti centos echo "I'm Centos"
   docker run -ti ubuntu:18.04 echo "I'm Ubuntu 18.04"
   docker run -ti ubuntu:20.04 echo "I'm Ubuntu 20.04"
   docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              20.04               1d622ef86b13        2 weeks ago         73.9MB
ubuntu              latest              1d622ef86b13        2 weeks ago         73.9MB
ubuntu              18.04               c3c304cb4f22        2 weeks ago         64.2MB
centos              latest              470671670cac        3 months ago        237MB
hello-world         latest              bf756fb1ae65        4 months ago        13.3kB
ubuntu              14.10               a8a2ba3ce1a3        4 years ago         194MB


# docker ps pokzuje tylko aktywne kontenery
 docker ps 
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS 
              NAMES
# docker ps -a pokazuje wszystkie zbudowane kontenery lacznie z nie aktywnymi

docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                         PORTS               NAMES
c8031511709f        ubuntu:20.04        "echo 'I'm Ubuntu 20…"   2 minutes ago       Exited (0) 2 minutes ago                           lucid_snyder
fed605d46f1c        ubuntu:18.04        "echo 'I'm Ubuntu 18…"   4 minutes ago       Exited (0) 4 minutes ago                           strange_curran
86fb84a50b46        centos              "echo 'I'm Centos'"      11 minutes ago      Exited (0) 11 minutes ago                          wizardly_hodgkin
88c28ec72869        ubuntu:14.10        "echo 'I'm Ubuntu'"      12 minutes ago      Exited (0) 12 minutes ago                          awesome_heisenberg
585dbe5ca2a3        ubuntu              "echo test"              14 minutes ago      Exited (0) 14 minutes ago                          adoring_matsumoto
3cfee8c93b8f        hello-world         "/hello"                 About an hour ago   Exited (0) About an hour ago                       reverent_wing



docker run -ti ubuntu bash

docker run -ti --name Kontener_01 ubuntu echo "Kontener01"
docker run -ti --name Kontener_02 ubuntu echo "Kontener02"


docker start c4fb5b2ef5f9

docker exec -ti c4fb5b2ef5f9 bash
# wyjscie z takiego kontenera nic nie zmienia by zakoczyc prace konternera trzeba wejsc do niego przy pomoc komendy attache - podlaczamy sie do aktywnego procesu w tym przykladzie bash.
docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
c4fb5b2ef5f9        ubuntu              "bash"              19 minutes ago      Up 3 minutes                            recursing_chatelet

docker attach c4fb5b2ef5f9
root@c4fb5b2ef5f9:/# exit
exit
# po wyjsciu kontener konczy swoja prace.
docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES


docker run -ti --name ubuntu_test01 ubuntu:18.04 bash
root@f9dc4b6807e0:/# touch plik.txt
root@f9dc4b6807e0:/# echo "tomasz" >> plik.txt 
root@f9dc4b6807e0:/# exit
exit


docker start ubuntu_test01
docker exec -ti ubuntu_test01 bash
# stworzony plik dalej istnieje
root@f9dc4b6807e0:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  plik.txt  proc  root  run  sbin  srv  sys  tmp  usr  var
# zamkniecie polaczenie exitem nie zamyka kontener bo bylem polaczony poprzez exec to zupelne nowy proces
docker stop ubuntu_test01
# zeby polaczyc sie do glownego procesu nalezy przy starcie uzyc flagi -ia = interactive attach
docker start -ia ubuntu_test01