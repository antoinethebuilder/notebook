# Quick Reference

## General

### List profiles

```
$ aws configure list

      Name                    Value             Type    Location
      ----                    -----             ----    --------
   profile                <not set>             None    None
access_key     ****************AAAA              sso
secret_key     ****************BBBB              sso
    region                us-east-1      config-file    ~/.aws/config
```

## System Manager

### Get inventory

Returns the available instances to connect to.

```bash
$ aws ssm get-inventory

{
    "Id": "i-39281321321321",
    "Data": {
        "AWS:InstanceInformation": {
            "TypeName": "AWS:InstanceInformation",
            "SchemaVersion": "1.0",
            "CaptureTime": "2020-04-04T18:24:17Z",
            "Content": [
                {
                    "AgentType": "amazon-ssm-agent",
                    "AgentVersion": "2.3.1550.0",
                    "ComputerName": "ip-111-11-222-44.ca-central-1.compute.internal",
                    "InstanceId": "i-39281321321321",
                    "IpAddress": "172.44.23.41",
                    "PlatformName": "CentOS Linux",
                    "PlatformType": "Linux",
                    "PlatformVersion": "7.6.2004",
                    "ResourceType": "EC2Instance"
                }
            ]
        }
    }
}
```



