# Lab Report 1

**From Group 6 [ IT 02-02 ]**

**Gede Reyki Astika   1202190052 || Ignatia Indreswari  1202190022**

------

### Scheme

Virtual Box Ubuntu Server IP : 192.168.0.100

LXC Debian Server 9 php5.6 IP : 10.0.3.102

LXC Ubuntu Server 18.04 php7.4 IP : 10.0.3.101

LXC Ubuntu Server 16.04 IP : 10.0.3.103

------

### Step by Step

1. ------

   Rename ubuntu_php5.6 to ubuntu_landing, also change the IP following the new scheme

   ![01_1_rename](D:\ITTelkom IT\SAS\Soal Prak 1\01_1_rename.PNG)

   Type

   ```
   sudo lxc-copy -R -n ubuntu_php5.6 -N ubuntu_landing
   ```

   This code is for copy than changing the container name from ubuntu_php5.6 to ubuntu_landing.

   After that, we have to check the state of every containers from containers  name, are the containers RUNNING or not and the IP of each containers. Using this code :

   ```
   sudo lxc-ls -f
   ```

   If the container name has changed to ubuntu_landing, we have to change the IP from before to a new one following the new scheme

   ![01_2_setstaticiplanding](D:\ITTelkom IT\SAS\Soal Prak 1\01_2_setstaticiplanding.PNG)

   We need to start the container first (if the container state is not running)

   ```
   sudo lxc-start -n ubuntu_landing
   ```

   Use the code below to enter the root of ubuntu_landing container

   ```
   sudo lxc-attach -n ubuntu_landing
   ```

   After we enter the root, we need to set the IP following the new scheme

   ```
   nano /etc/network/interfaces
   ```

   Then we will see this interface

   ![01_3_nanobefore](D:\ITTelkom IT\SAS\Soal Prak 1\01_3_nanobefore.PNG)

   As we see, the IP is still 10.0.3.102. We need to change it according to the new scheme

   ![01_4_nanoafter](D:\ITTelkom IT\SAS\Soal Prak 1\01_4_nanoafter.PNG)

   After changing the IP, we have to restart the container

   There are 2 ways to restart a container

   ```
   systemctl restart networking.service
   ```

   Function of `systemctl restart networking.service` is restart networking for the latest version of Ubuntu server.

   If this is not working, then we have to restart the container manually (Have to exit the container (exit the root))

   ```
   sudo lxc-stop -n ubuntu_landing
   sudo lxc-start -n ubuntu_landing
   ```

   After restarted then check if the IP changed or not

   ![01_4_iplanding1](D:\ITTelkom IT\SAS\Soal Prak 1\01_4_iplanding2.png)

   So, in this first step, we knew that if we want to change a container name, and if we want to change the IP, we need to go trough all this step. At first we can't rename the container name, we search at google, but we don't find the code for renaming it, but after we search one by one in the ubuntu, then we find the code.

2. ------

   Install LXC Debian 9 with the name ubuntu_php5.6

   ![02_1_installdebian](D:\ITTelkom IT\SAS\Soal Prak 1\02_1_installdebian.PNG)

   ```
   sudo lxc-create -n ubuntu_php5.6 -t download -- --dist debian --release stretch --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
   ```

   LXC Debian 9 use distro debian and release stretch.

   Then we have to check is it already installed

   ![02_2_checklxc](D:\ITTelkom IT\SAS\Soal Prak 1\02_2_checklxc.PNG)

3. ------

   Setup ubuntu_php5.6 nginx to it's domain http://lxc_php5.dev, then make index.html page that explains lxc names information

   We need to make sure that ubuntu_php5.6 is running, then enter the container. After that install the nginx

   ```
   sudo apt install nginx nginx-extras
   ```

   ![03_1_instalnginx](D:\ITTelkom IT\SAS\Soal Prak 1\03_1_instalnginx.PNG)

   Then we need to install net tools

   ```
   apt install nano net-tools curl
   ```

   ![03_2_installnettools](D:\ITTelkom IT\SAS\Soal Prak 1\03_2_installnettools.PNG)

   After that we need to set the IP

   ![03_3_configjaringanphp5](D:\ITTelkom IT\SAS\Soal Prak 1\03_3_configjaringanphp5.PNG)

   Then restart the container

   ![03_4_ifconfig](D:\ITTelkom IT\SAS\Soal Prak 1\03_4_ifconfig.PNG)

   As we see, after we restarted it, open the ifconfig to check whether the IP has been changed or not.

   After that we need to set the nginx

   ![03_5_settingnginx](D:\ITTelkom IT\SAS\Soal Prak 1\03_5_settingnginx.PNG)

   Enter the nano for configuration

   ```
   nano lxc_php5.6.dev
   ```

   ![03_6_setting](D:\ITTelkom IT\SAS\Soal Prak 1\03_6_setting.PNG)

   Nginx play the role as the bridge for the container and internet

   ![03_7_codedirectory](D:\ITTelkom IT\SAS\Soal Prak 1\03_7_codedirectory.PNG)

   Then we need to configure the hosts

   ```
   nano /etc/hosts
   ```

   ![03_8_directory](D:\ITTelkom IT\SAS\Soal Prak 1\03_8_directory.PNG)

   After we save it then we need to enter the directory for html

   ```
   cd /var/www/html
   mkdir lxc_php5.6
   cp index.nginx-debian.html lxc_php5.6/index.html
   cd lxc_php5.6
   ```

   ![03_9_codehtml](D:\ITTelkom IT\SAS\Soal Prak 1\03_9_codehtml.PNG)

   ```
   nano index.html
   ```

   ![03_10_htmlprogram](D:\ITTelkom IT\SAS\Soal Prak 1\03_10_htmlprogram.PNG)

   After that, we need to check if the code that we type in index.html, shown

   ```
   curl -i http://lxc_5.dev
   ```

   ![03_11_outputhtml](D:\ITTelkom IT\SAS\Soal Prak 1\03_11_outputhtml.PNG)

   Yes! finally

   We have to know the architecture and distro of Debian Operating System, we found out that we need to enter the `cd lxc_php5.6` before we code in nano index.html.

4. ------

   Setup ubuntu_landing nginx to it's domain http://lxc_landing.dev, then make index.html page that explains lxc names information, just like step number 3
   
   We just need to repeated it
   
   ![04_1_touchlandingdev](D:\ITTelkom IT\SAS\Soal Prak 1\04_1_touchlandingdev.PNG)
   
   ![04_2_nanolxclanding](D:\ITTelkom IT\SAS\Soal Prak 1\04_2_nanolxclanding.PNG)
   
   ![04_3_sitesenabled](D:\ITTelkom IT\SAS\Soal Prak 1\04_3_sitesenabled.PNG)
   
   ![04_4_nanoetchosts](D:\ITTelkom IT\SAS\Soal Prak 1\04_4_nanoetchosts.PNG)
   
   ![04_5_decvarmkdirlxclanding](D:\ITTelkom IT\SAS\Soal Prak 1\04_5_decvarmkdirlxclanding.PNG)
   
   Code in index.html just like this
   
   ![04_6_nanoindexhtml](D:\ITTelkom IT\SAS\Soal Prak 1\04_6_nanoindexhtml.PNG)
   
   Than we need to check with command curl
   
   ```
   curl -i http://lxc_landing.dev
   ```
   
   ![04_7_curlhttplxclanding](D:\ITTelkom IT\SAS\Soal Prak 1\04_7_curlhttplxclanding.PNG)
   
   
   
5. ------

   LXC ubuntu_landing need to be 'auto start' when we turn on vm. We need to enter super user to enter config page

   ```
   sudo su
   cd /var/lib/lxc/ubuntu_landing
   nano config
   ```

   ![05_01](D:\ITTelkom IT\SAS\Soal Prak 1\05_01.PNG)

   Add

   ```
   lxc.start.auto = 1
   ```

   ![05_02_nano](D:\ITTelkom IT\SAS\Soal Prak 1\05_02_nano.PNG)

   Exit the super user, then reboot it

   ```
   reboot
   ```

   Then check is the ubuntu_landing autostart has been running or not. If the autostart has been running, the 'AUTOSTART' will change from '0' to '1'.

   ```
   sudo lxc-ls -f
   ```

   ![05_03_autostart](D:\ITTelkom IT\SAS\Soal Prak 1\05_03_autostart.PNG)

   To run the autostart we need to enter the super user, so we can run the code to enter the config, we also need to reboot it.

6. ------

   Setup nginx for vm.local to config the proxy_pass

   ![06_1_setting hosts](D:\ITTelkom IT\SAS\Soal Prak 1\06_1_settinghosts.PNG)

   ```
   sudo nano /etc/hosts
   ```

   ![06_2_hosts](D:\ITTelkom IT\SAS\Soal Prak 1\06_2_hosts.PNG)

   After that we need to install nginx, then enter the directory of sites available, then enter the nano of vm.local

   ![06_3_setnginx](D:\ITTelkom IT\SAS\Soal Prak 1\06_3_setnginx.PNG)

   ```
   sudo apt install nginx nginx-extras
   cd /etc/nginx/sites-available
   sudo touch vm.local
   sudo nano vm.local
   ```

   Edit the nano vm.local

   ![06_4_setlocation](D:\ITTelkom IT\SAS\Soal Prak 1\06_4_setlocation.PNG)

   Then enter the directory of sites enabled, If nginx fails to start, run `sudo nginx -t` to find if there is anything wrong with your configuration file.  `sudo nginx -s reload` keeps the nginx server running as it reloads updated configuration files. If nginx notices a syntax error in any of the configuration files, the reload is aborted and the server keeps running based on old config files.

   ![06_5_settingnginx](D:\ITTelkom IT\SAS\Soal Prak 1\06_5_settingnginx.PNG)

   ```
   cd ../sites-enabled
   sudo nginx -t
   sudo nginx -s reload
   ```

   We check is the html code appear as it should be or not, using command curl, curl is a command line tool to transfer data to or form a server, using any of the supported protocols like HTTP, FTP, etc. Curl can transfer multiple file at once.

   ```
   curl -i http://vm.local/blog
   ```

   ![06_6_vmlocalblogphp7](D:\ITTelkom IT\SAS\Soal Prak 1\06_6_vmlocalblogphp7.PNG)

   ```
   curl -i http://vm.local/app
   ```

   ![06_7_vmlocalappphp5](D:\ITTelkom IT\SAS\Soal Prak 1\06_7_vmlocalappphp5.PNG)

   ```
   curl -i http://vm.local/
   ```

   ![06_8_vmlocallanding](D:\ITTelkom IT\SAS\Soal Prak 1\06_8_vmlocallanding.PNG)

7. ------

   Access the vm.local in web browser, the output should be like this

   ```
   http://vm.local/
   ```

   ![07_1_landingpage](D:\ITTelkom IT\SAS\Soal Prak 1\07_1_landingpage.PNG)

   ```
   http://vm.local/app
   ```

   ![07_2_php5](D:\ITTelkom IT\SAS\Soal Prak 1\07_2_php5.PNG)

   ```
   http://vm.local/blog
   ```

   ![07_3_php7](D:\ITTelkom IT\SAS\Soal Prak 1\07_3_php7.PNG)



------

### Analysis

1. Why we can't use ubuntu 16.04 for php5.6 needs, and we have to change the OS to debian 9?
2. Why we have to use LXC Virtualization for website scheme that will be developed?
3. What is a proxy server? Why we think of vm.local as a proxy server?



------

### Answer of the Analysis

1. Because php5.6 is already at the end of service which doesn't support the latest updates, so we use Debian 9, which is still operating
2. Because LXC Virtualization is easy to use (lightweight), it can happen because it shares the host system's kernel. Users can easily create and manage system or application containers with a powerful API and simple tools.
3. Proxy server is a gateway provider between users and the internet. Proxy server is a server, referred to as a bridge between end-users and the web pages they visit online. A proxy server is basically a computer on the internet that has an IP address of its own. We think vm.local is the same as proxy server if the vm.local has the correct IP so that it can communicate with the local machines and the internet. In order to use the VM as a proxy server it just has to be connected to the correct networks.

