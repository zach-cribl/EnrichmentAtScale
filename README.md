## Instructions
### Start order
1. Cribl Stream
2. Redis
3. Opensearch
### Start commands
All of the docker-compose files should be startable with

`docker-compose up -d`

Create a cribl-config folder in the CriblStream directory!

### Resources
*Stuff for copy/paste*

Intel locations:
https://rules.emergingthreats.net/blockrules/compromised-ips.txt
https://feodotracker.abuse.ch/downloads/ipblocklist_recommended.txt

Geoip database no registration (you can get an updated copy by registering at [maxmind](https://dev.maxmind.com/geoip/geolite2-free-geolocation-data)):
https://github.com/P3TERX/GeoLite.mmdb

### Troubleshooting 
Make sure you use `host.docker.internal` to access the ports that are shared by the containers. For example for the Cribl Stream container to access the OpenSearch container you'll need to use the URL `https://host.docker.internal:9200/_bulk`.

If data is not going through pipelines correctly, check the condition to make sure the sourcetype or __inputId match the data (*Case matters*).

#### **Windows specific issues**
Windows can create issues for this workshop in a few different ways. 

The first issue encountered is Windows reserving ports which intersect with the ones used by this workshop's containers. The ports needed are 6379(Redis), 9200(OpenSearch), 5601(OpenSearch Dashboards), 19000(Cribl Stream). If your system is reserving these  ports, you will need to change the host port in the docker file to a non-reserved port. Syntax in the 'ports:' section is [host port]:[container port].

The second issue which has been encountered is an issue with the WSL2 engine not allowing enough virtual memory maps (for the elastic container). This can be fixe by increasing the vm.max_map_count sysctl parameter in the `%UserProfile%/.wslconfig` file. The file should contain the following content:
``` 
[wsl2]
kernelCommandLine = "sysctl.vm.max_map_count=262144"
```
References:
* Issue 1: https://github.com/docker/for-win/issues/3171
* Issue 2: https://stackoverflow.com/questions/69214301/using-docker-desktop-for-windows-how-can-sysctl-parameters-be-configured-to-per/69294687#69294687
