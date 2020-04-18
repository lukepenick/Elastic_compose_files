# Portable SIEM config files
Elastic docker compose files as well as files for configs
4/11/20- Full TLS/SSL between nodes RBA (role based authentication) 

 Download docker desktop for windows, packetbeat, winlogbeat, and the wazuh agent
 
 Create a folder in C: called MobileSIEM
 
 Inside that folder decompress the certs:
https://drive.google.com/file/d/1SFRfEIZmj3_3oSbAmpQEOszp_4P7sRRW/view?usp=sharing
 
If you want your own certs download elasticsearch 7.6.2 
Decompress the folder
Open a powershell window and navigate to the folder. 
Drop the instances.yml file into the mobileSIEM folder

run the following:

<code class ="sh">
 
elasticsearch-certutil cert --silent --pem --in C:\MobileSIEM\instances.yml -out C:\MobileSIEM\certs.zip
 
 </code>
Then decompress



Also drop the docker-compose.yml and the filebeat.yml file here

In powershell navigate to the directory C:\MobileSIEM\

# Run these commands:
# 1:
<code class ="sh">
 
docker-compose up -d;

</code>

# 2:

<code class ="sh">
 
docker exec es01 /bin/bash -c "bin/elasticsearch-setup-passwords \
auto --batch \
--url https://es01:9200" > passwords.txt;

</code>

(you can copy and paste the above or run indivdualy)
((YOU WILL NEED THE PASSWORDS FROM THE TEXT FILE BEING OUTPUT))

# 3:In docker desktop go to kib01 and click cli:

<code class ="sh">
usr/share/kibana/bin/kibana-plugin install https://packages.wazuh.com/wazuhapp/wazuhapp-3.12.2_7.6.2.zip

</code>

# 4:In docker desktop go to wazuh and click cli:

<code class ="sh">
/var/ossec/api/configuration/config.js
 
 </code>
 
(change to:)

<code class ="sh">
"// HTTPS Certificates
config.https_key = "configuration/ssl/wazuh.key"

config.https_cert = "configuration/ssl/wazuh.crt"

config.https_use_ca = "no"

config.https_ca = "configuration/ssl/ca.crt"

</code>

# 5:


<code class ="sh">
vi /usr/share/kibana/optimize/wazuh/config/wazuh.yml
hosts:
  - default:
     url: http://wazuh
     port: 55000
     user: foo
     password: bar
 
 </code>
     
# 6:

<code class ="sh">
docker-compose stop

</code>

# 7:


<code class ="sh">
Import-Certificate -FilePath "C:\mobilesiem\testing\certs\ca\ca.crt" -CertStoreLocation Cert:\LocalMachine\Root
Import-Certificate -FilePath "C:\mobilesiem\testing\certs\es01\es01.crt" -CertStoreLocation Cert:\LocalMachine\Root
Import-Certificate -FilePath "C:\mobilesiem\testing\certs\kib01\kib01.crt" -CertStoreLocation Cert:\LocalMachine\Root
Import-Certificate -FilePath "C:\mobilesiem\testing\certs\wazuh\wazuh.crt" -CertStoreLocation Cert:\LocalMachine\Root

</code>

# 8:
Edit the docker-compose.yml file with the updated elasticsearch password in the kibana section and in the filebeat.yml file

# 9:

<code class ="sh">
docker-compose up
 
 </code>



HOST VISIBILITY:
You will need to download the beats (7.6.2) and run the following in the directory of the beats: (ex:packetbeat) and follow the instructions for set up. You will need to put in the elastic username and password like in the filebeat.yml file. 
all hosts will be https://localhost or commented like so, protocol: https    hosts:['localhost:']

In the Wazuh app in Kibana go to agents and click deploy agents.

Change the server address to localhost and run the powershell script with admin privileges.

Start the Wazuh.msi 



OTHERNOTES:
Only use logstash is the event elastic doesnt support logs from whatever you are trying to get, it will cause you not to get IPs and whatnot of hosts you are sending through it.

If you want logstash to run please visit and follow the steps to add it:
https://www.elastic.co/guide/en/logstash/7.6/ls-security.html

In Kibana you should create a new user to login with
On the left go to the cog (Management)>Security section, Users> then click add new user.
You can make this a superuser if you like. Be careful on what permissions you have. This could cause you to not be able to access certian things.
For more info:
https://www.elastic.co/guide/en/kibana/current/development-security-rbac.html

Enjoy! If you have any contributions or to report issues please let me know!
