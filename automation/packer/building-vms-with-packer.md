# Building Vagrant Boxes

## Description

This explains how to create reusable VMs using Packer and Vagrant. It is possible to create virtual machine from scratch but taking existent vagrant boxes from a trustable repository speeds up the process a little bit.

{% hint style="info" %}
This example take into considerations you have learned the very basic of Packer and Vagrant.

**Most of the scripts** that were used in this article are available [here](https://github.com/chef/bento/tree/master/packer_templates/_common).  
You can also visit this section of the [Chef Bento](https://github.com/chef/bento/tree/master/packer_templates) repository which contains various templates.
{% endhint %}

### Requirements

* [Packer](https://www.packer.io/)
* [Vagrant](https://www.vagrantup.com/)
* [Virtualbox ](https://www.virtualbox.org/wiki/Downloads)\(_only for this example_\)

## Template

This is a basic template that pulls a CentOS base box from Vagrant so we can customize the image and upload our version of this base box to Vagrant.

{% hint style="info" %}
If you are unsure about the source of this image, please refer to the Official repository of the CentOS project on [Vagrant Cloud](https://app.vagrantup.com/centos/boxes/7) or even on the official website of [CentOS](https://www.centos.org/download/).
{% endhint %}

{% hint style="warning" %}
If you ever want to use another vagrant box that you found somewhere else whether it is on Vagrant Cloud or not, **always be careful about the source and the author of the box.**
{% endhint %}

{% code title="centos-template.json" %}
    {
      "variables": {
        "centos_version": "7.5",
        "box_version": "1809.01",
        "box_name": "centos",
        "username": "your-username"
      },
      "provisioners": [
        {
          "type": "shell",
          "execute_command": "echo 'vagrant' | {{.Vars}} sudo -S -E sh -eux '{{.Path}}'",
          "expect_disconnect": true,
          "scripts": [
            "{{template_dir}}/scripts/banner.sh",
            "{{template_dir}}/scripts/networking.sh",
            "{{template_dir}}/scripts/vagrant.sh",
            "{{template_dir}}/scripts/virtualbox.sh",
            "{{template_dir}}/scripts/vmware.sh",
            "{{template_dir}}/scripts/cleanup.sh",
            "{{template_dir}}/scripts/minimize.sh"
          ]
        }
      ],
      "builders": [
        {
          "communicator": "ssh",
          "source_path": "centos/7",
          "provider": "virtualbox",
          "box_version": "{{ user `box_version` }}",
          "add_force": true,
          "type": "vagrant"
        }
      ],
      "post-processors": [
        {
          "type": "vagrant-cloud",
          "box_tag": "{{ user `username` }}/{{ user `box_name` }}",
          "access_token": "{{ user `cloud_token` }}",
          "version": "{{ user `centos_version` }}",
          "keep_input_artifact": false
        }
      ]
    }
{% endcode %}

### Build

{% hint style="info" %}
Before actually building this box, you should read this [section ](https://www.packer.io/docs/post-processors/vagrant-cloud#workflow)of the Packer documentation regarding the Vagrant Cloud post-processor since the "Vagrant Cloud Box" needs to exist for packer to upload it to the cloud.

> The Vagrant Cloud box \(`hashicorp/foobar` in this example\) must already exist. Packer will not create the box automatically. If running Packer in automation, consider using the [Vagrant Cloud API](https://www.vagrantup.com/docs/vagrant-cloud/api.html) to create the Vagrant Cloud box if it doesn't already exist.
{% endhint %}

To build this image, simply run the following command:

{% tabs %}
{% tab title="Bash" %}
```bash
$ packer build -var-file="secrets.json" centos.json
```
{% endtab %}

{% tab title="Secrets" %}
{% code title="secrets.json" %}
```
{
    "cloud_token": "my-vagrant-token"
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Using the box

Now you can use your vagrant box by running:

```text
$ vagrant init username/boxname
$ vagrant up
```

Vagrant will automatically use the provider the image was created with.

