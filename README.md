# Bad IP blacklist
![badips-live-stat](https://i.ibb.co/r6FD9jt/image.png)

## Description
The site above is available [here](https://leet.network/badips) 
and connects to our WebSocket API endpoint to receive alerts when 
automated systems scan our ports and/or attempt to exploit services binded to them. 
Recently we began sending automated abuse reports to medium/large hosting companies via email when there is suspicious traffic detected from their networks to our networks. Every address has 30 minutes cooldown in attempt to prevent email spam.
```diff
-  I do NOT want to start ratelimiting people or blocking access from certain 
-  IP addresses so please reduce your requests to very minimum.
```

## Related API endpoints
### GET
- https://router.unrealsec.eu/badips (list of all unique IP addresses blacklisted so far and it's a lot)
- https://router.unrealsec.eu/badip (check if the request sender's address is on our blacklist)
- https://router.unrealsec.eu/127.0.0.1/badip (check if specified address is on our blacklist)
### POST
- https://router.unrealsec.eu/bl (request IP blacklisting, whitelisted addresses only)
- https://router.unrealsec.eu/bulk (geo information and blacklist status for list of addresses separated by newline character)
### WebSocket
- wss://leet.network/ws-live

## Examples
### WebSocket
#### Receive instant alerts about port activity
```javascript
const WebSocket = require('ws');
var ws = new WebSocket('wss://leet.network/ws-live');

ws.on('open', ()=>{
   console.log('WebSocket connected');
});

ws.on('close', ()=>{
   console.log('WebSocket disconnected');
});

ws.on('message', (data)=>{
   data = JSON.parse(data);
   console.log(data);
});
```
```javascript
{
  address: '51.89.134.150',
  port: 3389,
  service: 'rdp',
  hostname: 'ip150.ip-51-89-134.eu',
  time: 1584482657,
  ipv: 4,
  country: { code: 'gb', name: 'United Kingdom' },
  tor: false,
  new: false,
  nick: '64bdd4'
}
{
  address: '218.92.0.190',
  port: 22,
  service: 'ssh',
  hostname: '',
  time: 1584482653,
  ipv: 4,
  country: { code: 'cn', name: 'China' },
  tor: false,
  new: false,
  nick: '0041cb'
}
{
  address: '92.114.17.214',
  port: 23,
  service: 'telnet',
  hostname: '214.mobinnet.net',
  time: 1584482659,
  ipv: 4,
  country: { code: 'ir', name: 'Iran' },
  tor: false,
  new: false,
  nick: '8ca1de'
}
```
**Optional fields that aren't sent if they're empty**
- service ``string``
- description ``string``
- tags ``string[]``

### Bulk
#### Check multiple IP addresses separated by newline character and return JSON
```bash
curl -s --data-binary @list.txt https://router.unrealsec.eu/bulk
```
```json
{
  "146.90.171.2": {
    "country": {
      "code": "gb",
      "name": "United Kingdom"
    },
    "tor": false,
    "listed": false
  },
  "90.255.146.115": {
    "country": {
      "code": "gb",
      "name": "United Kingdom"
    },
    "tor": false,
    "listed": false
  }
}
```
