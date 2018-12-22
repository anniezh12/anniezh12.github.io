---
layout: post
title:      "Setting Up Oracle Virtual Machine "
date:       2018-12-22 19:24:50 +0000
permalink:  setting_up_oracle_virtual_machine
---

###                Virtualization

>What is Oracle Virtual Machine?

>Oracle VM is a free and open source solution to run other operating systems on your PC.

>What is virtualization?

>It is a process of running different Operating Systems on your PC (referred to as a host) while the resulting VM manager can be used to create as many virtual boxes as needed. These are referred to as guests.

>A simple way to define the process of virtualization as a 'Virtual Machine' is a guest house which can have one or many guest rooms,
 where a guest (any Operating System) can live/perform as if they are in their home.

###                                   SETTING UP THE VIRTUAL MACHINE MANAGER TO USE LINUX OS

To install the `Oracle Virtual Machine` on my PC, I went to the following link
https://www.virtualbox.org/wiki/Dowlaods.

Then I chose `Windows Hosts` since my PC had Windows OS. Then I followed the following steps:

>Please note, I am installing this virtual machine/virtual box for the sake of showing how I did this earlier, where not only I installed the Virtual Machine but also the Ubuntu OS.  

1. Once the download was complete, I chose **New** to create a new Virtual Box named 'test'.
![](https://imgur.com/a/nLVEsh2)

2. I kept default settings for the memory size in this part.
![](https://imgur.com/a/E9rLGxw)

3. In this step I also kept the default option of `Create a virtual hard disk now` and then hit the `Create` button.
![vm](images/oracle-vm/vm-hard.png)

4. In the next step I kept the default option for the `Hard disk file type` and hit `Next`.

5. I selected `dynamically allocate` and hit `Next`.

![vm](images/oracle-vm/dynamic-storage.png)

6. In the next step I kept the default settings for `File location and size` and hit `create`.

7. The virtual box was created. However, I could see the other virtual boxes as well since I had been working in another 'virtual box'  where I did my entire assignment.

![vm](images/oracle-vm/vb-created.png)

8. In order to start this newly created vb `test`, I selected `test` and hit `start` (Green arrow). A new text box appeared prompting me to select `virtual optical disk` file.

![vm](images/oracle-vm/vb-select-location.png)

9. I selected the `browser icon` and then selected the Ubuntu file as shown in the following image.

 ![virtual-optical-disk-option](images/oracle-vm/virtual-optical-disk-option.png)

10. After selecting the file, I hit `start`.

 ![virtual optical ](images/oracle-vm/vo-disk-start.png)

11. Finally the virtual box `test` started running  with a welcome screen prompting to either install `Ubuntu` or use `Ubuntu Mate`. 
I used Ubuntu Mate this time (Note: I installed ubuntu for the VB which I used to complete my Datadog assignment)

 ![Welcome ](images/oracle-vm/welcome.png)

12. The following image shows the virtual box, `test`, running.

![virtual box test ](images/oracle-vm/vb-test-running.png)

13. I could now select the terminal and start working in it.

![selecting terminall ](images/oracle-vm/selecting-terminal.png)

14. The following image shows the `Command Line Interface/terminal`.

![terminal ](images/oracle-vm/vb-terminal.png)

15. At this point, my system had nothing installed. I planned to work in Ruby, Rails and Atom (text editor) so the next step was to install these software using the terminal command `$ sudo apt-get install name-of-software`.

![checks for softwares](images/oracle-vm/checks-for-softwares.png)

>Tip: The easiest way to check if a system has a perticular program installed is to use ** $ software -v** command (where v stands for version)

14. I tried to install ruby by using `$ sudo apt-get install ruby`, it didn't install `Ruby` instead a system update was suggested. I then updated the system by `$ sudo apt-get update`, which started the update process.
Once the system updated, I installed ruby with no difficulty using `$ sudo apt-get install ruby`.

![Ruby installed ](images/oracle-vm/ruby-installed.png)

###                            SETTING UP GIT IN THE LINUX TERMINAL

In order to complete `Datadog Solutions Engineer` assignment, I had to configure Git in my new Virtual box
that I just created in the above section.

>What is GitHub?
>GitHub is an open source version control system (VCS) commonly known as Git. It is responsible for everything
GitHub-related that happens locally on our computers.

1. In order to use Github, one has to have an account on Github.
2. I used terminal command to install git in my local environment using `$ sudo apt-get install git`.

 ![git installation](images/oracle-vm/git-install.png)

 checking git version

 ![git installation](images/oracle-vm/git-version.png)

3. After installing the git in my local environment, I had to connect it with the remote GitHub server which I did by creating a `SSH key`, more info in
this link (https://help.github.com/articles/connecting-to-github-with-ssh/).

 `$ ssh-keygen -t rsa -b 4096 -C "my_email@example.com"`

 I hit enter for all the prompt e.g passphrase and location.

  ![git installation](images/oracle-vm/creating-ssh-key.png)

  The following image shows key generated.

   ![git installation](images/oracle-vm/key-generated.png)

I opened this key using `$ cat .ssh/id_rsa.pub` or `cat ~/.ssh/id_rsa.pub`, it showed a long key that I copied to use in my github account.

4. Next I started ssh agent in the backgroung by using the command `$ eval "$(ssh-agent -s)"`.

 ![git installation](images/oracle-vm/ssh-agent.png)

 5. I then used the key (from step 3) and added it to my github account. In order to do this I went to my Github account and selected the `SSH and GPG keys` menu option. Next, I selected the `New SSH key` button which showed options to add a `Title` and a `Key`. I then gave a title to this key and pasted the key that I copied earlier from the terminal (step 3) and hit the `Add SSH Key` button. Now I was connected to GitHub and was able to perform all the Git tasks (ex. cloning, pushing etc).
 I then cloned the Datadog hiring-engineers repository in my terminal using `$ git clone git@github.com:DataDog/hiring-engineers.git` and started working on it.

