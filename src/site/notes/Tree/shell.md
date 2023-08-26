---
{"dg-publish":true,"permalink":"/tree/shell/","tags":["CS/operating-system/linux"],"created":"2021-10-16T23:24:20.000+08:00","updated":"2023-08-27T05:00:15.095+08:00"}
---


## man

to find help! (manul pages) like ：man man

## pwd

 pwd   print working directory

## ls

 ls    lists  

==ls -a can expose those .name type hidden files== 

ls-l in long format  

ls-t by the time they were last modified



## cd 

cd   change  directory

## cd ..

Cd .. back up to the parent directory 

## cd -

cd - go back to the previous directory


## cd ~

cd ~ home directory of the use

## clear / cmd+K

clear the screen

## open

open

## ./

execute file



## touch

creat a new file

## rm

rm stands for remove 

- rm -r remove a directory

## cp

copy files

## mv

move and rename file

## tab

autocomplete what you type 

## sudo

do as sue, do the following things as a superuser

sudo su get you a shell as super user

## nano

edit  (nano is an editor which is simpler than Vim)

## ⬆︎⬇︎

go through the previous lines

## history

to see the history 

## mkdir

mkdir make directory

## cat

The cat command outputs the contents of a file to  the terminal. When you type cat hello. txt, the

 contents of hello.txt are displayed.

## tail

tail prints the last end line of its input 

## lsof

lsof is a command meaning "list open files", which is used in many Unix-like systems to report a list of all open files and the processes that opened them.

- lsof -i tcp:port : show info of this port

for example:
```shell
~ lsof -i tcp:8080
COMMAND   PID USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
java    95423   re  125u  IPv6 0x531851168af9827b      0t0  TCP *:http-alt (LISTEN)
```



