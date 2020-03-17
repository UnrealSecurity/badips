# Bad IP blacklist
![badips-live-stat](https://i.ibb.co/r6FD9jt/image.png)

## Description
The site above is available [here](https://leet.network/badips) 
and connects my WebSocket API endpoint to receive alerts when 
automated systems scan our ports or attempt to exploit services binded to them.

## Related API endpoints
### GET
https://router.unrealsec.eu/badips
https://router.unrealsec.eu/badip
https://router.unrealsec.eu/127.0.0.1/badip
### POST
https://router.unrealsec.eu/bl
https://router.unrealsec.eu/bulk
### WebSocket
wss://leet.network/ws-live

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
