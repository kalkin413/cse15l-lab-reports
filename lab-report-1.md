---
layout: post
title: Remote Access
---
Remote access can be incredibly useful in various scenarios. Below are some examples:
- Access to a supercomputer/compute cluster
    - Great for training ML models!
- Isolating data - makes sure security isn't compromised
- OS Dependent software

## 1.  Installing VSCode
I didn't actually do this step. I had VSCode installed from a previous course. I'm pretty sure I just followed the steps [here](https://inst.eecs.berkeley.edu/~cs61a/su21/articles/vscode/).

This is what my VSCode setup looks like:
![](images/lab-report-1/vscode.png)

## 2. Remotely Connecting
Windows has native ssh now and I also have WSL installed so I didn't have to do any prior work to set up ssh compatibility. I got my course account [here](https://sdacs.ucsd.edu/~icc/index.php) and simply typed in `ssh cs15lfa22aq@ieng6.ucsd.edu` into my terminal. Then, I enter my password (which I had to reset) in order to log in. This works both on Windows (either command prompt, powershell, or git bash) and WSL.

* ## Ubuntu
![](images/lab-report-1/ubuntu.png)
- ## Git Bash
![](images/lab-report-1/git-bash.png)
- ## Windows Powershell
![](images/lab-report-1/pws.png)
- ## Command Prompt
![](images/lab-report-1/cmd.png)

This was the first time I was finally able to log in! There were a lot of issues with the password reset.
![](images/lab-report-1/first_login.png)
Only took 38 failed attempts and 4 hours of waiting 💀

The first time I tried to connect I saw a message saying
```
$ ssh cs15lfa22aq@ieng6.ucsd.edu
The authenticity of host 'ieng6-202.ucsd.edu (128.54.70.227)' can't be established.
RSA key fingerprint is SHA256:ksruYwhnYH+sySHnHAtLUHngrPEyZTDl/1x99wUQcec.
Are you sure you want to continue connecting (yes/no/[fingerprint])? 
Password:
```

I simply just had to type `yes` and press enter.

## 3. Trying Some Commands
This is me trying some various commands. They functioned how I expected them to. It is interesting that I can attempt to access other people's accounts but thankfully it fails. I wonder if there is some overarching entity who monitors the files on my account.

To log out, i can type `exit` or Ctrl-D.

![](images/lab-report-1/trying-commands-1.png)
![](images/lab-report-1/trying-commands-2.png)

Another interesting command is `!` followed by a number. For example, `!1` would show the first command I ran in the remote system, `!2` would show the second, and so on.


## 4. Moving Files With `scp`
To copy files from the client to a remote computer, I used `scp`

The file I used had the contents below:
```
class WhereAmI {
  public static void main(String[] args) {
    System.out.println(System.getProperty("os.name"));
    System.out.println(System.getProperty("user.name"));
    System.out.println(System.getProperty("user.home"));
    System.out.println(System.getProperty("user.dir"));
  }
}
```

Running the file using
```
$ javac WhereAmI.java
$ java WhereAmI
```
I got the following result:
![](images/lab-report-1/local-whereami.png)

As you can see, I lied earlier! I really just ran `$ java WhereAmI.java` to run the file in one step instead of two.

To copy the file into the remote computer, i ran
```
$ scp WhereAmI.java cs15lfa22aq@ieng6.ucsd.edu:~/
```
and entered the password.

Running the program provides the following result:
![](images/lab-report-1/scp.png)

As you can see, I wasn't able to use the little trick from earlier where I just ran `java WhereAmI.java`. I wonder if the servers run and older version of Java.

## 5. Setting up an SSH Key
If you haven't noticed yet, I make a lot of typos! I had plenty of failed login attemps while doing the lab. Thankfully ssh keys exist!

I ran the command `ssh-keygen` and pressed enter when prompted for a file location (doing so uses the default location in /home/{username}/.ssh/id_rsa). I then entered a password twice.

![](images/lab-report-1/keygen.png)

I went back to the server and made a directory called `.ssh`. Then I logged out and copied the id_rsa.pub file into the `.ssh` directory in the remote server. Now I can log into the remote server using the password I set earlier instead of having the remember the other password that I set even earlier. How convenient! Maybe I shouldn't have set a password when making the ssh key.

**NOTE: MAKE SURE TO COPY `id_rsa.pub` AND NOT `id_rsa`**

![](images/lab-report-1/usingkey.png)

Turns out it's not hard to change the passphrase.
I ran `ssh-keygen -p`, pressed enter once, entered my old password, and then pressed enter twice. Now there isn't a passphrase and I can easily `ssh` and `scp`

![](images/lab-report-1/reset.png)

## 6. Optimizing Remote Running

The goal in this section was to figure out the most efficient process for editing a file locally and then running it remotely. I figured out how to do so in one command:

```
$ scp WhereAmI.java cs15lfa22aq@ieng6.ucsd.edu:~/; ssh cs15lfa22aq@ieng6.ucsd.edu "javac WhereAmI.java; java WhereAmI"
```

It might seem like a long command but it comes down to 2 keystrokes if you click the "up" arrow key and press "enter".

![](images/lab-report-1/optimized.png)
