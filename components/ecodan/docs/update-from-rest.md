# Using direct REST API
The esphome component will expose a REST API for the thermostats. You can use GET/POST to access/modify the current temperature. The entities will be named `room_0`, `room_1` and so on.

### Get data 
```bash
curl -X GET "http://<esp_id>/number/room_0"
# returns
{"id":"number-room_0","value":"21.5","state":"21.5"}
```

### Post data
```bash
curl -X POST "http://<esp_ip>/number/room_0/set?value=22.5" -d ""
```