# Report Module 3

**From Group 6 [ IT 02-02 ]**

**Gede Reyki Astika   1202190052 || Ignatia Indreswari  1202190022**

------

## Step by Step

------

Make 'dev.vm.local' subdomain using ansible

Register 'dev.vm.local' subdomain to DNS

Go to

```
cd ~/ansible/modul2-ansible
cd roles/php/tasks
cd main.yml
```

Here we added 'bind9 and dnsutils'

![01_1_install_php_1](assets/01_1_install_php_1.PNG)

After that we go to

```
cd roles/lv/tasks
cd main.yml
```

Here we added tasks such as  'Creates directory, Copy conf.local, Copy vm.local, Copy 0.168.192, Copy resolv.conf, and Copy named.conf.options'

![01_8_tasks_mainyml](assets/01_8_tasks_mainyml.PNG)

![01_9_tasks_mainyml](assets/01_9_tasks_mainyml.PNG)

After done, we go to

```
cd roles/lv/templates
nano 0.168.192.in-addr.arpa
nano named.conf.local
nano named.conf.options
nano  resolv.conf
nano vm.local
```

![01_2_inaddrarpa](assets/01_2_inaddrarpa.PNG)

![01_3_named_conf_local](assets/01_3_named_conf_local.PNG)

![01_4_named_conf_options](assets/01_4_named_conf_options.PNG)

![01_5_resolv_conf](assets/01_5_resolv_conf.PNG)

![01_6_vm_local](assets/01_6_vm_local.PNG)

Then we go to handlers to add restart bind

```
cd roles/lv/handlers
nano main.yml
```

![01_7_lv_handlers_mainyml](assets/01_7_lv_handlers_mainyml.PNG)

We run ansible playbook

```
ansible-playbook -i hosts install-laravel.yml -k
```

![01_10_ansible_playbook_laravel](assets/01_10_ansible_playbook_laravel.PNG)

![01_11_ansible_playbook_laravel](assets/01_11_ansible_playbook_laravel.PNG)

Success!

After that we add 'dev.vm.local' in

```
sudo nano /etc/hosts
```

![01_12_etc_hosts_vm](assets/01_12_etc_hosts_vm.PNG)

And then we go to

![01_13_varwwwhtml_devlanding](assets/01_13_varwwwhtml_devlanding.PNG)

enter nano vm.local, then add 'www IN CNAME vm.local.'

![01_14_nano_vmlocal](assets/01_14_nano_vmlocal.PNG)

After that we must restart

```
sudo /etc/init.d/named restart
host -t CNAME www.vm.local
```

We succeed :)

![02_2_laravel_devvmlocal](assets/02_2_laravel_devvmlocal.PNG)

![02_blog_devvmlocal](assets/02_blog_devvmlocal.PNG)

