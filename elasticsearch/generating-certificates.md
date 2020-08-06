# Generating certificates

## Transport between nodes

Using the PKCS\#12 keystore, it will include the node certificate, node key, and CA certificate.

### Silent mode using YAML

{% code title="instances.yml" %}
```
instances:
  - name: "node1"
    ip: 
      - "192.0.2.1"
    dns: 
      - "node1.mydomain.com"
  - name: "node2"
    ip:
      - "192.0.2.2"
      - "198.51.100.1"
  - name: "node3"
  - name: "node4"
    dns:
      - "node4.mydomain.com"
      - "node4.internal"
  - name: "CN=node5,OU=IT,DC=mydomain,DC=com"
    filename: "node5"
```
{% endcode %}

{% tabs %}
{% tab title="Certificates" %}
```bash
bin/elasticsearch-certutil cert --silent --in instances.yml --out certs.zip
```
{% endtab %}

{% tab title="Add password to keystore" %}
```bash
bin/elasticsearch-keystore add xpack.security.transport.ssl.keystore.secure_password
bin/elasticsearch-keystore add xpack.security.transport.ssl.truststore.secure_password
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
 You will be asked to enter a password for each instances, it is highly recommended.

The PKCS\#12 keystore will include the node certificate, node key, and CA certificate.
{% endhint %}

### Configuration

Add the following lines to your configuration file.

{% code title="elasticsearch.yml" %}
```bash
xpack.security.transport.ssl.enabled: true
xpack.security.transport.ssl.verification_mode: certificate 
xpack.security.transport.ssl.keystore.path: /etc/elasticsearch/certs/node-1.p12 
xpack.security.transport.ssl.truststore.path: /etc/elasticsearch/certs/node-1.p12
```
{% endcode %}



## HTTP

```text
bin/elasticsearch-certutil http
```

{% hint style="info" %}
This command will guide you through to create your HTTP certificate\(s\).

It will output the configuration for elasticsearch and kibana with a README.txt for each nodes.
{% endhint %}

