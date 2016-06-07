This repository contains a setup for:  

1. A (free) splunk container which accepts data from the splunk forwarder on port 9997
2. A splunk forwarder which will take input from port 1514 and forward it to splunk via port 9997
3. An infinispan container which will publish its logs to the splunk forwarder instance via port 1514

Run with:

    export SPLUNK_FROM_PUPPET=$(echo $DOCKER_HOST | awk -F/ '{print $3}' | awk -F: '{print $1}')

Will probably remove these, so just use the command above instead:

# Splunk
    docker-compose -f splunk/docker-compose.yml up -d
    docker exec -it splunk_splunk_1 entrypoint.sh splunk enable listen 9997 -auth admin:changeme

# Splunk Forwarder
    docker-compose -f splunk_forwarder/docker-compose.yml up -d
    docker exec -it splunkforwarder_forwarder_1 entrypoint.sh splunk add udp 1514 -sourcetype syslog

# Infinispan
    docker-compose -f infinispan/docker-compose.yml up -d
    
This repository is based on [this article](http://blogs.splunk.com/2015/08/24/collecting-docker-logs-and-stats-with-splunk/)
