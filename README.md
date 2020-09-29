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

**Which forwardersare sending data to Splunk and how much?**
`index=_internal sourcetype=splunkd host=<indexer> metrics tcpin_connections
|  timechart span=5m max(tcp_KBps) by sourceIp`

**Where is the forwarder trying to send data to?**
`index=_internal host=< uf > sourcetype=splunkd StatusMgr destHost`

**What output queues are set up?**
`index=_internal host=<uf >  source=* metrics.log group=queue tcpout
| stats count by name`


