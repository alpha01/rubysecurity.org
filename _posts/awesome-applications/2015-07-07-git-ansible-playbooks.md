---
categories:
  - awesome-applications
  - ansible
tags:
  - ansible
  - git
layout: post
title: Git Ansible Playbooks
created: 1436239082
---

One of the reasons I love Ansible over any other config management tool is because of its simplistic design and ease of use. It literally took me less than 15 minutes to write a set of playbooks to manage my local git server.

git_server_setup.yml - configures base server git repository configuration. 


```yaml
---
- hosts: git
  tasks:
  - name: Installing git package
    yum: name=git state=latest

  - name: Creating developers group
    group: name=developers state=present

  - name: Creating git user
    user: name=git group=developers home=/home/git shell=/sbin/nologin

  - name: Updating /home/git permissions
    file: path=/home/git mode=2770
```


`create_git_user.yml` - creates local system git user accounts.

{% raw %}
```yaml
---
- hosts: git
  tasks:

  - name: Creating new git user
    user: name={{ user_name }} password={{ user_password }} home=/home/git shell=/usr/bin/git-shell group=developers

  vars_prompt:
  - name: "user_name"
    prompt: "Enter a new git username"
    private: no

  - name: "user_password"
    prompt: "Enter a password for the new git user"
    private: yes
    encrypt: "sha512_crypt"
    confirm: yes
    salt_size: 7
```
{% endraw %}

`create_git_repo.yml` - creates an empty bare git repository.

{% raw %}
```yaml
---
- hosts: git
  vars:
    repo_name: www.alpha01.org

  tasks:
  - file: path=/home/git/{{ repo_name }} state=directory mode=2770

  - name: Creating {{ repo_name }} git repository
    command: git init --bare --shared /home/git/{{ repo_name }}

  - name: Updating repo permissions
    file: path=/home/git/{{ repo_name }} recurse=yes owner=git
```
{% endraw %}
