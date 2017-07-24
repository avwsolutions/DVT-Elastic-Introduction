# Introduction to Elastic Stack

<img src="https://raw.githubusercontent.com/avwsolutions/DVT-Elastic-Introduction/master/content/banner.png" alt="introduction banner">

Below an overview of all Elastic components which help you to setup an Elastic Stack. Most familiar is using Elastic-Logstash-Kibana and additional Beats agents. if the customer buys support additional features can be added such as Security (Shield), Monitoring (Marvel, Alerting (Watcher) and Reporting. Other nice kick-ass features for Platinum and Enterprise are Graph and Machine Learning (Prelert).

<img src="https://raw.githubusercontent.com/avwsolutions/DVT-Elastic-Introduction/master/content/elastic_stack.png" alt="Elastic stack">

- Elasticsearch ; *is our lucene search enabled document store, for indexing and analysing our data.*
- Logstash ; *is our swiss knife for advanced indexing to Elasticsearch and complex ingesting of data sources like data queues.*
- Kibana ; *is the key user interface component, which brings data & visualisations together on one or more beautiful dashboards.*
- Beats ; *is the family name for many lightweight data shippers like MetricBeat, FileBeat, WinlogBeat, PacketBeat and HeartBeat. For diehards we have LibBeat to brew your own GoLang.*
- Optional X-Pack ; *Adds amazing features like Security, Alerting, Monitoring, Reporting, Graph or even Machine Learning capabilities.*
- Optional Elastic Cloud ; *This is the hosted variant for currently Elasticsearch and Kibana. New release will include Logstash.*

## Training materials:

*This will be a self-paced beginners’ tutorial for attendees to learn Elastic basics as they install the components within their VirtualBox. Experienced Devoteam colleagues will serve as support to help attendees successfully complete this tutorial.*

:warning: **This section prepares you for the introduction before you actually attend.**

## Pre-tutorial preparation

At the KES/KISS, you will need to bring your own computer. Before you go to the KES/KISS, there are some steps you should do some preparation to get your work environment ready. Here are the steps:
- For PC, Mac and Linux users we need you to install the latest version of [VirtualBox](https://www.virtualbox.org/wiki/Downloads).
- Use the following installation instructions for [Windows] (https://www.virtualbox.org/manual/ch02.html#installation_windows).
- Now download the following OVA file, which delivers a fully functional CentOS 7.0 (minimal - No GUI) installation.
- Use PuTTY or any other compatible SSH client to connect to the terminal interface using 127.0.0.1, port 2222. As user account use the 'elastic' account.

## Introduction tutorial starts here

> Note : Elastic recommends using Oracle Java over OpenJDK. In this tutorial we will use OpenJDK which also is supported.

Below you will find the commands for installing the openJDK JRE and checking if the path & version (1.8 or greater) are correct. 

```
$ sudo yum -y install jre
$ sudo java -version
openjdk version "1.8.0_141"
OpenJDK Runtime Environment (build 1.8.0_141-b16)
OpenJDK 64-Bit Server VM (build 25.141-b16, mixed mode)
```

### Installation Elasticsearch

First step is to to get introduced to [Elasticsearch](https://www.elastic.co/products/elasticsearch)

> Note : Be aware that for this task internet connectivity is needed. For convenience the Elastic repository is configured and there is already a cache yum download available.

```
$ rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch

$ cat /etc/yum.repos.d/elastic.repo

[elasticsearch-5.x]
name=Elasticsearch repository for 5.x packages
baseurl=https://artifacts.elastic.co/packages/5.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md

```

Below you will find the command for installing elasticsearch. 

```
$ sudo yum -y install elasticsearch
```

Now use the following commands to automatically start the daemon at startup.

```
$ sudo /bin/systemctl daemon-reload
$ sudo /bin/systemctl enable elasticsearch.service

# Now we can stop & start the daemon
$ sudo systemctl stop elasticsearch.service
$ sudo systemctl start elasticsearch.service
```

### Installation Logstash

First step is to to get introduced to [Logstash](https://www.elastic.co/products/logstash)

> Note : Be aware that for this task internet connectivity is needed. For convenience the Elastic repository is configured and there is already a cache yum download available.

Below you will find the command for installing logstash. 

```
$ sudo yum -y install logstash
```

Now use the following commands to automatically start the daemon at startup.

```
$ sudo /bin/systemctl daemon-reload
$ sudo /bin/systemctl enable logstash.service

```

Now create some simple configuration for logstash.

```
input {
  beats {
    # The port to listen on for filebeat connections.
    port => 5044
    # The IP address to listen for filebeat connections.
    host => "0.0.0.0"
  }
}
filter {

}
output {
  elasticsearch {
    hosts => localhost
    manage_template => false
    index => "%{[@metadata][beat]}-%{+YYYY.MM.dd}"
    document_type => "%{[@metadata][type]}"
  }
}
```
And validate.

```
$ /usr/share/logstash/bin/logstash --path.settings /etc/logstash/ -t
```

```
# Now we can stop & start the daemon
$ sudo systemctl stop logstash.service
$ sudo systemctl start logstash.service
```
### Installation Kibana

First step is to to get introduced to [Kibana](https://www.elastic.co/products/kibana)

> Note : Be aware that for this task internet connectivity is needed. For convenience the Elastic repository is configured and there is already a cache yum download available.

Below you will find the command for installing logstash. 

```
$ sudo yum -y install kibana
```

Now use the following commands to automatically start the daemon at startup.

```
$ sudo /bin/systemctl daemon-reload
$ sudo /bin/systemctl enable kibana.service

```
Now edit the configuration file and add a acces rule to enable external access from your laptop. In our case through NAT forwarding to port 5601.

```
$ vi /etc/kibana/kibana.yml

# Now change 'localhost' to 0.0.0.0 and save kibana.yml.

# Specifies the address to which the Kibana server will bind. IP addresses and host names are both valid values.
# The default is 'localhost', which usually means remote machines will not be able to connect.
# To allow connections from remote users, set this parameter to a non-loopback address.
server.host: 0.0.0.0

$ firewall-cmd --permanent --add-port 5601/tcp
success
$ firewall-cmd --reload

```

```
# Now we can stop & start the daemon
$ sudo systemctl stop kibana.service
$ sudo systemctl start kibana.service
```
> Note : You can now acces the kibana user interface on [Kibana](http://127.0.0.1:5601), but keep in mind no events are received yet.


### Installation Beats

First step is to to get introduced to [Beats](https://www.elastic.co/products/beats)

> Note : Be aware that for this task internet connectivity is needed. For convenience the Elastic repository is configured and there is already a cache yum download available.

Below you will find the command for installing filebeat and metricbeat. 

```
$ sudo yum -y install filebeat metricbeat

```

Now use the following commands to automatically start the daemon at startup.

```
$ sudo /bin/systemctl daemon-reload
$ sudo /bin/systemctl enable filebeat.service metricbeat.service

```
For this simple configuration we just replace the elasticsearch output for the logstash output for both filebeat and metricbeat. Please take in account the YAML format e.g. 2 spaces.

```
#disable output.elasticsearch
#output.elasticsearch:
  # Array of hosts to connect to.
  #hosts: ["localhost:9200"]

#enable output.logstash
output.logstash:
  # The Logstash hosts
  hosts: ["localhost:5044"]
  
```
Now load the correct templates for both filebeat and metricbeat

```
$ curl -XPUT 'http://localhost:9200/_template/filebeat' -d@/etc/filebeat/filebeat.template.json
{"acknowledged":true}
$ curl -XPUT 'http://localhost:9200/_template/metricbeat' -d@/etc/metricbeat/metricbeat.template.json
{"acknowledged":true}
```

```
# Now we can stop & start the daemon
$ sudo systemctl stop metricbeat.service
$ sudo systemctl start metricbeat.service
$ sudo systemctl stop filebeat.service
$ sudo systemctl start filebeat.service
```
### Importing default dashboards

Now the first logs & metrics are flooding into your stack. Now it's time to create your index patterns.

- Use filebeat-\* for filebeat and use @timestamp for Time Filter field name.
- Use metricbeat-\* for metricbeat and use @timestamp for Time Filter field name.

We can also import some default dashboards, which is simply done with one command. 

```
$ /usr/share/filebeat/scripts/import_dashboards
$ /usr/share/metricbeat/scripts/import_dashboards
```
### Installation X-Pack

X-pack can be easily installed with the following commands and must be installed for both elasticsearch and kibana. Don't forget to restart the daemons.

:warning:Be aware that after installation minimal security is activated. This forces us to login with the default username:elastic and password:changeme. 

```
$ /usr/share/elasticsearch/bin/elasticsearch-plugin install x-pack
-> Downloading x-pack from elastic
[=================================================] 100%?? 
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@     WARNING: plugin requires additional permissions     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
* java.io.FilePermission \\.\pipe\* read,write
* java.lang.RuntimePermission accessClassInPackage.com.sun.activation.registries
* java.lang.RuntimePermission getClassLoader
* java.lang.RuntimePermission setContextClassLoader
* java.lang.RuntimePermission setFactory
* java.security.SecurityPermission createPolicy.JavaPolicy
* java.security.SecurityPermission getPolicy
* java.security.SecurityPermission putProviderProperty.BC
* java.security.SecurityPermission setPolicy
* java.util.PropertyPermission * read,write
* java.util.PropertyPermission sun.nio.ch.bugLevel write
* javax.net.ssl.SSLPermission setHostnameVerifier
See http://docs.oracle.com/javase/8/docs/technotes/guides/security/permissions.html
for descriptions of what these permissions allow and the associated risks.

Continue with installation? [y/N]y
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@        WARNING: plugin forks a native controller        @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
This plugin launches a native controller that is not subject to the Java
security manager nor to system call filters.

Continue with installation? [y/N]y
-> Installed x-pack

$ /usr/share/kibana/bin/kibana-plugin install x-pack
Attempting to transfer from x-pack
Attempting to transfer from https://artifacts.elastic.co/downloads/kibana-plugins/x-pack/x-pack-5.5.0.zip
Transferring 119276235 bytes....................
Transfer complete
Retrieving metadata from plugin archive
Extracting plugin archive
Extraction complete
Optimizing and caching browser bundles...
Plugin installation complete
```
Now update your */etc/logstash/conf.d/logstash.conf* with two additional parameters for the elasticsearch output plugin. Common practice is to create a separate user for logstash, but in the current Introduction we use the default credentials.

```
user => "elastic"
password => "changeme"
```

### Additional X-Pack features

Now that you have installed X-Pack you can setup some nice example configurations for Graph, Alerting or even Machine Learning. All these examples are available at [Github](https://github.com/elastic/examples) and for convinience cloned to the elastic user home directory.



