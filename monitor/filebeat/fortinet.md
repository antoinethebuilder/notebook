# Fortinet

## Ingesting from file

### Problem

I've had some issues setting up parsing the log using the `file` mode which typically ingests the exported log files you can get from the UI \(_default/text format_\).

The one thing I could not get my head around this section when looking at the `pipeline.yml` under Fortinet's module directory:

{% tabs %}
{% tab title="Module pipeline" %}
{% code title="module/fortinet/firewall/ingest/pipeline.yml" %}
```yaml
- grok:
    field: message
    patterns:
    - '%{SYSLOG5424PRI}%{GREEDYDATA:syslog5424_sd}$'
```
{% endcode %}
{% endtab %}

{% tab title="fortinet.yml" %}
```yaml
- module: fortinet
  firewall:
    enabled: true

    # Set which input to use between tcp, udp (default) or file.
    var.input: file
    var.paths: ["path/to/*.log", "path/to/my/other/files/*.log"]
```
{% endtab %}

{% tab title="Original logs" %}
```
date=2020-04-23 time=12:17:48 devname="testswitch1" devid="somerouterid" logid="0316013056" type="utm" subtype="webfilter" eventtype="ftgd_blk" level="warning" vd="root" eventtime=1587230269052907555 tz="-0500" policyid=100602 sessionid=1234 user="elasticuser" group="elasticgroup" authserver="elasticauth" srcip=192.168.2.1 srcport=61930 srcintf="port1" srcintfrole="lan" dstip=8.8.8.8 dstport=443 dstintf="wan1" dstintfrole="wan" proto=6 service="HTTPS" hostname="elastic.co" profile="elasticruleset" action="blocked" reqtype="direct" url="/config/" sentbyte=1152 rcvdbyte=1130 direction="outgoing" msg="URL belongs to a denied category in policy" method="domain" cat=76 catdesc="Internet Telephony"
date=2020-04-23 time=01:16:08 devname="testswitch1" devid="somerouterid" logid="0000000013" type="traffic" subtype="forward" level="notice" vd="OPERATIONAL" eventtime=1592961368 srcip=10.10.10.10 srcport=60899 srcintf="srcintfname" srcintfrole="lan" dstip=8.8.8.8 dstport=161 dstintf="dstintfname" dstintfrole="lan" sessionid=155313 proto=17 action="deny" policyid=0 policytype="policy" service="SNMP" dstcountry="Reserved" srccountry="Reserved" trandisp="noop" duration=0 sentbyte=0 rcvdbyte=0 sentpkt=0 appcat="unscanned" crscore=30 craction=131072 crlevel="high"
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
According [RFC5424](https://tools.ietf.org/html/rfc5424#section-6.2.1), the exported logs I had was missing the_`<PRIVAL>`_ which represents both the Facility and Priority value.

Here is an example of one the valid syslog messages to show the `<PRIVAL>`:

```text
<165>1 2003-08-24T05:14:15.000003-07:00 192.0.2.1 myproc 8710 - - %% It's time to make the do-nuts.
```
{% endhint %}

## Use the Grok Debugger

![](../../.gitbook/assets/image%20%282%29.png)

### Solution

My solution was to quickly modify the ingest pipeline to add a GROK pattern without the _PRIVAL_:

```bash
- grok:
    field: message
    patterns:
    - '%{SYSLOG5424PRI}%{GREEDYDATA:syslog5424_sd}$'
    - '%{GREEDYDATA:syslog5424_sd}$'
```

### Simulate the Ingest Pipeline

{% tabs %}
{% tab title="Request" %}
```bash
POST /_ingest/pipeline/filebeat-7.9.1-fortinet-firewall-pipeline/_simulate
{
  "docs": [
    {
      "_index": "index",
      "_id": "id",
    "_source": {
      "message": "date=2020-04-23 time=12:17:48 devname=\"testswitch1\" devid=\"somerouterid\" logid=\"0316013056\" type=\"utm\" subtype=\"webfilter\" eventtype=\"ftgd_blk\" level=\"warning\" vd=\"root\" eventtime=1587230269052907555 tz=\"-0500\" policyid=100602 sessionid=1234 user=\"elasticuser\" group=\"elasticgroup\" authserver=\"elasticauth\" srcip=192.168.2.1 srcport=61930 srcintf=\"port1\" srcintfrole=\"lan\" dstip=8.8.8.8 dstport=443 dstintf=\"wan1\" dstintfrole=\"wan\" proto=6 service=\"HTTPS\" hostname=\"elastic.co\" profile=\"elasticruleset\" action=\"blocked\" reqtype=\"direct\" url=\"/config/\" sentbyte=1152 rcvdbyte=1130 direction=\"outgoing\" msg=\"URL belongs to a denied category in policy\" method=\"domain\" cat=76 catdesc=\"Internet Telephony\""
    }
    }
  ]
}
```
{% endtab %}

{% tab title="Response" %}
```
{
  "docs" : [
    {
      "doc" : {
        "_index" : "index",
        "_type" : "_doc",
        "_id" : "id",
        "_source" : {
          "log" : {
            "level" : "warning"
          },
          "destination" : {
            "geo" : {
              "continent_name" : "North America",
              "location" : {
                "lon" : -97.822,
                "lat" : 37.751
              },
              "country_iso_code" : "US"
            },
            "as" : {
              "number" : 15169,
              "organization" : {
                "name" : "Google LLC"
              }
            },
            "port" : 443,
            "bytes" : 1130,
            "ip" : "8.8.8.8"
          },
          "rule" : {
            "ruleset" : "elasticruleset",
            "category" : "Internet Telephony",
            "id" : "100602"
          },
          "source" : {
            "port" : 61930,
            "user" : {
              "name" : "elasticuser",
              "group" : {
                "name" : "elasticgroup"
              }
            },
            "bytes" : 1152,
            "ip" : "192.168.2.1"
          },
          "message" : "URL belongs to a denied category in policy",
          "url" : {
            "path" : "/config/",
            "domain" : "elastic.co"
          },
          "network" : {
            "protocol" : "https",
            "bytes" : 2282,
            "iana_number" : "6",
            "direction" : "outgoing"
          },
          "observer" : {
            "ingress" : {
              "interface" : {
                "name" : "port1"
              }
            },
            "product" : "Fortigate",
            "vendor" : "Fortinet",
            "name" : "testswitch1",
            "serial_number" : "somerouterid",
            "type" : "firewall",
            "egress" : {
              "interface" : {
                "name" : "wan1"
              }
            }
          },
          "@timestamp" : "2020-04-23T12:17:48.000-05:00",
          "related" : {
            "user" : [
              "elasticuser"
            ],
            "ip" : [
              "192.168.2.1",
              "8.8.8.8"
            ]
          },
          "fortinet" : {
            "firewall" : {
              "dstintfrole" : "wan",
              "srcintfrole" : "lan",
              "method" : "domain",
              "subtype" : "webfilter",
              "authserver" : "elasticauth",
              "reqtype" : "direct",
              "cat" : "76",
              "action" : "blocked",
              "sessionid" : "1234",
              "type" : "utm",
              "vd" : "root"
            }
          },
          "event" : {
            "code" : "0316013056",
            "timezone" : "-0500",
            "kind" : "event",
            "module" : "fortinet",
            "start" : "2020-04-18T12:17:49.052-05:00",
            "action" : "ftgd_blk",
            "type" : [
              "denied"
            ],
            "category" : [
              "network"
            ],
            "dataset" : "fortinet.firewall",
            "outcome" : "success"
          }
        },
        "_ingest" : {
          "timestamp" : "2020-09-17T20:35:59.781293Z"
        }
      }
    }
  ]
}

```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**PLEASE NOTE**: The logs can be different from the example used in this article.

You should also be informed that not everything is going to be parsed correctly. Some data coming from the event category did not get to be successfully parsed. Perhaps, the most important logs to me did get successfully parsed.
{% endhint %}

## Reference

[Fortinet module](https://www.elastic.co/guide/en/beats/filebeat/7.9/filebeat-module-fortinet.html)  
[Fortinet expected logs from the official module repository](https://github.com/elastic/beats/blob/829c3b7dcc6365161d83a3b10f05a9f9990f36c3/x-pack/filebeat/module/fortinet/firewall/test/fortinet.log)

