# Siemplify - Cheat Sheet

## Using CURL and JQ

### Get Alerts

```bash
curl --request GET --url \
https://{{baseUrl}}/api/external/v1/cases/GetCaseFullDetails/$ID \
--header 'appkey: $APITOKEN' -k | jq '.alerts[]|.additionalProperties'
```



