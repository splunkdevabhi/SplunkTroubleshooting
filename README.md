# SplunkTroubleshooting
A repository of important troubleshooting searches for Splunk

# Splunk Problem Classification
![Image 1](https://github.com/splunkdevabhi/SplunkTroubleshooting/blob/master/Screen%20Shot%202020-09-29%20at%2010.59.44%20AM.png?raw=true)

# What Splunk Logs About Itself
![Image 2](https://github.com/splunkdevabhi/SplunkTroubleshooting/blob/master/What%20Splunk%20logs.png?raw=true) 

# Useful Pipeline Searches with Metrics.log

**How much time is Splunk spending within each pipeline?**
<br />`index=_internal source=*metrics.log* group=pipeline 
| timechart sum(cpu_seconds) by name`

**How much time is Splunk spending within each processor?**
<br />`index=_internal source=*metrics.log* group=pipeline 
| timechart sum(cpu_seconds) by processor`

**What is the 95th percentile of measured queue size?**
<br />`index=_internal source=*metrics.log* group=queue 
| timechart perc95(current_size) by name`

**What is the maximum number of entries used in each queue? 
(1000 is max queue size, except for forwarding)**
<br />`index=_internal source=*metrics.log* group=queue 
| timechart max(current_size) by name`


# Troubleshooting Forwarding 

**Which forwarders are sending data to Splunk and how much?**
<br />`index=_internal sourcetype=splunkd host=<indexer> metrics tcpin_connections
|  timechart span=5m max(tcp_KBps) by sourceIp`

**Where is the forwarder trying to send data to?**
<br />`index=_internal host=< uf > sourcetype=splunkd StatusMgr destHost`

**What output queues are set up?**
<br />`index=_internal host=<uf >  source=* metrics.log group=queue tcpout
| stats count by name`

# Distributed Search Job Overview
![Image 3](https://github.com/splunkdevabhi/SplunkTroubleshooting/blob/master/DSJ%20Overview.png?raw=true)

# Search Problem Categories 
![Image 4](https://github.com/splunkdevabhi/SplunkTroubleshooting/blob/master/Search%20Problems.png?raw=true)

# Troubleshooting User Searches 

**Lengthy Search?**
<br />`index="_audit" action="search" (id=* OR search_id=*) | eval user=if(user=="n/a",null(),user) | stats max(total_run_time) as  total_run_time first(user) as user by search_id
| stats count perc95(total_run_time) median(total_run_time) by user`

**How much time are the indexers spending in response to queries from SH?**
<br />`index=_internal source=* remote_searches.log server=<sh> | stats max(elapsedTime) by search_id host`

**Identify all splunkd responses taking more than 100ms** 
<br />`index=_internal sourcetype=splunkd_access host=<sh> user=<user> | rex "(?<spent>\d+)ms" | search spent > 100`

**What is the size of the artifacts?**
<br />`index=_internal sourcetype=splunkd_access method=GET jobs | stats sum(bytes) by uri`

# Troubleshooting No Results Problems
<br /> **The following checks can help troubleshoot no results **

