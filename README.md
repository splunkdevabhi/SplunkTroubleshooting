# SplunkTroubleshooting
A repository of important troubleshooting searches for Splunk

# Splunk Problem Classification
![Image 1](https://github.com/splunkdevabhi/SplunkTroubleshooting/blob/master/Screen%20Shot%202020-09-29%20at%2010.59.44%20AM.png?raw=true)

# What Splunk Logs About Itself
![Image 2](https://github.com/splunkdevabhi/SplunkTroubleshooting/blob/master/What%20Splunk%20logs.png?raw=true) 

# Useful Pipeline Searches with Metrics.log

**How much time is Splunk spending within each pipeline?**
`index=_internal source=*metrics.log* group=pipeline 
| timechart sum(cpu_seconds) by name`

**How much time is Splunk spending within each processor?**
`index=_internal source=*metrics.log* group=pipeline 
| timechart sum(cpu_seconds) by processor`

**What is the 95th percentile of measured queue size?**
`index=_internal source=*metrics.log* group=queue 
| timechart perc95(current_size) by name`

**What is the maximum number of entries used in each queue? 
(1000 is max queue size, except for forwarding)**
`index=_internal source=*metrics.log* group=queue 
| timechart max(current_size) by name`





