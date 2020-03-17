# Portable SIEM config files
Elastic docker compose files as well as files for configs
3/16/20- Added Wazuh manager

In the docker-compose file, remember you have to run the docker-compose up command from the command line in the directory the file is in.

Also on the left side of the : in the volumes section and the ports section are that of the host machine

You will need to make or change the directories on the left and make sure the files are there that need to be.

You will need to download the beats (7.6.1) and run the following in the directory of the beats: (ex:packetbeat)





\install-service-packetbeat.ps1

.\packetbeat.exe setup --index-management -E output.logstash.enabled=false -E 'output.elasticsearch.hosts=["localhost:9200"]'
(this tells it for this one time run without logstash to connect to elastic and load indexes on the ip and port of elastic server.)

Start-Service packetbeat



Also no need to put the wazuh client on the hosts in this version. It give a powershell or bash script to authenticate the user automagically!!

If you need help or have questions let me know.
The stable folder is the logstash config files.

