# Portable SIEM config files
Elastic docker compose files


 Download docker desktop and docker compose for windows or linux 


# How to start:
 Create a folder in C: called SIEM (in linux /SIEM) or pull the entire directory from this page.
 

Open a powershell window (or bash) and navigate to the folder you created above. 
Drop the instances.yml, .env, docker-compose.yml, and create-certs.yml file into the SIEM folder

# RENAME THE ENV FILE TO .env!!!
run the following:

<code class ="sh">
 


docker-compose -f create-certs.yml run --rm create_certs

 
 </code>


# 1: Start the docker containers or use the scripts in linux

Drop the scripts into somewhere your path shows or add a directory to your path
(Will have a powershell script soon)
(Make sure the scripts have execute permissions
<code class ="sh">
elastic-startup 
</code>

If not using the script:
<code class ="sh">
docker-compose up -d
</code>

# 2: Run the Elastic password tool

<code class ="sh">
 
docker exec es01 /bin/bash -c "bin/elasticsearch-setup-passwords \
auto --batch \
--url https://es01:9200" > passwords.txt;

</code>

# 3:Take docker down:

<code class ="sh">
 docker-compose down

</code>

# 4:Change your kibana password:

After this completes open the docker-compose.yml file and edit the "kibana_system" password to mirrior the password from the txt file

# 5:Bring the containers back up:

<code class ="sh">
docker-compose up -d
</code>

Now you should be able to go to https://localhost:5601 or https://<IP of device>:5601
 
The username is elastic and the password is in the passwords.txt file.



Enjoy! If you have any contributions or to report issues please let me know!
