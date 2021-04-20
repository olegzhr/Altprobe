The repository includes Alertflex collector and installation scripts for security sensors (Suricata NIDS, Wazuh HIDS, Falco CRS). 

Alertflex project is an automation, continuous monitoring, threat detection and response solution. The Alertflex works as a Security Event Manager with SOAR functionality for a distributed grid of security sensors and scanners.

Solution is designed for use in Hybrid IT infrastructure (on-premises and cloud-based), provides an orchestrator and a single pane of glass for more than 30 products (mostly free open-source software), monitors different types of platforms: Windows, Linux, Docker, Kubernetes, Amazon AWS.

One of the advantages of Alertflex is cyber threat intelligence and response close to real-time. Additionally, Alertflex integrates with the OpenDistro Anomaly Detection feature (Random Cut Forest AI algorithm), you can configure automatic response in case of anomaly detection for certain security events rate.

![](https://github.com/alertflex/altprobe/blob/master/img/dashboard.png)

For working inside of Hybrid IT environment, the solution consists of distributed software components Collector, Controller, Management Console. Alertflex Collector (Altprobe) is placed in the network segment where security sensors are installed (Container Runtime Security, Host IDS, File Integrity Monitor, The Docker Bench for Security, Network IDS). Together with security sensors and scanners, Collector logically forms the Remote node.

![](https://github.com/alertflex/altprobe/blob/master/img/lld-arch.png)

For more information, please see [solution description](https://alertflex.github.io/solution.html) and [project documentation](https://alertflex.github.io/doc/index.html)

Please [open an issue on GitHub](https://github.com/alertflex/altprobe/issues), if you'd like to report a bug or request a feature. 
Have a question or need tech support, please send an email to address: info@alertflex.org
and join the community via [Alertflex Discord server](https://discord.gg/wDSz7rDMWv)
