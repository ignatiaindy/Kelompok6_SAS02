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



1. Rename ubuntu_php5.6 to ubuntu_landing, also change the IP following the new scheme

 ![01_1_rename](https://user-images.githubusercontent.com/77930572/138566072-9093e5ae-8937-469e-9ba2-51a83edb3ad0.PNG)

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

   ![01_2_setstaticiplanding](https://user-images.githubusercontent.com/77930572/138566090-2572b992-f3e0-4c2e-8ff0-e3c3c464cb8d.PNG)

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

   ![01_3_nanobefore](https://user-images.githubusercontent.com/77930572/138566113-921df831-c686-4a17-aded-0a9b54eb4d63.PNG)

   As we see, the IP is still 10.0.3.102. We need to change it according to the new scheme

   ![01_4_nanoafter](https://user-images.githubusercontent.com/77930572/138566171-fd9258de-b276-4ea1-81e8-a4a09ccdd80c.PNG)

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

   ![01_4_iplanding2](https://user-images.githubusercontent.com/77930572/138566238-de7af1ae-ee99-4b1b-bd4a-bbb474155619.png)

   So, in this first step, we knew that if we want to change a container name, and if we want to change the IP, we need to go trough all this step. At first we can't rename the container name, we search at google, but we don't find the code for renaming it, but after we search one by one in the ubuntu, then we find the code.

   

2. Install LXC Debian 9 with the name ubuntu_php5.6

   ![02_1_installdebian](https://user-images.githubusercontent.com/77930572/138566260-944c8486-eb1d-41cf-ad25-579e4f554fe7.PNG)

   ```
   sudo lxc-create -n ubuntu_php5.6 -t download -- --dist debian --release stretch --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
   ```

   LXC Debian 9 use distro debian and release stretch.

   Then we have to check is it already installed

   ![02_2_checklxc](https://user-images.githubusercontent.com/77930572/138566279-8023ba6d-9575-4a1f-92e9-6227bdf3d388.PNG)

   

3. Setup ubuntu_php5.6 nginx to it's domain `http://lxc_php5.dev`, then make index.html page that explains lxc names information

   We need to make sure that ubuntu_php5.6 is running, then enter the container. After that install the nginx

   ```
   sudo apt install nginx nginx-extras
   ```

   ![03_1_instalnginx](https://user-images.githubusercontent.com/77930572/138566303-de56012d-3dcc-4aad-9dbe-6ad2c9df011e.PNG)

   Then we need to install net tools

   ```
   apt install nano net-tools curl
   ```

   ![03_2_installnettools](https://user-images.githubusercontent.com/77930572/138566313-1f79c4c9-e185-4e89-b2b2-f67aedfdb9a9.PNG)

   After that we need to set the IP

   ![03_3_configjaringanphp5](https://user-images.githubusercontent.com/77930572/138566336-52bb7ae8-91b1-4d4d-a6d0-17fcfe4d9396.PNG)

   Then restart the container

   ![03_4_ifconfig](https://user-images.githubusercontent.com/77930572/138566345-2c375358-e74b-4039-8daf-618e6b74679b.PNG)

   As we see, after we restarted it, open the ifconfig to check whether the IP has been changed or not.

   After that we need to set the nginx

   ![03_5_settingnginx](https://user-images.githubusercontent.com/77930572/138566358-3522ae4b-7519-4f30-98fc-3da892208947.PNG)

   Enter the nano for configuration

   ```
   nano lxc_php5.6.dev
   ```

   ![03_6_setting](https://user-images.githubusercontent.com/77930572/138566367-8a160e4b-7869-43e3-b879-b0647964d33b.PNG)

   Nginx play the role as the bridge for the container and internet

   ![03_7_codedirectory](https://user-images.githubusercontent.com/77930572/138566396-757b47ad-9fb2-4da5-8b70-5de3371ec0d9.PNG)

   Then we need to configure the hosts

   ```
   nano /etc/hosts
   ```

   ![03_8_directory](https://user-images.githubusercontent.com/77930572/138566404-74828716-3f19-47b3-933f-ff46f1039324.PNG)

   After we save it then we need to enter the directory for html

   ```
   cd /var/www/html
   mkdir lxc_php5.6
   cp index.nginx-debian.html lxc_php5.6/index.html
   cd lxc_php5.6
   ```

   ![03_9_codehtml](https://user-images.githubusercontent.com/77930572/138566416-eef3e36f-fab7-435f-b33d-c36c51cf057d.PNG)

   ```
   nano index.html
   ```

   ![03_10_htmlprogram](https://user-images.githubusercontent.com/77930572/138566425-4cc0c60d-1c37-4ac2-9624-466d379c5918.PNG)

   After that, we need to check if the code that we type in index.html, shown

   ```
   curl -i http://lxc_5.dev
   ```

   ![03_10_htmlprogram](https://user-images.githubusercontent.com/77930572/138566425-4cc0c60d-1c37-4ac2-9624-466d379c5918.PNG)

   Yes! finally

   We have to know the architecture and distro of Debian Operating System, we found out that we need to enter the `cd lxc_php5.6` before we code in nano index.html.

   

4. Setup ubuntu_landing nginx to it's domain `http://lxc_landing.dev`, then make index.html page that explains lxc names information, just like step number 3

   We just need to repeated it
   
   ![04_1_touchlandingdev](https://user-images.githubusercontent.com/77930572/138566432-187a7812-a31b-45a0-9047-9641ae8b2eba.PNG)
   
   ![04_2_nanolxclanding](https://user-images.githubusercontent.com/77930572/138566443-1bd2797c-62cd-47ae-9aa4-c87e1d54b11e.PNG)
   
   ![04_3_sitesenabled](https://user-images.githubusercontent.com/77930572/138566446-103da34f-40cb-44f8-a2e5-86dcbee70e5e.PNG)
   
   ![04_4_nanoetchosts](https://user-images.githubusercontent.com/77930572/138566450-bd69557e-a7ea-4b68-8279-fb7441b7d840.PNG)
   
   ![04_5_decvarmkdirlxclanding](https://user-images.githubusercontent.com/77930572/138566453-e60f788f-44ff-4d4c-89e5-fccc5d0bdd9d.PNG)
   
   Code in index.html just like this
   
   ![04_6_nanoindexhtml](https://user-images.githubusercontent.com/77930572/138566458-13fc357a-70a5-40db-b6b1-9717bd5076f6.PNG)
   
   Than we need to check with command curl
   
   ```
   curl -i http://lxc_landing.dev
   ```
   
   ![04_7_curlhttplxclanding](https://user-images.githubusercontent.com/77930572/138566461-a9dc17ef-b4fe-4b26-8947-7357a002b5d9.PNG)
   
   
   
   
   
5. LXC ubuntu_landing need to be 'auto start' when we turn on vm. We need to enter super user to enter config page

   ```
   sudo su
   cd /var/lib/lxc/ubuntu_landing
   nano config
   ```

   ![05_01](https://user-images.githubusercontent.com/77930572/138566465-d6a9dbb7-9c30-4e9c-81eb-e97344813656.PNG)

   Add

   ```
   lxc.start.auto = 1
   ```

   ![05_02_nano](https://user-images.githubusercontent.com/77930572/138566469-99ef1c24-c709-419b-905f-61bce1ccd62e.PNG)

   Exit the super user, then reboot it

   ```
   reboot
   ```

   Then check is the ubuntu_landing autostart has been running or not. If the autostart has been running, the 'AUTOSTART' will change from '0' to '1'.

   ```
   sudo lxc-ls -f
   ```

   ![05_03_autostart](https://user-images.githubusercontent.com/77930572/138566474-163dd263-1abc-4c01-b960-04be467a77ef.PNG)

   To run the autostart we need to enter the super user, so we can run the code to enter the config, we also need to reboot it.

   

6. Setup nginx for vm.local to config the proxy_pass

   ![06_1_settinghosts](https://user-images.githubusercontent.com/77930572/138566485-2bf58795-0aa4-44b0-82b8-51a9406089c4.PNG)

   ```
   sudo nano /etc/hosts
   ```

   ![06_2_hosts](https://user-images.githubusercontent.com/77930572/138566486-2dabe77f-3f4e-47f3-9166-5c1861a70244.PNG)

   After that we need to install nginx, then enter the directory of sites available, then enter the nano of vm.local

   ![06_3_setnginx](https://user-images.githubusercontent.com/77930572/138566487-c4a7e268-14c6-445f-bb52-68307d1507d1.PNG)

   ```
   sudo apt install nginx nginx-extras
   cd /etc/nginx/sites-available
   sudo touch vm.local
   sudo nano vm.local
   ```

   Edit the nano vm.local

   ![06_4_setlocation](https://user-images.githubusercontent.com/77930572/138566489-e4441f90-5859-4b73-ac70-134ea0eed5be.PNG)

   Then enter the directory of sites enabled, If nginx fails to start, run `sudo nginx -t` to find if there is anything wrong with your configuration file.  `sudo nginx -s reload` keeps the nginx server running as it reloads updated configuration files. If nginx notices a syntax error in any of the configuration files, the reload is aborted and the server keeps running based on old config files.

   ![06_5_settingnginx](https://user-images.githubusercontent.com/77930572/138566490-f12cec40-760d-48df-a315-88bd8e2c3de5.PNG)

   ```
   cd ../sites-enabled
   sudo nginx -t
   sudo nginx -s reload
   ```

   We check is the html code appear as it should be or not, using command curl, curl is a command line tool to transfer data to or form a server, using any of the supported protocols like HTTP, FTP, etc. Curl can transfer multiple file at once.

   ```
   curl -i http://vm.local/blog
   ```

   ![06_6_vmlocalblogphp7](https://user-images.githubusercontent.com/77930572/138566491-2829ff80-e8c6-40f3-8c64-f1e3ca1a578f.PNG)

   ```
   curl -i http://vm.local/app
   ```

   ![06_7_vmlocalappphp5](https://user-images.githubusercontent.com/77930572/138566492-0119e6d0-76f5-45c7-a331-88cbd9a145f1.PNG)

   ```
   curl -i http://vm.local/
   ```

   ![06_8_vmlocallanding](https://user-images.githubusercontent.com/77930572/138566493-a2a1d4b7-3532-4bd5-aec9-4d0c98f0a4b2.PNG)

   

7. Access the vm.local in web browser, the output should be like this

   ```
   http://vm.local/
   ```

   ![07_1_landingpage](https://user-images.githubusercontent.com/77930572/138566482-8fa7fbf3-05f7-48be-9b06-f5bf7ff7de82.PNG)

   ```
   http://vm.local/app
   ```

   ![07_2_php5](https://user-images.githubusercontent.com/77930572/138566483-fb094b1a-dcbe-4afe-ae4f-b4dad01dc860.PNG)

   ```
   http://vm.local/blog
   ```

   ![07_3_php7](https://user-images.githubusercontent.com/77930572/138566484-7d3c8dec-1e09-4eaa-8128-77398587ca9a.PNG)



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

