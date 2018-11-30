Problem Statement  
  
1.Automate the setting up and configuation of a linux server via any configuation management tool for deploying Java application.  
  
Considerations:   
1.Ansible is used for configuration management.  
2.tomcat1 stands for 1st tomcat node and tomcat2 for 2nd tomcat node.       
  
Solution:  
  
#Install and configure apache:  
1.Have used apache.yml file to install and configure apache also for auto restart on server reboot.  
2.Ansible command to run the apache.yml file: ansible-playbook apache.yml -c local --ask-sudo-pass  
  
#Install and configure Mysql  
1.Have used mysql.yml file to install and configure mysql also for auto restart on server reboot.  
2.Ansible command to run the mysql.yml file: ansible-playbook mysql.yml -c local --ask-sudo-pass  
  
#Install and configure two tomcat 7 nodes:  
1.Have used tomcat1.yml file to install and configure tomcat 7's first node and  also for auto restart on server reboot.  
2.Ansible command to run the tomcat1.yml file: ansible-playbook tomcat1.yml -c local --ask-sudo-pass  
3.Have used tomcat2.yml file to install and configure tomcat 7's second node and  also for auto restart on server reboot.  
4.Ansible command to run the tomcat2.yml file: ansible-playbook tomcat2.yml -c local --ask-sudo-pass  
  
#Configuation details:  
1."res" folder contains configuration files for apache and tomcat.  
2. hosts file contains the server on which ansible commands can be executed.  
configuration details for hosts file:  
  
[localhost]  
localhost anisble_ssh_user=ubuntu  
  
3.tomcat1 configuration file used: res/tomcat1.conf.server.xml  
4.tomcat2 configuration file used : res/tomcat2.conf.server.xml  
5.tomcat1 and tomcat2 configuration details:   
  
Tomcat1 shutdown port = 8001  
Tom2 shutdown port = 8002  
  
Tomcat1 service name = "Catalina1"  
Tomcat2 service name = "Catalina2"  
  
Tomcat1 connector (http) = 8089  
Tomcat2 connector (http) = 8090  
  
Tomcat1 redirect port = 8443  
Tomcat2 redirect port = 8444  
  
Tomcat1 jvmRoute = tomcat1  
Tomcat2 jvmRoute = tomcat2  
  
6.Apache configuration file used : res/apache2.conf  
Apache configuration details:  
  
&lt;IfModule proxy_module&gt;  
ProxyPass /petstore balancer://apache_lb  
ProxyPassReverse /petstore balancer://apache_lb  
  
&lt;Proxy balancer://apache_lb&gt;  
BalancerMember http://localhost:8089/petstore route=tomcat1  
BalancerMember http://localhost:8090/petstore route=tomcat2  
&lt;/Proxy&gt;  
  
&lt;/IfModule&gt;  
  
#Application Deployment details:  
1.Have downloaded 'petstore' app and deployed in both the tomcat nodes.  
2.Have added index.html in both tomcat1 and tomcat2 nodes.  
3.tomcat1 index.html page content:   
Hello from tomcat1 ..   
4.tomcat1 index.html page content:   
Hello from tomcat2 ..   
  
#Access the petstore App.  
1.URL to access the app: http://184.73.3.51/petstore  
Note: On page refresh it will route request to either tomcat1 or tomcat2.  
 
 
