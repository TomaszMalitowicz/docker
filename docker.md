# How to docker on ubuntu.
```
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`  
`sudo apt-key fingerprint 0EBFCD88`  
```
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
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
```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              20.04               1d622ef86b13        2 weeks ago         73.9MB
ubuntu              latest              1d622ef86b13        2 weeks ago         73.9MB
ubuntu              18.04               c3c304cb4f22        2 weeks ago         64.2MB
centos              latest              470671670cac        3 months ago        237MB
hello-world         latest              bf756fb1ae65        4 months ago        13.3kB
ubuntu              14.10               a8a2ba3ce1a3        4 years ago         194MB
```

#### docker ps pokzuje tylko aktywne kontenery

 `docker ps`  
```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS 
              NAMES
```
#### docker ps -a pokazuje wszystkie zbudowane kontenery lacznie z nie aktywnymi

`docker ps -a`  
```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                         PORTS               NAMES
c8031511709f        ubuntu:20.04        "echo 'I'm Ubuntu 20…"   2 minutes ago       Exited (0) 2 minutes ago                           lucid_snyder
fed605d46f1c        ubuntu:18.04        "echo 'I'm Ubuntu 18…"   4 minutes ago       Exited (0) 4 minutes ago                           strange_curran
86fb84a50b46        centos              "echo 'I'm Centos'"      11 minutes ago      Exited (0) 11 minutes ago                          wizardly_hodgkin
88c28ec72869        ubuntu:14.10        "echo 'I'm Ubuntu'"      12 minutes ago      Exited (0) 12 minutes ago                          awesome_heisenberg
585dbe5ca2a3        ubuntu              "echo test"              14 minutes ago      Exited (0) 14 minutes ago                          adoring_matsumoto
3cfee8c93b8f        hello-world         "/hello"                 About an hour ago   Exited (0) About an hour ago                       reverent_wing
```

`docker run -ti ubuntu bash`  
`docker run -ti --name Kontener_01 ubuntu echo "Kontener01"`  
`docker run -ti --name Kontener_02 ubuntu echo "Kontener02"`  
`docker start c4fb5b2ef5f9`  
`docker exec -ti c4fb5b2ef5f9 bash`  

#### wyjscie z takiego kontenera nic nie zmienia by zakoczyc prace konternera trzeba wejsc do niego przy pomoc komendy attache - podlaczamy sie do aktywnego procesu w tym przykladzie bash.

`docker container ls`  
```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
c4fb5b2ef5f9        ubuntu              "bash"              19 minutes ago      Up 3 minutes                            recursing_chatelet
```
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



#### Procesy w kontenerze

`docker run --rm centos echo "Jestem przez chwile"`  

po wykonaniu polecenie konterner zakonczyl swoja prace flaga --rm usuwa kontener z listy zbodwanych nieaktywnch kontenerow.


`docker run -ti --name kontener1 ubuntu /bin/bash`  


`docker run -d -ti --name donald_trump ubuntu /bin/bash`  

flaga -d - detach pozwala uruchomic kontener w tle.


jak juz wiesz aby sie podlaczyc do dzialajacego kontener i pierwszego procesu nalezy uzyc komendy docker attach  
`docker attach donald_trump`  

aby przejsc spowrotem do tla nalezy uzyc skrotu klawiszowego ctrl+p, ctrl+q



#### docker inspect

`docker inspect donald_trump`  
`docker start donald_trump`  
`docker inspect donald_trump`  

mozemy porownac informacje wylaczonego i wlaczonego kontenera.  

`docker pull nginx:latest`  

`docker run -ti -d --name WebServer1 nginx:latest /bin/bash`  

`docker inspect WebServer1`  


#### zarzadzanie kontenerami

`docker run -ti --name kontener2 ubuntu /bin/bash`  

aktualizujemy kontener insalujemy vima dodajemy folder plik itd. wychodzimy zapominamy co tam robilismy.  

jezeli kontener jest na liscie mozna spradzic co tam sie dzialo poprzez komende docker log nazwa/id kontenera.  

`docker logs kontener2`  



`docker run -d --name contener_loop ubuntu bash -c "while true; do echo Donald Trump for President of USA; sleep 1; done"`  

docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                       PORTS               NAMES
ffbdd3b2290a        ubuntu              "bash -c 'while true…"   52 seconds ago      Up 51 seconds                                    contener_loop

`docker logs ffbdd3b2290a`  
Donald Trump for President of USA  
Donald Trump for President of USA  
Donald Trump for President of USA  
Donald Trump for President of USA  

`docker logs ffbdd3b2290a | wc -l`  
104

zabijamy kontener z nieskonczona petla:  

`docker container kill ffbdd3b2290a`  


#### Siec
- Kontenery moga sie komunikowac miedzy soba.  
- Kontener moze sie komunikowac sam ze soba poprzez siec.
- Kontener moze sie komunikowac tez z hostem.
- Host moze komunikowac sie z kontenerem.

`docker run -ti --name WebServerNet ubuntu bash`  
`apt update` - bez update'u nie zaisnstalujemy zadnje paczki  
`apt install apache2`  
`apt install lsof`  
`/etc/init.d/apache2 restart` - restartujemy apacha
`lsof -i -P -n`  
COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
apache2 3670 root    3u  IPv4 109706      0t0  TCP *:80 (LISTEN)

`apt install elinks`  

elinks http://localhost

zostawiamy kontener w tle ctrl+p, ctrl+q

`sudo iptables -S`  
...
-A FORWARD -j DOCKER-USER
-A FORWARD -j DOCKER-ISOLATION-STAGE-1
-A FORWARD -o docker0 -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
-A FORWARD -o docker0 -j DOCKER
...

sprawdzamy interfejsy sieciowe:
`ifconfig -a`  
docker0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255


wracamy do kontenera:
`docker attach cabc8930823b`  
`apt install openssh-server -y`  
`/etc/init.d/ssh restart`  

`lsof -i -P -n`  
COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
sshd    1285 root    3u  IPv4 117179      0t0  TCP *:22 (LISTEN)
sshd    1285 root    4u  IPv6 117181      0t0  TCP *:22 (LISTEN)
apache2 1312 root    3u  IPv4 117234      0t0  TCP *:80 (LISTEN)


`adduser user`  
wrzucamy kontener w tlo ctrl+p crtl+q

laczymy sie z naszej maszyny poprzez ssh i utowrzonego user do kontenera.

ssh user@172.17.0.2


##### Porty   
`docker run -d -ti --name WebServer4 httpd`  

`docker ps`
CONTAINER ID        IMAGE               COMMAND              CREATED             STATUS              PORTS               NAMES
cb5607dce3d4        httpd               "httpd-foreground"   3 minutes ago       Up 3 minutes        80/tcp              WebServer4


parametr -P - wykonuje ekspozycje konkretnych portow uzytych przez dockera. Parametr wybierze pierwszy wolny port z duzego zakresu nie wchodzacy w dzialajace uslugi.

`docker run -d -ti --name WebServer5 -P  httpd`  

`docker ps`  
CONTAINER ID        IMAGE               COMMAND              CREATED             STATUS              PORTS                   NAMES
bf5328bd3430        httpd               "httpd-foreground"   5 minutes ago       Up 5 minutes        0.0.0.0:32768->80/tcp   WebServer5
cb5607dce3d4        httpd               "httpd-foreground"   13 minutes ago      Up 13 minutes       80/tcp                  WebServer4


`elinks http://172.17.0.3:80`  
`elinks http://localhost:32768`  

jak wyeksponowac konkretny port uzywamy paramertu -p pierwszy port to port na serwerze gospodarza port po : to port kontenera. np 8080:80

`docker run -d -ti --name WebServer6 -p 8080:80 httpd`  

`docker ps`  
CONTAINER ID        IMAGE               COMMAND              CREATED             STATUS              PORTS                   NAMES
dc264291efe7        httpd               "httpd-foreground"   2 minutes ago       Up 2 minutes        0.0.0.0:8080->80/tcp    WebServer6
bf5328bd3430        httpd               "httpd-foreground"   24 minutes ago      Up 23 minutes       0.0.0.0:32768->80/tcp   WebServer5
cb5607dce3d4        httpd               "httpd-foreground"   32 minutes ago      Up 32 minutes       80/tcp                  WebServer4


sprawdzamy czy dziala przy uzyciu elinksa
`elinks http://localhost:8080`  



##### Linkowanie kontenerow i flaga --net
stopujemy poprzednie kontenery:
`docker container stop $(docker container ls)`  
usuwamy wszystkie nieaktywne kontenery:
`docker container rm $(docker container ls -aq)`  


tworzymy dwa nowe kontenery w osobnych terminalach  
`docker run -ti --name server1 ubuntu /bin/bash`  

tworzymy drugi server klincki i linujemy go do pierwszego servera.  
`docker run -ti --name client --link server1 ubuntu /bin/bash`  


updateujemy i instalujemy netcat  
`apt update && apt install netcat -y`  


uruchamiamy netcata z nasuchiwaniem na konkretny port:
`nc -lp 3456`  

na serwerze clienckim uruchamiamy netcata z opcja komunikacji do servera1 na porcie 3456
`nc server1 3456`  
Mozna wysylac proste komunikatu z serwera client do server1.
```
root@5ceab7987928:/# nc server1 3456
Elo
Tutaj Donald Trump
```
```
root@95d2598052fb:/# nc -lp 3456
Elo
Tutaj Donald Trump
```

```
 cat /etc/hosts 
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
172.17.0.2	server1 95d2598052fb
172.17.0.3	5ceab7987928
```

##### Tworzenie wlasnej sieci
dzieki wlasnej siecie kontenery widza sie wzajemnie nie trzeba ich linkowac.

tworzymy siec:  
`docker network create siec`  
tworzymy nowy kontener serwerowy dolaczony do dedykowanje sieci:  
`docker run -ti --net=siec --name server2 ubuntu /bin/bash`  
tworzymy nowy kontener kliencji dolaczony do dedykowanej sieci:
`docker run -ti --name client2 --net=siec ubuntu /bin/bash`  
update i isnatlujemy netcat  
`apt update && apt install netcat -y`  
uruchamiamy nowy nasluchiwanie i przesylanie jak wczesniej.  
weryfikujemy czy dziala:


```
root@b9cbdaf2d3a7:/# nc server2 2345
HALO
```
```
root@60f244603df1:/# nc -lp 2345
HALO
```

##### Polecenia sieciowe docker'a  
wyswietlenie dostepnych sieci dockerowych  
`docker network ls`  
```
NETWORK ID          NAME                DRIVER              SCOPE
06135934ba19        bridge              bridge              local
9e4d6aa5a06d        host                host                local
cf8957df087b        none                null                local
c62f77001584        siec                bridge              local
```

`docker network inspect siec`  
```
[
    {
        "Name": "siec",
        "Id": "c62f770015845aec23855bfcebdc49fb5513ee9f82d4c8bdaab496c04bbfddc7",
        "Created": "2020-05-14T13:38:53.142023378+02:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```

dobra sprzatamy poprzednie kontenery:  
`docker container rm $(docker container ls -aq)`  
startujemy 5 kontenerow uzywajac petli for:  
`for count_id in {1..5}; do docker run -ti -d ubuntu bash; done`  

`docker container ls`  
```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
03508e1f7a89        ubuntu              "bash"              29 seconds ago      Up 28 seconds                           confident_shamir
9d4c4d925fdd        ubuntu              "bash"              30 seconds ago      Up 29 seconds                           distracted_elion
3b5b3aec937a        ubuntu              "bash"              32 seconds ago      Up 30 seconds                           awesome_nash
ea19441273ee        ubuntu              "bash"              33 seconds ago      Up 32 seconds                           hopeful_hellman
0febf5c1a274        ubuntu              "bash"              34 seconds ago      Up 33 seconds                           goofy_hamilton
```

wszystkie kontenery zostaly przypisane do sieci bridge  
`docker network inspect bridge`  
utworzmy kolejne kontenry z podlaczenie do sieci: siec  
`for count_id in {1..5}; do docker run -ti -d --net siec ubuntu /bin/bash; done`  
zostaly dolaczone do sieci.  
`docker network inspect siec`  
```
[
    {
        "Name": "siec",
        "Id": "c62f770015845aec23855bfcebdc49fb5513ee9f82d4c8bdaab496c04bbfddc7",
        "Created": "2020-05-14T13:38:53.142023378+02:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "3bb4461ee4c288cdf7fa6708479e13e357f1647810d43893b441551816158cd2": {
                "Name": "musing_pasteur",
                "EndpointID": "0a03fe55bf2ce266e752a2ed14a731bdf187f8c0e342a05a56bac71a69763209",
                "MacAddress": "02:42:ac:12:00:04",
                "IPv4Address": "172.18.0.4/16",
                "IPv6Address": ""
            },
            "58592c66331dc6224a3ed0e756ab569cb28622199bd806036628736c9d2da1b1": {
                "Name": "reverent_bartik",
                "EndpointID": "6d4d2b061a3022459bdc8e23e9942820a3d47da4e995e11608874c579bafcfdf",
                "MacAddress": "02:42:ac:12:00:03",
                "IPv4Address": "172.18.0.3/16",
                "IPv6Address": ""
            },
            "97d29a24ed3c77f74c922879401a725d2b2b4b6cd4ee5706cf4cad623e650773": {
                "Name": "great_shockley",
                "EndpointID": "d3183c97f0fd8c4c1837a300fa5f18d8d1b22d6b4fc19c9495a2fed9d0a1c42c",
                "MacAddress": "02:42:ac:12:00:06",
                "IPv4Address": "172.18.0.6/16",
                "IPv6Address": ""
            },
            "d3ec006db8bf92d9bd612304ec9118b9704b299bc50e9cdf50555d428973875e": {
                "Name": "amazing_franklin",
                "EndpointID": "9a959dd7f72c096e613a9c1340477c321d62692d86145ec4e92fe964380526da",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            },
            "e00d769d7f92350edec28d4868ebaf953501f55c191553924d65d4a36dfa5856": {
                "Name": "magical_chatelet",
                "EndpointID": "a0800a7ebe45a3bd26900fab5450a117104c6876da2476a274639af5b72492a0",
                "MacAddress": "02:42:ac:12:00:05",
                "IPv4Address": "172.18.0.5/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```

##### Tworzenie wlasnej podsieci dockerowej.

`docker network create --subnet 10.10.0.0/24 --gateway 10.10.0.1 custom_net`  

`docker network ls`  
```
NETWORK ID          NAME                DRIVER              SCOPE
06135934ba19        bridge              bridge              local
5ebc03f7504c        custom_net          bridge              local
9e4d6aa5a06d        host                host                local
cf8957df087b        none                null                local
c62f77001584        siec                bridge              local
```

`docker network inspect custom_net`  
```
[
    {
        "Name": "custom_net",
        "Id": "5ebc03f7504c68356be4531fe9dc6ed0999510ff186fbedaa1f92525fbc540ef",
        "Created": "2020-05-14T14:18:28.034684758+02:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "10.10.0.0/24",
                    "Gateway": "10.10.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```

##### Usuwanie sieci dockerowej
`docker network rm custom_net`  

`docker network ls`  
```
NETWORK ID          NAME                DRIVER              SCOPE
06135934ba19        bridge              bridge              local
9e4d6aa5a06d        host                host                local
cf8957df087b        none                null                local
c62f77001584        siec                bridge              local
```

sprzatamy nasze maszyny:  
`docker container stop $(docker container ls)`  
`docker container rm $(docker container ls -aq)`  


##### Tworzenie sieci dockerowej ze statycznymi adresami ip dla kontenerow

tworzymy nowa siec dockerowa:
`docker network create --subnet 10.10.0.0/16 --gateway 10.10.0.1 --ip-range=10.10.5.0/24 --driver=bridge custome_network_bridge_5` 
```
docker network ls
NETWORK ID          NAME                       DRIVER              SCOPE
06135934ba19        bridge                     bridge              local
c973ef41f7bb        custome_network_bridge_5   bridge              local
9e4d6aa5a06d        host                       host                local
cf8957df087b        none                       null                local
```
utworzyl sie rowniez interfejs sieciowy na maszynie glownej.
```
ifconfig -a
br-c973ef41f7bb: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 10.10.0.1  netmask 255.255.0.0  broadcast 10.10.255.255
        ether 02:42:5e:f5:a0:2f  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
tworzymy kontener i podlaczamy go do sieci:  
`docker run -ti -d --network=custome_network_bridge_5 ubuntu bash`  

```
docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
62744c64606c        ubuntu              "bash"              About a minute ago   Up About a minute                       festive_dhawan
docker inspect festive_dhawan| grep IPAddr
            "SecondaryIPAddresses": null,
            "IPAddress": "",
                    "IPAddress": "10.10.5.0",
```
tworzymy nowy kontener:  
`docker run -ti -d --network=custome_network_bridge_5 ubuntu bash`  

```
docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
8e51440946d8        ubuntu              "bash"              7 seconds ago       Up 5 seconds                            brave_saha
62744c64606c        ubuntu              "bash"              2 minutes ago       Up 2 minutes                            festive_dhawan
```
```
docker inspect brave_saha| grep IPAddr
            "SecondaryIPAddresses": null,
            "IPAddress": "",
                    "IPAddress": "10.10.5.1",
```
Jak widac adresy ip sa przydzielana zgodnie z polityka sieci.  

mozna nadawac adresy ip przy tworzeniu kontenera fla --ip:  
`docker run -ti -d --network=custome_network_bridge_5 --ip 10.10.3.1  ubuntu bash`  
```
docker inspect silly_lamport| grep IPAddr
            "SecondaryIPAddresses": null,
            "IPAddress": "",
                    "IPAddress": "10.10.3.1",

```

#### Wolumeny danych.
tworzymy nowy kontener i mapujemy obenca siezke na glownej maszynie na folder mapped w kontenerze
`docker run -ti -d --name NewContainer1 -v "$PWD":/mapped ubuntu /bin/bash`  

```
docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
119c2504a0cd        ubuntu              "/bin/bash"         10 seconds ago      Up 9 seconds                            NewContainer1
```
```
docker attach NewContainer1 
root@119c2504a0cd:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  mapped  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@119c2504a0cd:/# ls mapped/
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos  examples.desktop

```

jak widac katalog sie podmontowal :)

##### wolumeny ephemeral

`docker run -ti --name V1 -v /sharedata ubuntu bash`  
`touch ./sharedata/data.txt && echo "very important file." >> ./sharedata/data.txt`  
`cat sharedata/data.txt`  
 very important file.  

 wrzucamy konterner w tlo ctrl+p ctrl+q

 `docker run -ti --name V2 --volumes-from V1 ubuntu bash`  

```
root@969cb8656ba7:/# cat sharedata/data.txt 
very important file.
root@969cb8656ba7:/# echo "Dont delete it" >> sharedata/data.txt 
```
Sprawdzamy czy widac dodany tekst na pierwszym konenerze po czym zamykamy go.
```
docker attach V1 
root@e955e4e08b2a:/# cat sharedata/data.txt 
very important file.
Dont delete it
root@e955e4e08b2a:/# exit
exit
```

tworzymy klejny kontener ktory ma korzystac z volumenu kontenera V1

`docker run -ti --name V3 --volumes-from V1 ubuntu bash`  

```
oot@bf5f2a926f9e:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  sharedata  srv  sys  tmp  usr  var
root@bf5f2a926f9e:/# echo "ok.." >> sharedata/data.txt 
root@bf5f2a926f9e:/# cat sharedata/data.txt 
very important file.
Dont delete it
ok..
root@bf5f2a926f9e:/# exit
exit
```
wychodzimy z niego i podlaczamy sie do V2
```
docker attach V2
root@969cb8656ba7:/# cat sharedata/data.txt 
very important file.
Dont delete it
ok..
```
Jak widac nasze dane pomimo wylaczenia V1 dalej sa wspoldzielone. a jakby go usunac ?

`docker container rm V1`  


laczymy sie znowy do V2
```
docker attach V2
root@969cb8656ba7:/# cat sharedata/data.txt 
very important file.
Dont delete it
ok..
```
Dane dalej sa wspoldzielone. 
Dane ktore zostaly we wspoldzielonych kontenerach dalej beda dostepne do momentu zatrzymania kontenerow. 
Gdy usuniemy kontener ze wspoldzielonym zasobem:
Nie stworzymy juz nowego kontenera na bazie volumenu z V1.
Kontenery ktore wspoldziela zasob beda go posiadaly do momentu wylaczenia.
Po wylaczeniu nie beda mialy skad juz zaciaganc danych.

##### wolumeny persistent
 na przykladzie serwera www apache.

 na maszynie glownej tworzymy katalgo www, w srodku towrzymy dwa pliki html.index oraz newHtml.index wypelniamy roznymi danymi.

 tworzymy kontener z obrazu httpd z wyeksponowanym portem 80 oraz podmontowanym volumenem z maszyny golownej.

 `docker run -tid --name WebServer00 -p 8080:80 -v "$PWD":/usr/local/apache2/htdocs/ httpd`  

 sprawdzamy w przegladarce 
 http://localhost:8080/index.html
 http://localhost:8080/newIndex.html



 ##### docker cp

`docker run -itd --name copyServer ubuntu bash`  
tworzymy plik do kopiowania.
`mkdir testData && touch ./testData/importantFile.txt && echo "very important file dont delete it." >> ./testData/importantFile.txt`  

jak wykonac kopie do kontenera. docker cp sciezka_nazwa_pliku nazwa/id kontenera:sciezka_nazwa_nowego_pliku
`docker cp ./testData/importantFile.txt  copyServer:/plik.txt`  

```
docker attach copyServer 
root@8cf697ceff13:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  plik.txt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@8cf697ceff13:/# cat plik.txt 
very important file dont delete it.
```

kopiowanie z kontenera do maszyny glownej.  
` docker cp copyServer:/plik.txt ./file_important.txt`  


##### docker volume

`docker volume create vol-01`  

`cd /var/lib/docker/volumes/`  
```
/var/lib/docker/volumes# ll
total 40
drwx------  4 root root  4096 maj 14 18:40 ./
drwx--x--x 14 root root  4096 maj 14 10:54 ../
drwxr-xr-x  3 root root  4096 maj 14 16:09 8d8e12ca3f1555ee5551e9dd24500ad291725936329f4107e0461ce599fb837d/
-rw-------  1 root root 32768 maj 14 18:40 metadata.db
drwxr-xr-x  3 root root  4096 maj 14 18:40 vol-01/
```

`docker run -ti -d --name TestVolumeContainer --mount source=vol-01,target=/dane ubuntu bash`  
w kontnerze
`touch /dane.noweDane.txt`  
```
root@91e063d4b81e:/dane# ls
noweDane.txt
```
wrzucamy kontener w tlo.
wyswietlamy zawartosc volumenu na dysku maszyny glownej.
`sudo ls /var/lib/docker/volumes/vol-01/_data`  
noweDane.txt

```
docker volume inspect vol-01
[
    {
        "CreatedAt": "2020-05-14T19:57:03+02:00",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/vol-01/_data",
        "Name": "vol-01",
        "Options": {},
        "Scope": "local"
    }
]
```

nie uzywane wolumeny mozna usunac poleceniem:
`docker volume prune`  



#### Dockerfile
Dockerfile - jest to plik na podstawie ktrego mozemy zbudowac bardzo spersonalizowany image dla kontenera.
tworzymy dockerfile
`touch dockerfile`  


wrzucamy do srodka:
```
# Dockerfile
FROM ubuntu:latest
MAINTAINER Your Name <your_mail@gmail.com>
LABEL maintainer="Your_name <your_mail@gmail.com>"
```

budujemy obraz komenda:
`docker build -t imagefromdockerfile .`  
output:
```
Sending build context to Docker daemon  2.048kB
Step 1/3 : FROM ubuntu:latest
 ---> 1d622ef86b13
Step 2/3 : MAINTAINER Your Name <your_mail@gmail.com>
 ---> Running in f12c48a4dab1
Removing intermediate container f12c48a4dab1
 ---> f9974b9abb80
Step 3/3 : LABEL maintainer="Your_name <your_mail@gmail.com>"
 ---> Running in e37663e4fd5a
Removing intermediate container e37663e4fd5a
 ---> ccca2d65c567
Successfully built ccca2d65c567
Successfully tagged imagefromdockerfile:latest
```

obraz wybudowany i gotowy do uzytku:
```
docker image ls
REPOSITORY            TAG                 IMAGE ID            CREATED              SIZE
imagefromdockerfile   latest              ccca2d65c567        About a minute ago   73.9MB
ubuntu                latest              1d622ef86b13        3 weeks ago          73.9MB
```


ok dodajmy do Dockerfile'a polecenie RUN - pozwala on doinstalwoywac zmiany w obrazie.
dodajemy update i insalacje vima.
```
# Dockerfile
FROM ubuntu:latest
MAINTAINER Your Name <your_mail@gmail.com>
LABEL maintainer="Your_name <your_mail@gmail.com>"
RUN apt update
RUN apt upgrade -y
RUN apt install vim -y
```

`docker build -t imagefromdockerfile .`  

po ponowym wybudowaniu:
```
docker images
REPOSITORY            TAG                 IMAGE ID            CREATED             SIZE
imagefromdockerfile   latest              488e5b91d410        13 seconds ago      168MB
ubuntu                latest              1d622ef86b13        3 weeks ago         73.9MB
```

Zmniejszmy ilosc linijek dockerfile optymalizujac polecenia:
```
# Dockerfile
FROM ubuntu:latest
MAINTAINER Your Name <your_mail@gmail.com>
LABEL maintainer="Your_name <your_mail@gmail.com>"
RUN apt-get update -y && apt-get upgrade -y && apt-get install vim -y
```
`docker build -t imagefromdockerfilenext .`  

`docker images`  
```
REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
imagefromdockerfilenext   latest              8354ed256271        5 seconds ago       167MB
imagefromdockerfile       latest              488e5b91d410        40 minutes ago      168MB
ubuntu                    latest              1d622ef86b13        3 weeks ago         73.9MB
```

jak widac rozmiar zmniejszyl sie o 1 MB :)

##### dockerfile user

dodajemy doi

