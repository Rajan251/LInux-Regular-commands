### List of commands help to find malicious activity

### This command gives you a quick list of the top IPs trying to brute-force SSH on your server.
---
sudo grep "Failed password" /var/log/auth.log | awk '{for(i=1;i<=NF;i++) if($i=="from") print $(i+1)}' | sort | uniq -c | sort -nr | head
 
 ***output***
 ```sh
  5273 157.250.203.74
   1705 196.251.84.225
    798 187.108.193.162
    774 179.61.154.32
    716 45.188.181.56
    575 80.94.95.112
    573 62.60.131.158
    537 62.60.131.157
    511 193.46.255.33
    498 185.246.128.171
 ```
