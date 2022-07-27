---
categories:
  - awesome-applications
  - git
tags:
  - git
  - centos
layout: post
title: Setting up a Git Server in CentOS 6.5
created: 1392682017
---

1). Install git

```bash
[root@svn ~]# yum install git
```

2). Add the developers group, all git users will be part of this group.

```bash
[root@svn ~]# groupadd developers
```

3). Create the git user which will own all the repos.

```bash
[root@svn ~]# useradd -s /sbin/nologin -g developers git
[root@svn ~]# passwd git
Changing password for user git.
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.
```

4). Update Permissions.

```bash
[root@svn ~]# chmod 2770 /home/git/
```

5). Create an empty Git repo.

```bash
[root@svn project1]# git init --bare --shared
Initialized empty shared Git repository in /home/git/project1/
```

6). Update file ownership and permissions.

```bash
[root@svn project1]# chown -R git .
[root@svn project1]# chmod 2770 /home/git/project1
```

7). Create a git user account.

```bash
[root@svn git]# useradd -s /usr/bin/git-shell -g developers -d /home/git tony
useradd: warning: the home directory already exists.
Not copying any file from skel directory into it.
[root@svn git]# passwd tony
Changing password for user tony.
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.
```

### Usage: 

At this point a regular user should be able to checkout the project1 repo from the Git server.

```bash
tony@apha05:~$ mkdir ~/testing_shit/git_test
tony@apha05:~$ cd ~/testing_shit/git_test && git init
tony@apha05:~/testing_shit/git_test$ git remote add origin tony@svn:/home/git/project1
```

**Note:** Interestingly enough, an initial first commit has to be made onto the repo in order for any regular user to be able to push the repo, ie master branch. I received the following error when trying do so.

### Error:

```bash
tony@apha05:~/testing_shit/git_test$ git push origin master
tony@svn's password: 
error: src refspec master does not match any.
error: failed to push some refs to 'tony@svn:/home/git/project1'
```

### Fix:

```bash
tony@apha05:~/testing_shit/git_test$ git commit -m 'Initial'
[master (root-commit) 7bb7337] Initial
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.txt

tony@apha05:~/testing_shit/git_test$ git push origin master
tony@svn's password: 
Counting objects: 3, done.
Writing objects: 100% (3/3), 209 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To tony@svn:/home/git/project1
 * [new branch]      master -> master
```
