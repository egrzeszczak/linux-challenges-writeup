# Linux Challenges Write-up

# Task 2 The Basics

## Flag 1 - What is flag 1?

```sh
garry@ip-10-10-55-77:~$ ls -l
total 20
-rw-rw-r-- 1 garry ubuntu  190 Feb 19  2019 flag1.txt 	# <------------
-rwxrwxr-x 1 garry ubuntu 8656 Feb 20  2019 flag24
-rw-rw-r-- 1 garry ubuntu 3589 Feb 20  2019 flag29
```
```
garry@ip-10-10-55-77:~$ cat flag1.txt 
There are flags hidden around the file system, its your job to find them.

Flag 1: *:)*

Log into bobs account to get flag 2.

Username: bob
Password: linuxrules

```

## Flag 2 - What is flag 2?

```sh
$ su bob
$ cd ~
$ ls -l

bob@ip-10-10-55-77:~$ ls -l
total 48
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Desktop
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Documents
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Downloads
drwxrwxr-x 2 bob bob 4096 Feb 18  2019 flag13
-rw-rw-r-- 1 bob bob   65 Feb 20  2019 flag21.php
-rw-rw-r-- 1 bob bob   41 Feb 18  2019 flag2.txt	# <------------
-rw-rw-r-- 1 bob bob  149 Feb 18  2019 flag8.tar.gz
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Music
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Pictures
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Public
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Templates
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Videos
```

```sh
$ cat flag2.txt
Flag 2: *:)*
```

## Flag 3 - Flag 3 is located where bob's bash history gets stored.

```sh
bob@ip-10-10-55-77:~$ ls -la
total 204
drwxr-xr-x 21 bob  bob   4096 Feb 20  2019 .
drwxr-xr-x  6 root root  4096 Feb 20  2019 ..
-rw-rw-r--  1 bob  bob    308 Jan  8 23:14 .bash_history # <-----
-rw-r--r--  1 bob  bob    220 Feb 18  2019 .bash_logout
-rw-r--r--  1 bob  bob   3890 Feb 19  2019 .bashrc
...
```

```sh
bob@ip-10-10-55-77:~$ cat ~/.bash_history 
*:)*
cat ~/.bash_history 
rm ~/.bash_history
vim ~/.bash_history
exit
ls
crontab -e
ls
cd /home/alice/
ls

```

## Flag 4 - Flag 4 is located where cron jobs are created.
```
bob@ip-10-10-55-77:~$ crontab -e

...
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
#
# For more information see the manual pages of crontab(5) and cron(8)
#
# m h  dom mon dow   command

0 6 * * * echo 'flag4:*:)*' > /home/bob/flag4.txt

...
```

## Flag 5 - Find and retrieve flag 5.
```sh
$ find / -xdev -name "flag5*" 2> /dev/null
```
- `-xdev` - Nie szukaj w innych systemach plikow
- `-name` - Szukaj nazwy
- `2> /dev/null` - Przekierowanie bledow do czarnej dziury :)
```sh
bob@ip-10-10-55-77:~$ find / -xdev -name "flag5*" 2> /dev/null
/lib/terminfo/E/flag5.txt # <-----------
```
```sh
bob@ip-10-10-55-77:~$ cat /lib/terminfo/E/flag5.txt 
*:)*
```
Przy uzyciu ` | head -n 20 ` mozemy zobaczyc sciezke do innych flag, przyda nam sie to na potem
```
bob@ip-10-10-55-77:~$ find / -xdev -name "flag*" 2> /dev/null | head -n 20
/var/log/flagtourteen.txt
/lib/terminfo/E/flag5.txt
/home/flag27
/home/bob/flag8.tar.gz
/home/bob/flag13
/home/bob/flag21.php
/home/bob/flag2.txt
/home/flag6.txt 	# <------- Nastepna flaga
/home/garry/flag1.txt
/home/garry/flag29
/home/garry/flag24
/home/alice/flag22
/home/alice/flag32.mp3
/home/alice/flag23
/home/alice/flag20
/home/alice/flag19
/home/alice/flag17
/etc/flag36
```
## Flag 6 - "Grep" through flag 6 and find the flag. The first 2 characters of the flag is c9.


```sh
bob@ip-10-10-55-77:~$ cat /home/flag6.txt | grep c9
Sed sollicitudin eros quis vulputate rutrum. Curabitur mauris elit, elementum quis sapien sed, ullamcorper 
pellentesque neque. Aliquam erat volutpat. Cras vehicula mauris vel lectus hendrerit, sed malesuada 
ipsum consectetur. Donec in enim id erat condimentum vestibulum *:)* vitae eget nisi. 
Suspendisse eget commodo libero. Mauris eget gravida quam, a interdum orci. Vestibulum ante ipsum primis 
in faucibus orci luctus et ultrices posuere cubilia Curae; 
Quisque eu nisi non ligula tempor efficitur. Etiam eleifend, odio vel bibendum mattis, 
purus metus consectetur turpis, eu dignissim elit nunc at tortor. Mauris sapien enim, elementum faucibus
 magna at, rutrum venenatis ipsum.

```

## Flag 7 - Look at the systems processes. What is flag 7.
```
EXAMPLES
       To see every process on the system using standard syntax:
          ps -e
          ps -ef
```
```sh
$ ps -ef
...
...
root      1337     1  0 23:06 ?        00:00:00 /snap/amazon-ssm-agent/1068/amazon-ssm-agent
mysql     1356     1  0 23:06 ?        00:00:00 /usr/sbin/mysqld
root      1381     1  0 23:06 ?        00:00:00 flag7:*:)* 1000000
whoopsie  1388     1  0 23:06 ?        00:00:00 /usr/bin/whoopsie -f
root      1399     1  0 23:06 ?        00:00:00 /usr/sbin/sshd -D
root      1405     1  0 23:06 ?        00:00:00 /sbin/iscsid
...
...
```

## Flag 8 - De-compress and get flag 8.
Flaga jest w `/home/bob`

```sh
$ gunzip flag8.tar.gz # poniewaz rozszerzenie .gz
$ tar -xf flag8.tar # poniewaz .tar
```
```sh
bob@ip-10-10-55-77:~$ cat flag8.txt
*:)*
```

## Flag 9 - By look in your hosts file, locate and retrieve flag 9.

Plik hostow - `/etc/hosts`

```sh
bob@ip-10-10-55-77:~$ cat /etc/hosts
127.0.0.1 localhost

# The following lines are desirable for IPv6 capable hosts
::1 ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
ff02::3 ip6-allhosts

127.0.0.1       *:)*.com # <---------------

```

## Flag 10 - Find all other users on the system. What is flag 10.

Plik z uzytkownikami - `/etc/passwd`
```sh
bob@ip-10-10-55-77:~$ cat /etc/passwd
...
pollinate:x:111:1::/var/cache/pollinate:/bin/false
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
bob:x:1001:1001:Bob,,,:/home/bob:/bin/bash
5e23deecfe3a7292970ee48ff1b6d00c:x:1002:1002:,,,:/home/5e23deecfe3a7292970ee48ff1b6d00c:/bin/bash
alice:x:1003:1003:,,,:/home/alice:/bin/bash
mysql:x:112:117:MySQL Server,,,:/nonexistent:/bin/false
xrdp:x:113:118::/var/run/xrdp:/bin/false
...
```

# Task 3

## Flag 11 - Run the command flag11. Locate where your command alias are stored and get flag 11.

```sh
bob@ip-10-10-55-77:~$ flag11
You need to look where the alias are created...
```
Plik gdzie przechowywane sa aliasy - `~/.bashrc`
```sh
bob@ip-10-10-55-77:~$ cat ~/.bashrc
...
#custom alias
alias flag11='echo "You need to look where the alias are created..."' #*:)*
...

```


## Flag 12 - Flag12 is located were MOTD's are usually found on an Ubuntu OS. What is flag12?
Plik z MOTD - `/etc/update-motd.d`

```sh
bob@ip-10-10-55-77:~$ cd /etc/update-motd.d
bob@ip-10-10-55-77:/etc/update-motd.d$ ls
00-header     51-cloudguest         91-release-upgrade  98-fsck-at-reboot   99-esm
10-help-text  90-updates-available  97-overlayroot      98-reboot-required  logo.txt
bob@ip-10-10-55-77:/etc/update-motd.d$ cat 00-header 
```
Przeszukujemy kazdy po koleji, ale na szczescie flaga znajduje sie w pierwszym pliku `00-header`

```
...
# Flag12: *:)*
...
```


## Flag 13 - Find the difference between two script files to find flag 13.

Od szukania roznic jest program `diff`
```sh
bob@ip-10-10-55-77:~/flag13$ diff script1 script2
2437c2437
< Lightoller sees Smith walking stiffly toward him and quickly goes to him. 
He yells into the Captain's ear, through cupped hands, over the roar of the steam... 
---
> Lightoller sees *:)* Smith walking stiffly toward him a
nd quickly goes to him. He yells into the Captain's ear, through cupped hands, 
over the roar of the steam... 
```

## Flag 14 - Where on the file system are logs typically stored? Find flag 14.

Logi zazwyczaj sa przechowywane w katalogu `/var/log`
```sh
bob@ip-10-10-55-77:~$ cd /var/log
bob@ip-10-10-55-77:/var/log$ ls -la | grep flag
-rwxr-xr-x  1 root              root    518561 Feb 18  2019 flagtourteen.txt # <------------
```
Flaga znajduje sie na samym koncu tekstu

## Flag 15 - Can you find information about the system, such as the kernel version etc. Find flag 15

Szukamy: 

```sh
bob@ip-10-10-55-77:~$ find / -xdev -name "*release*" 2> /dev/null 
/var/lib/dpkg/info/lsb-release.postinst
/var/lib/dpkg/info/lsb-release.md5sums
/var/lib/dpkg/info/ubuntu-release-upgrader-core.list
/etc/lsb-release # <----------
/etc/update-manager/meta-release
/etc/update-manager/release-upgrades.d
/etc/update-manager/release-upgrades
/etc/os-release # <----------
/etc/update-motd.d/91-release-upgrade
/usr/share/polkit-1/actions/com.ubuntu.release-upgrader.policy
/usr/share/lintian/data/standards-version/release-dates

```
Znajdujemy dwa pliki ktore nas interesuja:
`lsb-release` i `os-release`

Akurat flaga znajduje sie w `lsb-release`

```sh
bob@ip-10-10-55-77:/etc$ cat /etc/lsb-release 
FLAG_15=*:)*
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=16.04
DISTRIB_CODENAME=xenial
DISTRIB_DESCRIPTION="Ubuntu 16.04.5 LTS"
```

## Flag 16 - Flag 16 lies within another system mount.

Do glowy przychodzi katalog `/media/`
```sh
bob@ip-10-10-55-77:/media$ ls
f # <-------- ?

bob@ip-10-10-55-77:/media/f/l/a/g/1/6/is/*:)*
```

## Flag 17 - Login to alice's account and get flag 17. Her password is TryHackMe123

```sh
alice@ip-10-10-55-77:~$ ls -l
total 128
-rw-rw-r-- 1 alice alice    33 Feb 18  2019 flag17
-rw-rw-r-- 1 alice alice 99001 Feb 19  2019 flag19
-rw-rw-r-- 1 alice alice    45 Feb 19  2019 flag20
-rw-rw-r-- 1 alice alice    96 Feb 19  2019 flag22
-rw-rw-r-- 1 alice alice    33 Feb 19  2019 flag23
-rw-rw-r-- 1 alice alice 10560 Feb 19  2019 flag32.mp3
```
```sh
alice@ip-10-10-55-77:~$ cat flag17 
*:)*
```

## Flag 18 - Find the hidden flag 18.
```sh
alice@ip-10-10-55-77:~$ ls -la
total 172
drwxr-xr-x 4 alice alice  4096 Feb 20  2019 .
drwxr-xr-x 6 root  root   4096 Feb 20  2019 ..
-rw------- 1 alice alice   518 Mar  7  2019 .bash_history
-rw-r--r-- 1 alice alice   220 Feb 18  2019 .bash_logout
-rw-r--r-- 1 alice alice  3771 Feb 18  2019 .bashrc
drwx------ 2 alice alice  4096 Feb 18  2019 .cache
-rw-rw-r-- 1 alice alice    33 Feb 18  2019 flag17
-rw-rw-r-- 1 alice alice    33 Feb 18  2019 .flag18 # <--------------------
-rw-rw-r-- 1 alice alice 99001 Feb 19  2019 flag19

```
```sh
alice@ip-10-10-55-77:~$ cat .flag18 
*:)*
```

## Flag 19 - Read the 2345th line of the file that contains flag 19.
```sh
alice@ip-10-10-55-77:~$ cat flag19 | head -n 2345
```
Ostatnia linia ktora nam sie pokazala to flaga 19.

# Task 4

## Flag 20 - Find and retrieve flag 20.

```sh
alice@ip-10-10-55-77:~$ cat flag20
MDJiOWFhYjhhMjk5NzBkYjA4ZWM3N2FlNDI1ZjZlNjg=
```
Po otwarciu pliku `flag20` widzimy oczywiscie ze to jest zakodowany string w `Base64`

Uzyjemy komendy `base64 -d flag20` zeby uzyskac odszyfrowany wynik

## Flag 21 - Inspect the flag21.php file. Find the flag.


Po zerknieciu na poprzednie wyniki mozemy sobie przypomniec ze flaga 21 jest w katalogu `/home/bob`
Ale po otwarciu nie widzimy zbyt wiele:
```sh
alice@ip-10-10-55-77:/home/bob$ cat flag21.php 
<?='MoreToThisFileThanYouThink';?>
```
Sprobojemy poleceniem `less`:

```sh

<?=`$_POST[flag21_*:)*]`?>^M<?='MoreToThisFileThanYouThink';?>
flag21.php (END)

```

## Flag 22 - Locate and read flag 22. Its represented as hex.
```sh
alice@ip-10-10-55-77:~$ cat flag22
39 64 31 61 65 38 64 35 36 39 63 38 33 65 30 33 64 38 61 38 66 36 31 35 36 38 61 30 66 61 37 64
```

Uzywamy polecenia `xxd -r -p` do odwrocenia hexdump'u :)

## Flag 23 - Locate, read and reverse flag 23.

Musimy odwrocic zawartosc flagi 23. Mozemy napisac maly skrypt w pythonie:
```py
msg = open('flag23', 'r').read()
print(msg[::-1])
```
albo uzyc polecenia `rev`
```sh
alice@ip-10-10-55-77:~$ cat flag23 | rev
*:)*
```

## Flag 24 - Analyse the flag 24 compiled C program. Find a command that might reveal human readable strings when looking in the machine code code.

Plik `flag24` znajduje sie w katalogu `/home/garry`

Mozemy skorzystac na upartego z polecenia `xxd` i przegladac recznie az znajdziemy
```
00001800: 544d 5f64 6572 6567 6973 7465 7254 4d43  TM_deregisterTMC
00001810: 6c6f 6e65 5461 626c 6500 5f65 6461 7461  loneTable._edata
00001820: 0070 7269 6e74 6640 4047 4c49 4243 5f32  .printf@@GLIBC_2
00001830: 2e32 2e35 0066 6c61 675f 3234 5f69 735f  .2.5.flag_24_is_
00001840: :):) :):) :):) :):) :):) :):) :):) :):)  :):):):):):).__l # <-------------------
00001850: 6962 635f 7374 6172 745f 6d61 696e 4040  ibc_start_main@@
00001860: 474c 4942 435f 322e 322e 3500 5f5f 6461  GLIBC_2.2.5.__da
00001870: 7461 5f73 7461 7274 005f 5f67 6d6f 6e5f  ta_start.__gmon_
```
albo uzyc polecenia `strings`
```sh
alice@ip-10-10-55-77:/home/garry$ strings flag24 | grep 24
flag24.c
flag_24_is_*:)* # <--------------------
```

## Flag 25 - Flag 25 does not exist.
:(

## Flag 26 - Find flag 26 by searching the all files for a string that begins with 4bceb and is 32 characters long. 

```sh
alice@ip-10-10-55-77:/home/garry$ find / \ 	# Szukamy w /
> -xdev \					# Omijamy inne system plikow
> -type f \					# Szukamy plikow
> -print0 \					# Poniewaz nie dajemy nazwy pliku do szukania (nizej)
> 2> /dev/null | \				# Nie pokazuj bledow dostepu
> xargs -0 \					# Biale znaki
> grep -E '^4bceb[a-f0-9]{27}' \		# Regex dla 4bcebXXXXXXXXXXXXXXXXXXXXXXXXX
> 2> /dev/null					# Nie pokazuj bledow dostepu
/var/cache/apache2/mod_cache_disk/config.json:*:)*
```
Mamy :)
```
alice@ip-10-10-55-77:/home/garry$ man find | grep print0
```
> If no expression is given, the expression -print is used (but you should probably consider using **-print0**

## Flag 27 - Locate and retrieve flag 27, which is owned by the root user.

Plik `flag27` jest w katalogu `/home/`, ale nie mozemy go przeczytac
```
-rwx------  1 root   root       33 Feb 19  2019 flag27 # Plik posiadany jest przez roota i nie mozna go czytac
```
```sh
alice@ip-10-10-55-77:/home$ cat flag27
cat: flag27: Permission denied
```
Patrzymy czy mamy jakies pozowolenia co do polecenia `sudo`
Uzywamy polecenia `sudo -l`
```sh
alice@ip-10-10-55-77:/home$ sudo -l
Matching Defaults entries for alice on ip-10-10-55-77.eu-west-1.compute.internal:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User alice may run the following commands on ip-10-10-55-77.eu-west-1.compute.internal:
    (ALL) NOPASSWD: /bin/cat /home/flag27 # <------------ Mamy tutaj

```

```sh
alice@ip-10-10-55-77:~$ sudo /bin/cat /home/flag27
*:)*
```

## Flag 28 - Whats the linux kernel version?
```sh
alice@ip-10-10-55-77:~$ uname -a
Linux ip-10-10-55-77 4.4.0-1075-aws #85-Ubuntu SMP Thu Jan 17 17:15:12 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
4.4.0-1075-aws 
```


## Flag 29 - Find the file called flag 29 and do the following operations on it:
1. Remove all spaces in file.
2. Remove all new line spaces.
3. Split by comma and get the last element in the split.

Szukamy:
```sh
alice@ip-10-10-55-77:~$ find / -xdev -name "*flag29*" -type f 2> /dev/null
/home/garry/flag29
```
Przeskakujemy:
```sh
alice@ip-10-10-55-77:~$ cd /home/garry
alice@ip-10-10-55-77:/home/garry$ 
```

Plik `flag29` zawiera jakis dlugi tekst, robimy to o co nas prosza w podpunktach.
Ja skorzystalem z pythona
```py
txt = open('flag29', 'r').read()
print("".join(txt.split(" ")))
```
```sh
Tehisetiamtritanialiquid,tehisnobisquaestio.Inpereripuitpersecutirationibus,dicoconstituamsitid.
Sitmovetquandoeligendiut,ignotainterpretariseosat.Inenimiisqueinermisvel,eimelpersiusprompta.
Neseasanctusdelicatissimi,meinecaseferrivulputate,atmelpericulisocurreret.
Dicoverearaccusamusuex,*:)*.
```

# Task 5

## Flag 30 - Use curl to find flag 30.
```sh
alice@ip-10-10-55-77:/home/garry$ curl localhost
*:)*
```

## Flag 31 - Flag 31 is a MySQL database name.
> MySQL username: root
> MySQL password: hello

```sh
alice@ip-10-10-55-77:/home/garry$ mysql -h localhost -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 5
Server version: 5.7.25-0ubuntu0.16.04.2 (Ubuntu)

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```
Aby zobaczyc jakie bazy danych znajduja sie w MySQLu uzyjemy polecenia `SHOW DATABASES;`
```sh
mysql> SHOW DATABASES;
+-------------------------------------------+
| Database                                  |
+-------------------------------------------+
| information_schema                        |
| database_*:)*			   	    |
| mysql                                     |
| performance_schema                        |
| sys                                       |
+-------------------------------------------+
```

## Flag 31 - Bonus flag question, get data out of the table from the database you found above!
Zeby zobaczyc jakie tabele kryja sie w naszej bazie danych uzyjemy polecenia:
`SHOW TABLES IN database_*:)*`
```sh
mysql> SHOW TABLES IN database_*:)*
    -> ;
+-----------------------------------------------------+
| Tables_in_database_*:)*			      |
+-----------------------------------------------------+
| flags                                               |
+-----------------------------------------------------+
1 row in set (0.00 sec)

```
Uzywamy `mysql> use database_*:)*` aby zmienic kontekst
Dalej poleceniem `SELECT * FROM flags` pokazemy zawartosc tabeli flags

```sh
mysql> SELECT * from flags
    -> ;
+----+----------------------------------+
| id | flag                             |
+----+----------------------------------+
|  1 | *:)*				|
+----+----------------------------------+
```

## Flag 32 - Using SCP, FileZilla or another FTP client download flag32.mp3 to reveal flag 32.
Tutaj musimy pobrac plik `flag32.mp3` ze zdalnej maszyny na swoj komputer
`scp alice@10.10.55.77:/home/alice/flag32.mp3 ~/flag32.mp3`
Po odtworzeniu uslyszymy czytany tekst-flage :)

## Flag 33 - Flag 33 is located where your personal $PATH's are stored.
Osobisty plik z $PATH to plik `.profile` w domowym katalogu kazdego uzytkownika.
Akurat nasza flaga znajduje sie w katalogu uzytkownika `bob`
```sh
bob@ip-10-10-55-77:~$ cat ./.profile 
#Flag 33: *:)*
```

## Flag 34 - Switch your account back to bob. Using system variables, what is flag34?
Proste. Uzywamy polecenia `export`
```sh
bob@ip-10-10-55-77:~$ echo $flag34
*:)*
```

## Flag 35 - Look at all groups created on the system. What is flag 35?
O wszystkich grupach na danym systemie mozemy sie dowiedziec z pliku `/etc/group`
```sh
bob@ip-10-10-55-77:~$ cat /etc/group
gdm:x:131:
flag35_*:)*:x:1005:
garry:x:1006:
```

## Flag 36 - Find the user which is apart of the "hacker" group and read flag 36.
Znajdzmy sobie najpierw plik `flag36`
```sh
bob@ip-10-10-55-77:/home$ find / -xdev -name "*flag36*" 2>/dev/null
/etc/flag36
```
Po dalszej analizie mozemy zobaczyc ze plik moze przeczytac tylko uzytkownik w grupie `hacker`
```sh
bob@ip-10-10-55-77:/home$ ls /etc/flag36 -la
-rw-r----- 1 root hacker 33 Feb 19  2019 /etc/flag36 
#      ^^^ # Zwykly user go nie przeczyta jesli nie jest w grupie
```

Zobaczmy czy jestesmy w grupie `hacker`
```
bob@ip-10-10-55-77:/home$ id 
uid=1001(bob) gid=1001(bob) groups=1001(bob),1004(hacker) # <------ Tak 
```
Wiec mozemy go przeczytac
```
bob@ip-10-10-55-77:/home$ cat /etc/flag36
*:)*
```























