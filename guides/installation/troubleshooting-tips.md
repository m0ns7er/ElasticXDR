---
description: Elasticsearch & Kibana Tips
---

# Troubleshooting Tips

If you get an error message like this for **Kibana** , **Elasticsearch** or **Filebeat.**

```
● elasticsearch.service - Elasticsearch
   Loaded: loaded (/usr/lib/systemd/system/elasticsearch.service; enabled; vendor preset: ena   Active: failed (Result: timeout) since Tue 2020-12-15 00:44:16 UTC; 24min ago
     Docs: https://www.elastic.co
  Process: 1007 ExecStart=/usr/share/elasticsearch/bin/systemd-entrypoint -p ${PID_DIR}/elast Main PID: 1007 (code=killed, signal=TERM)
    Tasks: 0 (limit: 4631)
   CGroup: /system.slice/elasticsearch.service

Dec 15 00:45:11 test01 systemd[1]: Starting Elasticsearch...
Dec 15 00:44:16 test01 systemd[1]: elasticsearch.service: Start operation timed out. TerminatDec 15 00:44:16 test01 systemd[1]: elasticsearch.service: Failed with result 'timeout'.
Dec 15 00:44:16 test01 systemd[1]: Failed to start Elasticsearch.
```

Try undoing the changes you have done by, **(#)** commenting you changes then save the file and restart the services. This will confirm if your recent last changes have made the file unstable!

This happens when an entry was not taken in the **.yml** file, the file may be damaged or corrupted.

Once that is done, everything should work after that!



Or you did not give the system enough resources, please power off and go back to.

{% content-ref url="./" %}
[.](./)
{% endcontent-ref %}

And give your Server Enough Memory and CPU for running these Services.
