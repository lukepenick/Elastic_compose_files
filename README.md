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

# How to start:
# 1: Start the docker containers
<code class ="sh">
 
docker-compose up -d;

</code>

# 2: Run the Elastic password tool

<code class ="sh">
 
docker exec es01 /bin/bash -c "bin/elasticsearch-setup-passwords \
auto --batch \
--url https://es01:9200" > passwords.txt;

</code>
After this completes open the docker-compose.yml file and edit line 57 to mirrior the kibana password from the txt file

# 3:Take docker down and back up:
In powershell
<code class ="sh">
 docker-compose down

</code>


# 4 Add elastic certs into trusted stores on host machine:


<code class ="sh">
Import-Certificate -FilePath "C:\mobilesiem\testing\certs\ca\ca.crt" -CertStoreLocation Cert:\LocalMachine\Root
Import-Certificate -FilePath "C:\mobilesiem\testing\certs\es01\es01.crt" -CertStoreLocation Cert:\LocalMachine\Root
Import-Certificate -FilePath "C:\mobilesiem\testing\certs\kib01\kib01.crt" -CertStoreLocation Cert:\LocalMachine\Root
Import-Certificate -FilePath "C:\mobilesiem\testing\certs\wazuh\wazuh.crt" -CertStoreLocation Cert:\LocalMachine\Root

</code>

# 5:
Edit the docker-compose.yml file with the updated elasticsearch password in the kibana section and in the filebeat.yml file

# 6:

<code class ="sh">
docker-compose up
 
 </code>




# HOST VISIBILITY:
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
