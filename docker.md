
# How to docker on ubuntu.

`sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common`

`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`

`sudo apt-key fingerprint 0EBFCD88`

`sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"`

   `sudo apt update`

   `sudo apt upgrade`

   `sudo apt-get install docker-ce docker-ce-cli containerd.io`

   `sudo docker info`

   `sudo docker run hello-world`


   `docker run ubuntu echo test`

#### ti - terminal interactive

   `docker run -ti ubuntu:14.10 echo "I'm Ubuntu"`

   `docker run -ti centos echo "I'm Centos"`

   `docker run -ti ubuntu:18.04 echo "I'm Ubuntu 18.04"`

   `docker run -ti ubuntu:20.04 echo "I'm Ubuntu 20.04"`

   `docker images`

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              20.04               1d622ef86b13        2 weeks ago         73.9MB
ubuntu              latest              1d622ef86b13        2 weeks ago         73.9MB
ubuntu              18.04               c3c304cb4f22        2 weeks ago         64.2MB
centos              latest              470671670cac        3 months ago        237MB
hello-world         latest              bf756fb1ae65        4 months ago        13.3kB
ubuntu              14.10               a8a2ba3ce1a3        4 years ago         194MB


#### docker ps pokzuje tylko aktywne kontenery

 `docker ps`

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS 
              NAMES
#### docker ps -a pokazuje wszystkie zbudowane kontenery lacznie z nie aktywnymi

`docker ps -a`

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                         PORTS               NAMES
c8031511709f        ubuntu:20.04        "echo 'I'm Ubuntu 20…"   2 minutes ago       Exited (0) 2 minutes ago                           lucid_snyder
fed605d46f1c        ubuntu:18.04        "echo 'I'm Ubuntu 18…"   4 minutes ago       Exited (0) 4 minutes ago                           strange_curran
86fb84a50b46        centos              "echo 'I'm Centos'"      11 minutes ago      Exited (0) 11 minutes ago                          wizardly_hodgkin
88c28ec72869        ubuntu:14.10        "echo 'I'm Ubuntu'"      12 minutes ago      Exited (0) 12 minutes ago                          awesome_heisenberg
585dbe5ca2a3        ubuntu              "echo test"              14 minutes ago      Exited (0) 14 minutes ago                          adoring_matsumoto
3cfee8c93b8f        hello-world         "/hello"                 About an hour ago   Exited (0) About an hour ago                       reverent_wing



`docker run -ti ubuntu bash`

`docker run -ti --name Kontener_01 ubuntu echo "Kontener01"`

`docker run -ti --name Kontener_02 ubuntu echo "Kontener02"`

`docker start c4fb5b2ef5f9`

`docker exec -ti c4fb5b2ef5f9 bash`

#### wyjscie z takiego kontenera nic nie zmienia by zakoczyc prace konternera trzeba wejsc do niego przy pomoc komendy attache - podlaczamy sie do aktywnego procesu w tym przykladzie bash.

`docker container ls`

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
c4fb5b2ef5f9        ubuntu              "bash"              19 minutes ago      Up 3 minutes                            recursing_chatelet

`docker attach c4fb5b2ef5f9`

root@c4fb5b2ef5f9:/# exit
exit

#### po wyjsciu kontener konczy swoja prace.

`docker container ls`

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES


`docker run -ti --name ubuntu_test01 ubuntu:18.04 bash`

root@f9dc4b6807e0:/# touch plik.txt
root@f9dc4b6807e0:/# echo "tomasz" >> plik.txt 
root@f9dc4b6807e0:/# exit
exit


`docker start ubuntu_test01`

`docker exec -ti ubuntu_test01 bash`

#### stworzony plik dalej istnieje

root@f9dc4b6807e0:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  plik.txt  proc  root  run  sbin  srv  sys  tmp  usr  var

#### zamkniecie polaczenie exitem nie zamyka kontener bo bylem polaczony poprzez exec to zupelne nowy proces

`docker stop ubuntu_test01`

#### zeby polaczyc sie do glownego procesu nalezy przy starcie uzyc flagi -ia = interactive attach

`docker start -ia ubuntu_test01`

flaga restart

`docker run -ti --restart unless-stopped --name ciagle_na_chodzie ubuntu bash`

`docker container ls`

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
101b1f9569f4        ubuntu              "bash"              11 minutes ago      Up 3 seconds                            ciagle_na_chodzie

`docker stop ciagle_na_chodzie`

`docker container ls`


#### usuwanie kontenerow

jezeli kontener jest uruchomiony zatrzymujemy go poleceniem

`docker stop nazwa/id_kontenera`

usuwanie pojedynczego kontenera:

`docker rm nazwa/id_kontenera`

a co jak jest ich hm ? 50 ?

wyswietlanie wszytkich kontenerow up i exited

`docker container ls -a`

`docker container ls -aq` - wyswietli nama same id kontenerow

usuwanie z zagniezdzeniem polecenia

`docker container rm $(docker container ls -aq)`

#### tworzenie obrazu z kontenera

`docker run -ti ubuntu bash`

root@2ab9b4e4a17c:/# touch image_info.txt
root@2ab9b4e4a17c:/# echo "Customizowany obraz ubuntu od TM" >> image_info.txt 
root@2ab9b4e4a17c:/# cat image_info.txt 
Customizowany obraz ubuntu od TM


docker ps -l zwraca info odnoscie ostatnio zamknietego kontenera

`docker ps -l`

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
2ab9b4e4a17c        ubuntu              "bash"              2 minutes ago       Exited (0) 12 seconds ago                       romantic_archimedes

Tworzenie imiga z contenera:

`docker commit 2ab9b4e4a17c`

sha256:13d1c1c2ee21896505cce4298bc956d055be3aff1f98658e546aa36e35ab6d44

tagujemy i nadajemy nazwe:

`docker tag 13d1c1c2ee21896505cce4298bc956d055be3aff1f98658e546aa36e35ab6d44 custom_ubuntu_tm`

sprawdzamy obrazy

`docker images`

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<ins>custom_ubuntu_tm    latest              13d1c1c2ee21        36 seconds ago      73.9MB</ins>
ubuntu              20.04               1d622ef86b13        2 weeks ago         73.9MB
ubuntu              latest              1d622ef86b13        2 weeks ago         73.9MB
ubuntu              18.04               c3c304cb4f22        2 weeks ago         64.2MB
centos              latest              470671670cac        3 months ago        237MB
hello-world         latest              bf756fb1ae65        4 months ago        13.3kB
ubuntu              14.10               a8a2ba3ce1a3        4 years ago         194MB


`docker run -ti custom_ubuntu_tm bash`

docker container ls -a
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS                      PORTS               NAMES
e79d3758717e        custom_ubuntu_tm    "bash"              About a minute ago   Exited (0) 9 seconds ago                        sleepy_tu



obraz mozna zobic szybciej w jendym poleceniu.
`docker commit e79d3758717e custom_ubuntu_tm_2`


#### Docker search

`docker search ubuntu`

NAME                                                      DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
ubuntu                                                    Ubuntu is a Debian-based Linux operating sys…   10877               [OK]                
dorowu/ubuntu-desktop-lxde-vnc                            Docker image to provide HTML5 VNC interface …   422                                     [OK]


`docker search http`
NAME                                 DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
httpd                                The Apache HTTP Server Project                  2999                [OK]                
haproxy                              HAProxy - The Reliable, High Performance TCP…   1417                [OK]                
steveltn/https-portal                A fully automated HTTPS server powered by Ng…   91                                      [OK]


#### Docker pull

`docker pull fedora:latest`

latest: Pulling from library/fedora
fe04b97b6519: Pull complete 
Digest: sha256:f2d15cd8f8424575f30fcf2df31384a1a8f6ace986764d98562779559546664d
Status: Downloaded newer image for fedora:latest
docker.io/library/fedora:latest

`docker pull httpd`

Using default tag: latest
latest: Pulling from library/httpd
54fec2fa59d0: Pull complete 
8219e18ac429: Pull complete 
3ae1b816f5e1: Pull complete 
a5aa59ad8b5e: Pull complete 
4f6febfae8db: Pull complete 
Digest: sha256:c9e4386ebcdf0583204e7a54d7a827577b5ff98b932c498e9ee603f7050db1c1
Status: Downloaded newer image for httpd:latest
docker.io/library/httpd:latest

#### Docker inspect
docker inspect nazwa_obrazu - zwraca informacje na temat obrazu

`docker inspect httpd`


#### Docker tag
docker tag pozwala zmienic nazwe obrazu.
docker tag nazwa_obrazu nowa_nazwa_obrazu:nr_versji

docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
custom_ubuntu_tm    latest              13d1c1c2ee21        31 minutes ago      73.9MB
ubuntu              20.04               1d622ef86b13        2 weeks ago         73.9MB
httpd               latest              b2c2ab6dcf2e        2 weeks ago         166MB

`docker tag custom_ubuntu_tm custom_ubuntu:20.04`


#### docker push



#### docker rmi

`docker images`

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
custom_ubuntu       20.04               13d1c1c2ee21        33 minutes ago      73.9MB
custom_ubuntu_tm    latest              13d1c1c2ee21        33 minutes ago      73.9MB
ubuntu              20.04               1d622ef86b13        2 weeks ago         73.9MB
httpd               latest              b2c2ab6dcf2e        2 weeks ago         166MB


`docker rmi -f $(docker images -q)` -f opcja force gdy imge id jest taki sam w kilku obrazach

Untagged: custom_ubuntu:20.04
Untagged: custom_ubuntu_tm:latest
Deleted: sha256:13d1c1c2ee21896505cce4298bc956d055be3aff1f98658e546aa36e35ab6d44
Untagged: ubuntu:20.04
...

`docker images`

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE

i mamy puste repo obrazow
