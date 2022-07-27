---
categories:
  - programming
  - ruby
tags:
  - ruby
  - vagrant
layout: post
title: 'Working with Ruby obfuscated code: Finding all classes available in a module'
created: 1495593811
---

As a follow up to my <a href="http://www.rubyninja.org/2017/05/21/hashicorp-rocks/" target="_blank">HashiCorp Rocks!</a> blog post. Up until now, I've never directly worked with any obfuscated code.  HashiCorp obfuscates their VMware <a href="https://www.vagrantup.com/vmware/" target="_blank">Fusion</a> and <a href="https://www.vagrantup.com/vmware/" target="_blank">Workstation</a> commercial Vagrant plugins.

Like Vagrant, the plugins themselves are written in Ruby.

```bash
alpha03:lib tony$ file vagrant-vmware-fusion.rb
vagrant-vmware-fusion.rb: ASCII text, with very long lines, with CRLF, LF line terminators
```

However, if you try to read the source all you'll see is a bunch of encoded text. Since <a href="https://github.com/fgrehm/vagrant-notify" target="_blank">my Vagrant plugin</a> has some <a href="https://github.com/fgrehm/vagrant-notify/blob/master/lib/vagrant-notify/plugin.rb#L41-L66" target="_blank">functionality</a> that only works after a certain action gets executed by the proprietary plugins. This is why I needed to know the exact name of that particular action (class name) exactly how it's defined inside the VMware <a href="https://www.vagrantup.com/vmware/" target="_blank">Fusion</a> and <a href="https://www.vagrantup.com/vmware/" target="_blank">Workstation</a> plugins. This was a serious problem because I can't read their source code! 

Luckily, this wasn't as difficult as it seems. Finding the classes (or methods but in the case of mine I didn't need too) available in Ruby is fairly simple process. To my luck somebody had already asked and answered this question in <a href="https://stackoverflow.com/questions/833125/find-classes-available-in-a-module#answer-833179" target="_blank">StackOverflow</a>. 

In my case, first step was needing to know the name of the actual module itself. I found the easiest way to get the name of the module that's obfuscated, is to intentionally have it spit out an exception. In doing that, I found that the module names whose namespace I'll be searching were `HashiCorp::VagrantVMwarefusion` and `HashiCorp::VagrantVMwareworkstation`.

Once I knew the modules's name, I  was able to use Ruby to view what additional modules I have within the particular module namespace. I was able to accomplish that using the following

```ruby 
t = HashiCorp::VagrantVMwareworkstation.constants.select {|c| 
HashiCorp::VagrantVMwareworkstation.const_get(c).is_a? Module
}
puts t
```

The above sample code spit out a bunch of modules inside `HashiCorp::VagrantVMwareworkstation`, but since I know the Vagrant plugin API and it's coding standards/practices. I was able to verify that the module I'm searching for is `HashiCorp::VagrantVMwareworkstation::Action`. Once again, looking at Plugin API and <a href="https://github.com/mitchellh/vagrant/blob/master/plugins/providers/hyperv/action/suspend_vm.rb#L4" target="_blank">other</a> <a href="https://github.com/mitchellh/vagrant/blob/master/plugins/providers/virtualbox/action/suspend.rb#L4" target="_blank">examples</a>, I knew that this is where the class is I'm looking is stored in. So I used the following to get the corresponding class name within `HashiCorp::VagrantVMwareworkstation::Action`.

```ruby
p = HashiCorp::VagrantVMwareworkstation::Action.constants.select { |c|
HashiCorp::VagrantVMwareworkstation::Action.const_get(c).is_a? Class
}
 puts p
```

I repeated the above tests for `HashiCorp::VagrantVMwarefusion` and I was also able to find the corresponding class name that it's defined inside the obfuscated Ruby code.

In the end I was able to get the classes _**<a href="https://github.com/fgrehm/vagrant-notify/blob/master/lib/vagrant-notify/plugin.rb#L56" target="_blank">HashiCorp::VagrantVMwareworkstation::Action::Suspend</a>**_ and _**<a href="https://github.com/fgrehm/vagrant-notify/blob/master/lib/vagrant-notify/plugin.rb#L52" target="_blank">HashiCorp::VagrantVMwarefusion::Action::Suspend</a>**_, and everything worked as expected.
