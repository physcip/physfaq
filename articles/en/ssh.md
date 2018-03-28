Users of a Unix-system can use the program ssh, to get a shell in the CIP Pool. You can then work on the console from at home, like you were in front of the computer in the Pool.
  

### Use the following command to connect:
```
ssh st123456@ssh.physcip.uni-stuttgart.de
```

where st123456 needs to be replaced by your actual user name. You will be redirected to an available computer for login.

If you are in a network, which blocks outgoing traffic on the port 22, you can use port 443 instead, e.g. via
```
ssh -p 443 st123456@ssh.physcip.uni-stuttgart.de
```
  
Users of Windows Systems can use e.g. [Putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/).
  

### To access files the protocols SCP or SFTP can be used.
To copy the file test.txt from your desktop in the pool to your personal computer you can issue the command 
```
scp st123456@ssh.physcip.uni-stuttgart.de:~/Desktop/test.txt test.txt
```

To do it the other way around, use
```
scp test.txt st123456@ssh.physcip.uni-stuttgart.de:~/Desktop/test.txt
```

Alternatively you can use SFTP, therefore the graphical tools [WinSCP](http://winscp.net) for Windows, [Cyberduck](http://cyberduck.ch) for Mac and [FileZilla](http://filezilla-project.org) for nearly all operating systems are available. The appropriate protocol is SFTP, the server address is `ssh.physcip.uni-stuttgart.de`.
  

### Remark
If you enter the wrong username and/or password several times in a row, you will be banned for 15 minutes (this leads to error messages, such as `ssh: connect to host ssh.physcip.uni-stuttgart.de port 22: Connection refused` or in an immediate loss of connection).
  

### Public-Key-Authentication
Because we use password protected file shares in our network, only very limited SSH-Public-Key-Authentication (i.e. without password) is possible: The usage of SFTP or interactive shell sessions isn't possible without password.
Nevertheless it is possible to use so called SSH-Forwarding with Public-Key-Authentication. For the OpenSSH-Client the options -L, -W and -D are of interest. To tell the server, that you wish Public-Key-Authentication, you need to prefix your username with key-, as in key-st123456. To suppress a warning use the -N option.
  
Like that you can forward a website, only accessible from the university network to your local computer:
```
ssh key-st123456@ssh.physcip.uni-stuttgart.de -N -L 8080:www.verwaltung.uni-stuttgart.de:80
```

Now the university administration's webseite is available at [http://localhost:8080](http://localhost:8080) .
  
You can do SSH-Bouncing that way. Edit you ~/.ssh/config according to the following scheme

```
Host *.icp.uni-stuttgart.de
User mmustermann
ProxyCommand ssh -W %h:%p key-st123456@ssh.physcip.uni-stuttgart.de
```
  
This is very useful to access other computers via SSH, which are accessible from university network, but not from outside. Here, as an example the computers of the Institute for Computational Physics are given.
Now you can `ssh cip12.icp.uni-stuttgart.de` from at home, despite of the fact, that it is not available from the internet.
Please note: Some tutorials for SSH-Bouncing on the internet suggest lines like

```
ProxyCommand ssh key-st123456@ssh.physcip.uni-stuttgart.de nc %h %p
```

They make use of the tool netcat and do essentially the same as the above, but are **NOT** supported for our Public-Key-Authentication.
