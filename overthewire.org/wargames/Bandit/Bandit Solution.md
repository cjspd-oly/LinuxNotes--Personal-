# [BANDIT](https://overthewire.org/wargames/bandit) - Unix/Linux basics
# SSH Information  
Host: bandit.labs.overthewire.org  
Port: 2220
### SSH Command
```bash
ssh bandit1@bandit.labs.overthewire.org -p 2220
```
---
# Important Commands
```bash
mktemp -d

file filename

reset

ls -l # 5th column is size in byte

2>/dev/null # redirect errors
```

# Important Keyboard Shortcuts
**CTRL+K**: Delete from cursor position to end
**CTRL+U**: Delete from start to cursor position

---

## Level 0
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

## Level 1
```bash
cat ~/readme
```

## Level 2
```bash
cat ~/-
```

## Level 3
```bash
cat ~/--spaces\ in\ this\ filename-- 

cat ~/'--spaces in this filename--'
```

## Level 4
```bash
cat inhere/...Hiding-From-You
```

## Level 5
```bash
cat ~/inhere/-file07
head ~/inhere/*

file ./*
```

## Level 6
```bash
find inhere -size 1033c ! -executable -exec file {} \; | grep text
```
**Note:** `-exec  command  this-file  end` => `-exec file {} \;`

## Level 7
```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password
```

## Level 8
```bash
 cat data.txt | grep millionth
```

## Level 9
```bash
sort data.txt | uniq -c | grep "1 "
```

## Level 10
```bash
strings data.txt | grep '='
```
**Note:** strings print only human readable text from a noisy binary file
## Level 11
```bash
base64 -d data.txt
```

## Level 12
```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m' # ROT13
```

## Level 13
```bash
xxd -r data.txt > data
file data
mv data data.gz
gzip -d data.gz
file data
mv data data.bz2
bzip2 -d data.bz2
file data
mv data data.gz
gzip -d data.gz
file data
tar xf data
file data5.bin
tar xf data5.bin
file data6.bin
mv data6.bin data6.bz2
bzip2 -d data6.bz2
file data6
tar xf data6
file data8.bin
mv data8.bin data8.gz
gzip -d data8.gz
file data8
cat data8
```

## Level 14
```bash
cat sshkey.private # copy the content of it
exit

cd path # any path to save the sshkey.private
touch sshkey.private
gedit sshkey.private # paste

ssh -i sshkey.private bandit14@localhost -p 2220

cat /etc/bandit_pass/bandit14
nc localhost 30000 # paste the level 14 password
```

## Level 15
```bash
openssl s_client -connect localhost:30001 # paste level 15 password
```

## Level 16
```bash
nmap -sV localhost -p 31000-32000 # pick with ssl/unknown
cat /etc/bandit_pass/bandit16
echo "kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx" | openssl s_client -connect localhost:31790 -quiet # level 16 password
ssh -i sshkey17.private bandit17@bandit.labs.overthewire.org -p 2220
```

## Level 17
```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 # x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO => Byebye!

```

## Level 18
```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 ls
ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme

# Alternative - spawn a bash shell
ssh bandit18@bandit.labs.overthewire.org -p 2220 -t /bin/sh
```

## Level 19
```bash
./bandit20-do
./bandit20-do whoami
./bandit20-do cat /etc/bandit_pass/bandit20
```

## Level 20
```bash
echo -n '0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO' | nc -l -p 1234 &
./suconnect 1234
```
**Note:** `1234` port number doesn't matter
## Level 21
```bash
cd /etc/cron.d/
cat cronjob_bandit21
cat /usr/bin/cronjob_bandit22.sh
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

## Level 22
```bash
cd /etc/cron.d/
cat /usr/bin/cronjob_bandit23.sh
echo "I am user bandit23" | md5sum
cat /tmp/8ca319486bfbbc3663ea0fbe81326349 # md5sum
```

## Level 23
```bash
cat /usr/bin/cronjob_bandit24.sh
sh /usr/bin/cronjob_bandit24.sh

cd /var/spool/bandit24/foo
nano steal.sh
chmod 700 steal.sh
chown bandit23:bandit23 steal.sh
cat /tmp/bandit24_pass
```

```bash
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/bandit24_pass
```

## Level 24
```bash
PASS24=gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
for i in $(seq -w 0000 9999); do
  echo "$PASS24 $i"
done | nc localhost 30002 | uniq -c
```
**Note:** PIN is 1668 (Uniq -c is used)
## Level 25
```bash
cat /etc/passwd | grep bandit26
ls -la /usr/bin/showtext
cat /bin/showtext

ls -l ~
cat bandit26.sshkey

ssh -i sshkey26.private bandit26@bandit.labs.overthewire.org -p 2220

# Pagers (more, less, man) exit automatically if output fits on screen.
# So we make sure it cannot fit.
# --- Shrink terminal size ---
stty rows 5 cols 20
ssh -i sshkey26.private bandit26@bandit.labs.overthewire.org -p 2220

# Inside Pager
# hit v -> ENTER -> this opens the file in vim
# type ':shell'

cat /etc/bandit\_pass/bandit26

```

## Level 26
```bash
ls
./bandit27-do id
./bandit27-do cat /etc/bandit_pass/bandit27
```

## Level 27
```bash
# clone locally
mktemp -d
cd /tmp/tmp.XXX
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
cd repo
cat README
```

## Level 28
```bash
git clone ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo
cd repo
cat README
git log
git show b5ed4b5a3499533c2611217c8780e8ead48609f6
```

## Level 29
```bash
git clone ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo
cd repo
cat README
git branch -a
git switch dev
git merge 
```

## Level 30
```bash
git clone ssh://bandit30-git@bandit.labs.overthewire.org:2220/home/bandit30-git/repo
cd repo
cat README
git tag
git show secret
```

## Level 31
```bash
git clone ssh://bandit31-git@bandit.labs.overthewire.org:2220/home/bandit31-git/repo
cd repo
cat README

echo 'May I come in?' > key.txt

cat .gitignore
gedit .gitignore # remove '*.txt'

git add -f key.txt

git push -u origin master
```

## Level 32
```bash
$0
cat /etc/bandit_pass/bandit32
```

---

I WON