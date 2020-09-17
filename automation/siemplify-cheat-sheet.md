# Siemplify - API Endpoints

## Description

The Siemplify API is already documented but I wanted to write down a couple of queries that I like to use.

Some of these uses `jq` which parses the JSON from the request's response.

## Playbooks

### Get Playbooks

{% tabs %}
{% tab title="Request" %}
```bash
curl -k --request POST --url \
'https://{{baseUrl}}/api/external/v1/playbooks/GetWorkflowMenuCards?format=camel' \
--header 'appkey: $APITOKEN' -k --header 'content-type: application/json' \
--data '[1,0]' | jq '.[]|{identifier: .identifier, name: .name}'
```
{% endtab %}

{% tab title="Response" %}
```
{
    "identifier": "29a4da6c-dd94-47e7-a90b-8c8e78f11111",
    "name": "enrichment_playbook"
},
{
    "identifier": "29a4da6c-dd94-47e7-a90b-8c8e78f22222",
    "name": "enrichment_playbook2"
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If you want to only get the disabled playbooks, change the requested data to: `--data '[0]'`
{% endhint %}

{% hint style="warning" %}
This is endpoint is not officially documented in the collection.
{% endhint %}

### Get Playbook Folders

The Siemplify term for the folders is "categories"

{% tabs %}
{% tab title="Request" %}
```bash
curl --request GET --url \
https://{{baseUrl}}/api/external/api/external/v1/playbooks/GetWorkflowCategories \
--header 'appkey: $APITOKEN' -k | jq '.[]|{name: .name, id: .id}'
```
{% endtab %}

{% tab title="Response" %}
```
{
  "name": "Imported Playbooks",
  "id": 1
}
{
  "name": "Default",
  "id": 2
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
You can change the jq query to only retrieve the id using `'.[]|.id'`
{% endhint %}

### Get a Playbook's Identifier

{% tabs %}
{% tab title="Request" %}
```bash
curl --request POST --url \
'https://{{baseUrl}}//api/external/v1/playbooks/CheckWorkflowNameInDifferentEnvironments' \
--header 'appkey: $APITOKEN' \
--header 'content-type: application/json' \
--data '{"wfName": "PlaybookName"}'
```
{% endtab %}

{% tab title="Response" %}
```
"29a4da6c-dd94-47e7-a90b-8c8e78f11111"
```
{% endtab %}
{% endtabs %}

### Duplicate Playbooks

```bash
curl --location --request POST \
'https://{{baseUrl}}/api/external/v1/playbooks/DuplicateWorkflows' \
--header 'AppKey: $APITOKEN' \
--data-raw '{
    "priority": 0,
    "categoryId": 10,
    "environments": [
        "Default"
    ],
    "identifiers": [
        "29a4da6c-dd94-47e7-a90b-8c8e78f11111",
        "29a4da6c-dd94-47e7-a90b-8c8e78f11111"
    ]
}'
```

### Delete Playbooks

{% tabs %}
{% tab title="Request" %}
```bash
curl --request POST \
  --url https://{{baseUrl}}//api/external/v1/playbooks/DeleteWorkflows \
  --header 'appkey: $APITOKEN' \
  --header 'content-type: application/json' \
  --data '{
    "identifiers": [
        "412ec3dd-1111-1111-1111-11111111111c",
        "ea6a2c9e-1111-1111-1111-11111111111c",
				"fdd31108-1111-1111-1111-11111111111c",
				"668057f7-1111-1111-1111-11111111111c",
				"bbb63896-1111-1111-1111-11111111111c"
    ]
}'
```
{% endtab %}

{% tab title="Response" %}
```
{      
  "results": [
    {
      "identifier": "412ec3dd-1111-1111-1111-11111111111c",
      "status": 0,
      "failed": false,
      "errorMessage": null
    },
    {
      "identifier": "ea6a2c9e-1111-1111-1111-11111111111c",
      "status": 0,
      "failed": false,
      "errorMessage": null
    },
    {
      "identifier": "fdd31108-1111-1111-1111-11111111111c",
      "status": 0,
      "failed": false,
      "errorMessage": null},
    {"identifier": "668057f7-1111-1111-1111-11111111111c",
      "status": 0,
      "failed": false,
      "errorMessage": null
      },
    {
      "identifier": "fdd31108-1111-1111-1111-11111111111c",
      "status": 0,
      "failed": false,
      "errorMessage": null
      }
    ]
}
```
{% endtab %}
{% endtabs %}

## Cases

### Get Alerts

{% tabs %}
{% tab title="Request" %}
```bash
curl --request GET --url \
https://{{baseUrl}}/api/external/v1/cases/GetCaseFullDetails/$ID \
--header 'appkey: $APITOKEN' -k | jq '.alerts[]|.additionalProperties'
```
{% endtab %}
{% endtabs %}

## Reference

[Siemplify API - Postman Collection](https://api.siemplify.co/)

