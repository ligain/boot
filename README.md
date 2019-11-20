# Boot loading  practice
This homework contains 3 ways to load linux as the root user by modifying boot config.
1)  [init=/bin/sh](https://www.youtube.com/watch?v=iOfOsvPHSeQ)
2) [rd.break](https://www.youtube.com/watch?v=9i_TmqcsxHU)
3) [rw init=/sysroot/bin/sh](https://www.youtube.com/watch?v=VWnMr78wkvM)

The second part of homework is to add a new module to `initrd`
Follow steps in below `Run` section to do this.

# Run  

0) `vagrant`  should be installed on your system
```
$ vagrant -v
Vagrant 2.2.5
```
1) Clone this repository
```bash  
$ git clone https://github.com/ligain/boot.git  
``` 
2) Go to project folder
```bash  
$ cd boot/
```  
3) Run `Vagrantfile`
```bash  
$ vagrant up
Long output here
...
```
4) Reload VM
```
$ vagrant reload
``` 
5) You will have to see a penguin in virtual box window of the VM. [Like this. ](https://www.youtube.com/watch?v=AsSgJK_SUYM)

# Project Goals 
The code is written for educational purposes.