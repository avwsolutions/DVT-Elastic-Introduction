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

## Introduction tutorial starts here

> Note : Elastic recommends using Oracle Java over OpenJDK. In this tutorial we will use OpenJDK which also is supported.

Below you will find the commands for installing the openJDK JRE and checking if the path & version (1.8 or greater) are correct. 

```
$ sudo yum -y install jre
$ sudo java -version
openjdk version "1.8.0_91"
OpenJDK Runtime Environment (build 1.8.0_91-b14)
OpenJDK 64-Bit Server VM (build 25.91-b14, mixed mode)
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








