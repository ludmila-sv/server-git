# server-git
Creating a Git repo on remote server

1. Create a repo in a folder
git --git-der=ls-web.git init

#---------------

2.a A bare repo
You cannot push to a non-bare repo. ... 
Actually you can - you cannot push to the currently checked out branch.
In a bare repo all branches are not checked out.

It is impossible to use git add . with a bare repo, it has no work tree.

Make a repo bare:
cd ls-web.git
(pwd - a command to check in which folder we are)
git --bare update-server-info
or
git config --bool core.bare true

#---------------

2.b None-bare repo
git config --unset core.bare
or
git config --bool core.bare false

Commands with a repo in a folder different from .git:
git --git-der=ls-web.git add .
git --git-der=ls-web.git commit -m 'initial'

#---------------

3. Set user to be displayed in the commits tree:
git config --global user.name "Ludmila Sviridova"
git config --global user.email "ls@ya.ru"

see file .gitconfig in the root folder.

#---------------

4. Deploy
a. Bare repo
- create a file hooks/post-receive with the following content:
#!/bin/sh
#
git --work-tree=/home/users/l/ludmila-sv/domains/ls-web.ru/ checkout -f

- make it executable:
chmode +x hooks/post-receive

b. None-bare repo
git config receiveCurrentBranch updateInstead
the value updateInstead appeared since version git 2.4. For earlier versions use the value "ignore",
but in this case you'll need to run the command on the server
git --git-der=ls-web.git reset --hard
after each push.

#---------------

SSH connection (for jino ru)

1. create a folder in the root:
.ssh
chmode 700 .ssh/

2. create a file authorized_keys in this folder
chmode 600 authorized_keys

Put your public key into this file.

3. in the local git config file
3.1 git config core.sshCommand "ssh -i ~/.ssh/id_rsa_example -F /dev/null"
3.2 add a path to your private key puttykeyfile = C:\\Users\\Ludmila\\.ssh\\key-priv.ppk
