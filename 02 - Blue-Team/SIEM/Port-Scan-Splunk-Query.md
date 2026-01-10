index=network sourcetype=pcap 
| stats count(eval(tcp.flags.syn==1)) as syn_count by src_ip
| where syn_count > 10
