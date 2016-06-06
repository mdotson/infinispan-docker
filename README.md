This repository contains a setup for:  
1. A (free) splunk container which accepts data from the splunk forwarder on port 9997
2. A splunk forwarder which will take input from UDP port 514 and forward it to splunk via port 9997
3. An infinispan container which will publish its logs to the splunk forwarder instance via UDP over port 514

# Splunk
    docker-compose -f splunk/docker-compose.yml up -d
    docker exec -it splunk_splunk_1 entrypoint.sh splunk enable listen 9997 -auth admin:changeme

# Splunk Forwarder
    docker-compose -f splunk_forwarder/docker-compose.yml up -d
    docker exec -it splunkforwarder_forwarder_1 entrypoint.sh splunk add udp 1514 -sourcetype syslog

# Infinispan
    docker-compose -f infinispan/docker-compose.yml up -d
