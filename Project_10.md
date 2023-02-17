# (STEP 20) PROJECT 10: LOAD BALANCER SOLUTION WITH NGINX AND SSL/TLS


By now we have learned what Load Balancing is used for and have configured an LB solution using Apache, but a DevOps engineer must be a versatile professional and know different alternative solutions for the same problem. That is why, in this project we will configure an *Nginx* Load Balancer solution.

It is also extremely important to ensure that connections to your Web solutions are secure and information is *encrypted in transit* – we will also cover connection over secured HTTP (HTTPS protocol), its purpose and what is required to implement it.

When data is moving between a client (browser) and a Web Server over the Internet – it passes through multiple network devices and, if the data is not encrypted, it can be relatively easy intercepted by someone who has access to the intermediate equipment. This kind of information security threat is called *Man-In-The-Middle (MIMT) attack.*

This threat is real – users that share sensitive information (bank details, social media access credentials, etc.) via non-secured channels, risk their data to be compromised and used by *cybercriminals.*

*SSL and its newer version, TSL* – is a security technology that protects connection from MITM attacks by creating an encrypted session between browser and Web server. Here we will refer this family of cryptographic protocols as SSL/TLS – even though SSL was replaced by TLS, the term is still being widely used.

SSL/TLS uses *digital certificates* to identify and validate a Website. A browser reads the certificate issued by a *Certificate Authority (CA)* to make sure that the website is registered in the CA so it can be trusted to establish a secured connection.

There are different types of SSL/TLS certificates – you can learn more about them below. 

https://blog.hubspot.com/marketing/what-is-ssl

https://www.youtube.com/watch?v=T4Df5_cojAs

https://www.youtube.com/watch?v=SJJmoDZ3il8



In this project you will register your website with *LetsEnrcypt Certificate Authority*, to automate certificate issuance you will use a shell client recommended by LetsEncrypt – *cetrbot.*

## Task

This project consists of two parts:

1. Configure Nginx as a Load Balancer

2. Register a new domain name and configure secured connection using SSL/TLS certificates

   Your target architecture will look like this:


   ![Target Architecture](./Images/Target%20Architecture.png)



## REGISTER A NEW DOMAIN NAME AND CONFIGURE SECURED CONNECTION USING SSL/TLS CERTIFICATES


  - Let us make necessary configurations to make connections to our Tooling Web Solution secured.
  
  - In order to get a valid SSL certificate – you need to register a new domain name, you can do it using any Domain name registrar – a company that manages reservation of domain names. The most popular ones are: *Godaddy.com, Domain.com, Bluehost.com., Ionos*


  - For the project we were going to user freenom.com , but it is currently down. 

    https://my.freenom.com/domains.php


     The screen below will be displayed.

    ![my.feenom.com](./Images/my.freenom.com.png)

    Register a new domain name with any registrar of your choice in any domain zone (e.g. .com, .net, .org, .edu, .info, .xyz or any other)


  - We will therefore use Ionos which is a paid option. Go to the web browser and paste the link below.

    https://www.ionos.com/


 - Seach for availabity for *toolingysf*. It will tell you the available domain. Click Check.

   ![Toolingysf](./Images/Toolingysf.png)


 - The screen below will be displayed. Click on Add to Cart for the 1 year subscription.


   ![Toolingysf_1](./Images/Toolingysf_1.png)


- Click on Continue.

  ![Toolingysf_2](./Images/Toolingysf_2.png)

- Click Domain Only

  ![Toolingysf_3](./Images/Toolingysf_3.png)

- Click Continue


  ![Toolingysf_4](./Images/Toolingysf_4.png)


- Click Continue to checkout.

  ![Toolingysf_5](./Images/Toolingysf_5.png)

- Fill the Create New Account form and billing information.


  ![Toolingysf_6](./Images/Toolingysf_6.png)


- After the order has completed, the screen below will be displayed. Check your email and complete the steps in the email.

  ![Toolingysf_7](./Images/Toolingysf_7.png)


- Finally you will receive an email showing that the Domain has been successfully registered.


  ![Toolingysf_8](./Images/Toolingysf_8.png)

- Login to Ionos with your credentials that you were provided before.

  ![Ionos login](./Images/Ionos%20login.png)

- You will received another email to confirm your email.

-  on Doamin and SSL.

   ![Toolingysf_9](./Images/Toolingysf_9.png)


- The Domain will be displayed at the bottom.

  ![Toolingysf_10](./Images/Toolingysf_10.png)


- Click on the Toolingysf and it will show that the Domain is Active.


  ![Toolingysf_11](./Images/Toolingysf_11.png)


- Go to AWS page and search for Route 53 under Services.

  ![Search Route 53](./Images/Search%20Route%2053.png)



- Click Create hosted zone.

  ![Create hosted zone](./Images/Create%20hosted%20zone.png)


- Fill the Domain Name , Tye as Public hosted zone and then click Create hosted zone.

  ![Creeate hosted zone-1](./Images/Create%20hosted%20zone_1.png)


- It will show that toolingysf.com was successfully created.

  ![Create hosted zone_2](./Images/Create%20hosted%20zone_2.png)

 - Go to Menu > Domains & SSL > Under Actions , click on Settings icon > Name Server.

   ![Name Server](./Images/Name%20Server.png)



- The screen below will be displayed.


  ![Name Server_1](./Images/Name%20Server_1.png)


- Click on Use custom name servers

  ![Name Servers_2](./Images/Name%20Server_2.png)

- Copy and paste each of the Name Servers from AWS Route 53 page to each line on Ionos page.


  ![Name Servers_3](./Images/Name%20Server_3.png)


- After that click Save.

  ![Name Servers_4](./Images/Name%20Server_4.png)


- It will show that Name Servers have been successfully changed.

  ![Name Servers_5](./Images/Name%20Server_5.png)


   This shows that our Route 53 and the toolingysf domain are connected.



## CONFIGURE NGINX AS A LOAD BALANCER

- You can either uninstall Apache from the existing Load Balancer server, or create a fresh installation of Linux for Nginx.

1. Create an EC2 VM based on Ubuntu Server 20.04 LTS and name it Nginx LB (do not forget to open TCP port 80 for HTTP connections, also open TCP port 443 – this port is used for secured HTTPS connections)


   ![Nginx LB](./Images/Nginx%20LB.png)



- To open Port TCP for Port 80 and TCP for Port 443 click on Security > Security groups > Edit Inbound Security. Add the two rules and Save rules.

  ![Edit Inbound Rules](./Images/Edit%20Inbound%20Rules.png)


- Copy the public IP address for Nginx LB; You will use it to create a record.

  18.217.14.224


- Go back to Route 53 as we now need to create a record for our domain name. click Create records.

  ![Create record](./Images/Create%20record.png)

- Click Create records.


  ![Create record_1](./Images/Create%20record_1.png)


- Paste the Public IP address for Nginx LB under values. Leave everything as defaulted and then click Create records.

  ![Create records_2](./Images/Create%20record_2.png)


- The screen below will show that a record has been created successfully. The first record at the top of the list shows the record that we have just created.


  ![Create record_3](./Images/Create%20record_3.png)


-  We will create another record for 'www'. Click Create records.


   ![Create record_3](./Images/Create%20record_2.png)

- Put "www" under Record name and the same Public IP address for the Nginx LB under Value, and then click create records.


  ![Create record_4](./Images/Create%20record_4.png)


-  The screen below will be diplayed showing that the record was successfully created. It will show as the forth record on the list.


   ![Create record_5](./Images/Create%20record_5.png)


- Now the load balancer with Route 53 and our Domain Name are now connected together.


- Connect the Load Balancer (Nginx) through the terminal.

  ![Nginx Login](./Images/Nginx%20LB%20Login.png)



2. Install and configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers.

  - Update the instance and Install Nginx. Click Y when prompted while installing Nginx.


  
    `sudo apt update`

    `sudo apt install nginx`
  

     ![Update Ubuntu](./Images/Update%20Ubuntu.png)


    ![Install Nginx](./Images/Install%20Nginx.png)


    Enable Nginx.

    `sudo systemctl enable nginx`

    ![Enable Nginx](./Images/Enable%20Nginx.png)

- Start Nginx.

  `sudo systemctl start nginx`

  ![Start Nginx](./Images/Start%20Nginx.png)


- Check the status of nginx to ensure that it is running.

  `sudo systemctl status nginx`


  ![Status nginx](./Images/Status%20nginx.png)


- Open the default nginx configuration file

  `sudo vi /etc/nginx/nginx.conf`


  ![Default nginx config](./Images/Default%20nginx%20config.png)



3. Insert following configuration into http section. Note that the 2 IP addresses shown below are Private IP addresses for Web Server 1 and Web Server 2. The servername includes the domain and the web site (toolingysf.com www.toolingysf.com).

  ```
upstream web {
    server 18.217.65.38;
    server 18.191.61.143;
  }

server {
        listen 80;

        server_name toolingysf.com www.toolingysf.com;

        location / {
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://web;
      }
  }

  ```

 - To perform the instructions above, follow the instructions below;

   Run;

   `sudo vi /etc/nginx/nginx.conf`

- The screen below will be displayed.

  ![Nginx Config](./Images/Nginx%20Config.png)

- Scroll down to http section;

- Copy and paste the section below under http. The two IP addresses are Private IP addresses for the two Web servers.


  ![Nginx Config_1](./Images/Nginx%20Config_1.png)


-  Delete all that was between the section of the file that you have added all the way to as shown below;

```
##
# Basic Settings
## 
```


![Nginx Config_2](./Images/Nginx%20Config_2.png)

Save and exit by :wq!:

-  Reload nginx.

   `sudo systemctl reload nginx`


4. We need to create a configuration for the reverse process sentence. 


-  Run the command below in Nginx LB server. We need to create a config file in nginx LB server.

   `sudo nano /etc/nginx/sites-available/load_balancer.conf`

   The screen below will be displayed.

   ![Config](./Images/Config.png)

- Input the web server information. You will need to update it. Under upstream web, those are the two Private IP addresses for the web servers. I have used Project 7 web server 1 and Project 7 web 2.


```
upstream web {
    server 1 Private IP address;
    server 2 Private IP address;
  }

server {
        listen 80;

        server_name www.domain.com;

        location / {
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://web;
      }
  }
```

- Our Domain  Name is : toolingysf.com



-  Below is the updated file. Copy and paste it to the nginx LB terminal.

  ```
upstream web {
    server 172.31.14.230;
    server 172.31.9.217;
  }

server {
        listen 80;

        server_name toolingysf.com www.toolingysf.com;

        location / {
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://web;
      }
  }
   ```

  ![Config_2](./Images/Config_2.png)


-  Control O to write.

   ![Config_3](./Images/Config_3.png)


-  Press Enter.

   ![Config_4](./Images/Config_4.png)


-  Control X to exit.

   ![Config_5](./Images/Config_5.png)


-  We need to remove the default site so that the reverse process will be redirecting to the new configuration file. 


   `sudo rm -f /etc/nginx/sites-enabled/default`

   ![Remove default site](./Images/Remove%20default%20site.png)

 -  Check if Nginx is susccessfully configured. It shows that the configuration of syntax is successful.

    `sudo nginx -t`


    ![Nginx Configuration_Success](./Images/Nginx%20Configuration_Success.png)


 -  Go back to nginx sites enabled by running the command below to change directory.

    `cd /etc/nginx/sites-enabled/`


 - ![Nginx sites enabled](./Images/Nginx%20sites%20enabled.png)


 - Run `ls`. It should be empty. Our screenshot below is not correct because I took it after I linked the account and that is why we see the load_balancer.config.

   ![ls](./Images/ls.png)


- We now need to link the load balancer config file that we just created in site available to site enabled.


   `sudo ln -s ../sites-available/load_balancer.conf .`

 - Then run `ls`

   ![Link load balancer config file](./Images/Link%20load%20balancer%20config%20file.png)


 - Run `ll` and it will show you again that it is linked.

   ![Confirm lb link](./Images/Confirm%20lb%20link.png)


 - Reload nginx.

   `sudo systemctl reload nginx`

   ![Reload nginx](./Images/Reload%20nginx.png)

 - Go back to the two web server and start httpd using the command below. check the status to ensure that they are up and running.

   `sudo systemctl start httpd`

   `sudo systemctl status httpd`

   ![Web server 1 status](./Images/Web%20server%201%20status.png)

   ![Web server 2 status](./Images/Web%20server%202%20status.png)



 - Lets go to our domain (toolingysf.com/login.php) it should provide the screen below. 


   ![Propitix Toolong Login](./Images/Propitix%20Tooling%20Login.png)


 - It shows that it is not secured. 

   ![Not Secure](./Images/Not%20Secure.png)


5. We need to secure the web page using Let's encrypt.

   -  Go back to Nginx terminal. Make sure you are in the original directory. You can use cd .. to go back to the original directory.

    - Install certbot

      `sudo apt install certbot -y`

      ![Install certbot](./Images/Install%20certbot.png)


   - Install a module that our certbot that cerbot will be using. It's like a dependency.

      Run;

     `sudo apt install python3-certbot nginx -y`


     ![Install python3](./Images/Install%20python3.png)



    - Check the systax and reload nginx.

      `sudo nginx -t && sudo nginx -s reload`


      ![Syntax ok](./Images/Syntax%20ok.png)

    -  To confirm that nginx is reloaded , you can check the status that it is running by the command below.


       `sudo systemctl status nginx`


       ![Nginx status](./Images/Nginx%20status.png)


     - Create a certificate for our domain to make it secured as it is unsecured currently.

     - Run the command below in the nginx terminal.

       `sudo certbot --nginx -d toolingysf.com -d www.toolingysf.com`

    - I got an error that the requested plugin was not installed. Run the command below to to fix the error by installing the nginx plugin.

    - If you have not run the command below then run it to ensure everything is upgraded.

      `sudo apt upgrade`

      `sudo apt-get install python3-certbot-nginx`


      ![Install nginx plugin](./Images/Install%20nginx%20plugin.png)

    - Rerun the command.

      `sudo certbot --nginx -d toolingysf.com -d www.toolingysf.com`



    - You will be prompted to enter email used for urgent renewals and security notices. Press Enter once you have input the email address.

      ![Email](./Images/Email.png)


      ![Create certificates](./Images/Create%20certificates.png)

     - Press A to Agree to the terms.

       ![Create certificates](./Images/Create%20certificates.png)



    - Choose Y if you want to share your email address or N if you dont.

      ![Create certificates_1](./Images/Create%20certificates_1.png)

    - The screen below will be displayed.  

      ![Create certificates_4](./Images/Create%20certificates_4.png)


    - Select 1 to attempt to reinstall this existing certificate. This prompting is displayed beacuse I already attempted to install the certificate and ran into an error due to a mistake I made, otherwise you will be prompted to make a selection to install the certificate.

      ![Create certificates_5](./Images/Create%20certificates_5.png)

    - You will be prompted to choose whether to redirect HTTP traffic to HTTPS, removing HTTP access.

      ![Create certificates_6](./Images/Create%20certificates_6.png)


   - Select option 2. the screen below will show that you have successfully installed the certificates.

     ![Create certificates_8](./Images/Create%20certificates_8.png)


   - Now go back to the Tooling site and refresh your browser. It will change the Toolingysf.com site from unsecured to secured as shown below.


     ![Toolingysf_secured](./Images/Toolingysf_secured.png)


    - You can click on the lock to view site i formation, then connection secured. You can get the certificates information.

      ![Certificates info](./Images/Certificates%20Info.png)


    -  You can login to the site with;

       Username:admin

       Pw: admin


        ![Propitix Tooling Login_2](./Images/Propitix%20Tooling%20Login_2.png)


    -  We need to create a cron tab.

       You can test renewal command in `dry-run` mode.

       `sudo certbot renew --dry-run`


       ![Renew dry run](./Images/Renew%20dry%20run.png)

      - It is a best practice to have a scheduled job to run "renew" command periodically. Let us configure a cronjob to run the command twice  a day.

     - To do so, let;s edit the crontab file with the following command;

       `crontab -e`


       ![Crontab](./Images/Crontab.png)


      - Select 1 and Enter.


        ![Crontab_1](./Images/Crontab_1.png)


     - The screen below will be displayed.

       ![Crontab_2](./Images/Crontab_2.png)

     - Scroll to the bottom of the file and you will see;

         m h dom mon dow  command

       ```
       m - month
       h - hour
       dom - day of the month
       mon - month
       dow - day of the week
       command - command you want to run
       ```


       Add the following line;

        `* */12 * * *  root /usr/bin/certbot renew > /dev/null 2>&1`

       The command means that the job will be running for all months, every day at 12th minute of every hour, everyday of the month, every month, and every day of the week.


       ![Crontab_2](./Images/Crontab_3.png)

       Write out by Ctrl O , Enter and Exit by Ctrl X.

       The screen below will be displayed.

       ![Cronjob](./Images/Cronjob.png)


       Currently the website is up and running and secured.



       You can always change the frequency of the cronjob if twice a day is too often by adjusting schedule expression.


       Complete.







































 



































































