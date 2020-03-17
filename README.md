# Bad IP blacklist
![badips-live-stat](https://i.ibb.co/r6FD9jt/image.png)

## Description
The site above is available [here](https://leet.network/badips) 
and connects our WebSocket API endpoint to receive alerts when 
automated systems scan our ports and/or attempt to exploit services binded to them.
- *I do NOT want to start ratelimiting people or blocking access from certain IP addresses so please reduce your requests to very minimum.*

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
