# Altprobe

Altprobe is a component of the Alertflex project, it has functional of a collector according to SIEM/Log Management terminologies.
In tandem with Alertflex controller (see [AlertflexCtrl repository](https://github.com/olegzhr/AlertflexCtrl/blob/master/README.md) ), 
Altprobe can integrate a Wazuh Host IDS (OSSEC fork) and Suricata Network IDS
with Log Management platform Graylog and Threat Intelligence Platform MISP. 

## Functionalities of Altprobe

* Reads through the server Redis events in JSON format from Suricata NIDS, Wazuh HIDS, Modsecurity WAF, ElasticStack MetricBeat
* Based on filtering policies, Collector retrieves high priority events from data streams created by security sensors, makes aggregation and normalization for these events. This allows to simplify the managementof alerts and incidents, reduces noise from minor events.
* High priority events (alerts) are immediately sent to the central node
* Events (log events) that have been rejected for processing as alerts can be redirected to the Log Management platform
* All log events are sent to the Controller pre-accumulated in a compressed data set, this implements the "Anti-flooding" algorithm to prevent large bursts of events on the controller side.
* In case of loss of communication between remote and central nodes, the Collector saves all alerts locally in a file
* Generates an alert if the network traffic thresholds have reached the limits
* Generates an alert if the metrics thresholds for hosts (CPU, Memory, HDD, Swap) have reached the limits
* Creates reports on network activities of host processes (based on events from Sysmon for Windows and Auditd for Linux, received through Wazuh IDS). This allows determining the name of the process associated with suspicious network connections.
* To avoid loss of useful information, the Collector collects various statistics about all events that have been sent via the Collector

## Type of events (GELF format is used), that are generated by Altprobe

* "short_message":"alert-flex", "full_message":"Alert from Alertflex collector/controller"

* "short_message":"alert-hids", "full_message":"Alert from OSSEC/Wazuh HIDS"

* "short_message":"alert-fim", "full_message":"Alert from OSSEC/Wazuh FIM"

* "short_message":"alert-nids", "full_message":"Alert from Suricata NIDS"

* "short_message":"dns-nids", "full_message":"DNS event from Suricata NIDS"

* "short_message":"ssh-nids", "full_message":"SSH event from Suricata NIDS"

* "short_message":"netflow-nids", "full_message":"Netflow event from Suricata NIDS"

* "short_message":"alert-waf", "full_message":"Alert from ModSecurity/NGINX"

* "short_message":"process-linux", "full_message":"Network activity of linux process from Auditd"

* "short_message":"process-win", "full_message":"Network activity of windows process from Sysmon"

## Screenshots

Below, a screenshots of Graylog web forms for IDS events from Altprobe

![](https://github.com/olegzhr/altprobe/blob/master/img/graylog1.jpg)
Normalized and aggregated alerts from Host and Network IDS

![](https://github.com/olegzhr/altprobe/blob/master/img/graylog2.jpg)
Simple statistics about IDS alerts categories, applications protocols and Geo IP netflow map

## Requirements

Altprobe was tested under Ubuntu version 14.04 with Wazuh HIDS (OSSEC fork) version 3.2, ModSecurity 3.0 and Nginx, Suricata NIDS version 4.0.3

## Installation instructions

Download installation files

    $ sudo apt-get install git
    git clone git://github.com/olegzhr/Altprobe.git
    cd ./Altprobe

Fill in collector specific parameters in file ``env.sh`` and start installation		
    
    $ chmod u+x install_node.sh
    ./install_node.sh

For the distributed configuration with SSL connections, you need to perform extra steps, see installation instructions for AlertflexCtrl

NOTE:

* Installation and configuration of IDS OSSEC/Wazuh and Suricata IDS are not parts of Alertflex solution. Although install script for Altprobe includes these procedures, it comes with NO WARRANTY!

* The collector’s install script will replace file /etc/rc.local by a new one, if you use this file for something else, backup it for keeping the previous configuration

* For the distributed configuration all nodes should be reachable over a network via DNS names or host names (use these names in file ``env.sh`` as values for parameters)

* For enabling an events from Sysmon via Wazuh IDS, please, change level of ``rule_id 185001`` instead 0  to other value. See file ``/var/ossec/ruleset/rules0330-sysmon_rules.xml``

* For enabling an network activities events from Auditd, please, use the command: ``auditctl -a exit,always -F arch=b64 -S connect -k linux-connects``, key value ``linux-connects`` is important!

* For integration Nginx and ModSecurity 3.0 with Wazuh IDS, add string ``error_log  /var/log/nginx/error.log info;`` to file  nginx.conf. 
Examples of Modsecurity rules for Wazuh, please, see in file [0260-nginx_rules.xml](https://github.com/olegzhr/Altprobe/blob/master/configs/0260-nginx_rules.xml)

* For advanced configuration of Altprobe use altprobe config file and filtering policies, please, see samples in dir: [/src/etc/](https://github.com/olegzhr/Altprobe/blob/master/src/etc/)

## Support

Please [open an issue on GitHub](https://github.com/olegzhr/altprobe/issues) or send an email to <info@alertflex.org>,
if you'd like to report a bug or request a feature 


## Old version of Altprobe 

Previous version of altprobe (single package with support Ntop nProbe, ZeroMQ as sources and output to MySQL) is available under branch ``old_version``


