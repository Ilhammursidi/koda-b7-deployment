# koda-b7-deployment
## File Permissions & Ownership Practice
## Sceanrio 
You are a system administrator preparing a shared project workspace on a Linux server. there are two users and one group involved:


Users:
- koda
- dako

Group: devteam (both users are here),
The project directory is /srv/projectX

## 1. Initial setup (create users, group and directory based on scenario).
```bash
ilhammursidi@DESKTOP-SA0ROG1:~$ sudo useradd -m -s /bin/bash koda
ilhammursidi@DESKTOP-SA0ROG1:~$ sudo useradd -m -s /bin/bash dako
ilhammursidi@DESKTOP-SA0ROG1:~$ sudo groupadd devteam
ilhammursidi@DESKTOP-SA0ROG1:~$ mkdir srv
ilhammursidi@DESKTOP-SA0ROG1:~$ cd srv
ilhammursidi@DESKTOP-SA0ROG1:~/srv$ mkdir projectX
ilhammursidi@DESKTOP-SA0ROG1:~$ sudo usermod -aG devteam koda
ilhammursidi@DESKTOP-SA0ROG1:~$ sudo usermod -aG devteam dako
ilhammursidi@DESKTOP-SA0ROG1:~$ getent group devteam
devteam:x:1003:koda,dako
```
## 2. Make koda the owner and devteam the group owner of /srv/projectX.
```bash
drwxr-xr-x 3 ilhammursidi ilhammursidi 4096 Apr 16 20:57 srv
ilhammursidi@DESKTOP-SA0ROG1:~$ sudo chown koda:devteam srv
ilhammursidi@DESKTOP-SA0ROG1:~$ ls -l
total 4
drwxr-xr-x 3 koda devteam 4096 Apr 16 20:57 srv
```

## 3. Restricting access
```bash
drwxr-xr-x 2 koda devteam 4096 Apr 16 20:57 projectX
ilhammursidi@DESKTOP-SA0ROG1:~/srv$ sudo chmod 750 projectX
ilhammursidi@DESKTOP-SA0ROG1:~/srv$ ls -l
total 4
drwxr-x--- 2 koda devteam 4096 Apr 16 20:57 projectX
```

## 4. As user koda, create file and folder structure
```bash
ilhammursidi@DESKTOP-SA0ROG1:~/srv$ sudo su koda
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv$ cd projectX
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv/projectX$ touch README.md
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv/projectX$ mkdir src
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv/projectX$ mkdir data
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv/projectX$ cd src
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv/projectX/src$ touch app.sh
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv/projectX/src$ cd ..
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv/projectX$ cd src
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv/projectX/src$ ls -l
total 0
-rw-rw-r-- 1 koda koda 0 Apr 16 21:54 app.sh
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv/projectX/src$ cd ..
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv/projectX$ cd data
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv/projectX/data$ touch input.txt
```

## 5. Permissions for app.sh
```bash
-rw-rw-r-- 1 koda koda 0 Apr 16 21:54 app.sh
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv/projectX/src$ chmod ug+x app.sh
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv/projectX/src$ ls -l
total 0
-rwxrwxr-- 1 koda koda 0 Apr 16 21:54 app.sh
```

## 6. Protecting input.txt
```bash
-rw-rw-r-- 1 koda koda 0 Apr 16 21:55 input.txt
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv/projectX/data$ chmod 600 input.txt
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv/projectX/data$ ls -l
total 0
-rw------- 1 koda koda 0 Apr 16 21:55 input.txt
```

## 7. Allow group collaboration
```bash
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv$ chmod -R koda:devteam projectX/
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv$ cd projectX
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv/projectX$ chmod -R 770 src
-rwxrwx--- 1 koda devteam 0 Apr 16 21:54 app.sh
```

## 8. Change ownership too
```bash
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv$ chown -R koda:devteam projectX/
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv$ ls -l
total 4
drwxr-x--- 4 koda devteam 4096 Apr 16 21:54 projectX
```

## 9. Prevent accidental deletion
```bash
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv$ chmod -R 770 projectX/
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv$ ls -l
total 4
drwxrwx--- 4 koda devteam 4096 Apr 16 21:54 projectX
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv$ cd projectX/
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv/projectX$ ls -l
total 8
-rwxrwx--- 1 koda devteam    0 Apr 16 21:53 README.md
drwxrwx--- 2 koda devteam 4096 Apr 16 21:55 data
drwxrwx--- 2 koda devteam 4096 Apr 16 21:54 src
```

## 10. Readme Read Only
```bash
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv/projectX$ chmod 644 README.md
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv/projectX$ ls -l
total 8
-rw-r--r-- 1 koda devteam    0 Apr 16 21:53 README.md
```

## 11. Resignation and Project Handover
```bash
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv$ sudo chown -R dako:devteam projectX
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv$ ls -l
total 4
drwxrwx--- 4 dako devteam 4096 Apr 16 21:54 projectX
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv$ cd projectX
koda@DESKTOP-SA0ROG1:/home/ilhammursidi/srv/projectX$ ls -l
total 8
-rw-r--r-- 1 dako devteam    0 Apr 16 21:53 README.md
drwxrwx--- 2 dako devteam 4096 Apr 16 21:55 data
drwxrwx--- 2 dako devteam 4096 Apr 16 21:54 src
```