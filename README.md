# ansible-ipwcli_dns
Manage DNS Records for Ericsson IPWorks via ipwcli

Manage DNS records for the Ericsson IPWorks DNS server. The module will use the ipwcli to deploy the DNS records.

## Parameters

| Parameter | Type | required | Choices/Defaults |  Comments |
| --------- | ---- |-------- | ----------------- |  -------- |
| address      | string   |          |                                        |	The IP address for the A or AAAA record. Required for `type=A` or `type=AAAA`		|
| container    | string   | required |                                        |	Sets the container zone for the record.			|
| dnsname      | string   | required |                                        |	Name of the record.			|
| flags        | string   |          | Choices: S,A,U,P                       |	Sets one of the possible flags of NAPTR record.			|
| order        | integer   |         |                                        |	Sets the order of the NAPTR record. Required for `type=NAPTR`		|
| password     | string   | required |                                        |	Password to login on ipwcli.			|
| port         | integer   |         |                                        |	Sets the port of the SRV record. Required for `type=SRV`			|
| preference   | integer   |         |                                        |	Sets the preference of the NAPTR record. Required for `type=NAPTR`			|
| priority     | integer   |         | Default: 10                            |	Sets the priority of the SRV record.			|
| replacement  | string   |          |                                        |	Sets the type of the NAPTR record. Required for `type=NAPTR`			|
| service      | string   |          |                                        |	Sets the type of the record. Required for `type=NAPTR`			|
| state        | string   |          | Choices: absent, present (<- default)  |	Whether the record should exist or not.			|
| target       | string   |          |                                        |	Sets the target of the SRV record. Required for `type=SRV`			|
| ttl          | integer   |         | Default: 3600                          |	Sets the ttl of the record.			|
| type         | string   | required | Choices: NAPTR, SRV, A, AAAA           |	Type of the record.			|
| user         | string   | required |                                        |	Username to login on ipwcli.			|
| weight       | integer   |         | Default: 10                            |	Sets the weight of the SRV record.			|

## Notes

- To make the DNS record change affective, you need to run ‘update dnsserver’ on the ipwcli.

## Examples

### Create A record
```yaml
- name: Create A record
  ipwcli_dns:
    dnsname: example.com
    type: A
    container: ZoneOne
    address: 127.0.0.1
```
### Remove SRV record if exists
```yaml
- name: Remove SRV record if exists
  ipwcli_dns:
    dnsname: _sip._tcp.test.example.com
    type: SRV
    container: ZoneOne
    ttl: 100
    state: absent
    target: example.com
    port: 5060
```
### Create NAPTR record
```yaml
- name: Create NAPTR record
  ipwcli_dns:
    dnsname: test.example.com
    type: NAPTR
    preference: 10
    container: ZoneOne
    ttl: 100
    order: 10
    service: 'SIP+D2T'
    replacement: '_sip._tcp.test.example.com.'
    flags: S
```

## Return Values

| Key | Type |  Returned | Description | 
| --- | ---- | --------- | ----------- |
| record |  string | always | The created record from the input params   |

## Authors
Christian Wollinger [(@cwollinger)](https://github.com/CWollinger)