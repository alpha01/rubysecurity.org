---
categories:
  - awesome-applications
  - ansible
tags: ansible
layout: post
title: Annoying Ansible Gotcha
created: 1523680336
---

Ansible is by far my favorite Configuration Management tool, however it certainly has it's own unique quirks and annoyances. To start, I prefer the Ansible's YAML/Jinja approach instead of Puppet and Chef's own DSL custom configurations.

Today I ran into an interesting YAML parsing quirk. It turns out if you use colon ':' character inside a string anywhere in your playbooks, Ansible will fail to properly parse it.

Example playbook:

```yaml
---
- hosts: 127.0.0.1
  tasks:
    - lineinfile: dest=/etc/sudoers regexp='^testuser ALL=' state=present line="testuser ALL=(ALL) NOPASSWD: TEST_PROGRAM" state=present
```

### Error

When running the playbook, triggers the following error:

```bash
ERROR! Syntax Error while loading YAML.


The error appears to have been in '/etc/ansible/one_off_playbooks/example.yml': line 4, column 104, but may
be elsewhere in the file depending on the exact syntax problem.

The offending line appears to be:

  tasks:
    - lineinfile: dest=/etc/sudoers regexp='^testuser ALL=' state=present line="testuser ALL=(ALL) NOPASSWD: TEST_PROGRAM" state=present
                                                                                                       ^ here
```

### Fix

This is a known issue <a href="https://github.com/ansible/ansible/issues/1341" target="_blank">https://github.com/ansible/ansible/issues/1341</a> and the easiest work around for this, is to force the colon ':' character to be evaluated by the Jinja templating engine.

{% raw %}

```bash
{{':'}}
```

{% endraw %}

The hilarious part of this, is that it doesn't look like this stupid quirk is going to be fixed.
