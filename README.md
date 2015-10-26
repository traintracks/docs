# Sending events to Traintracks

### API Configuration Basics

Traintracks has a simple API endpoint for your client to send events to.

To send, you will:

1. Assemble a JSON array of JSON objects, each of which represent one event.
2. HTTP POST this JSON array to your friendly neighborhood traintracks endpoint (ex: https://api.traintracks.io/v1/events).
3. The HTTP headers should contain "Remote-Address: 1.2.3.4” to associate an IP Address with your event batch (1.2.3.4 will do if you don’t have that information).
4. The HTTP headers should also contain two more headers:
  * X-Product-Key 
    * the UUID1 string that was provided to you by Traintracks for your product
    * ex: "a32e6578-17b7-11e4-930a-bc764e1c05e4"
  * X-Product-Auth
    * an MD5 hash of your json body concatenated with your product secret
    * ex: md5(json body + productSecret) = "44d9dbb60b6c2c24922cd62d249412f9"

### HTTP Body Format

Our format is a JSON array of JSON objects. Each JSON object should have the following attributes:

1. build
  * a string that represents the build version of your product
  * ex: "1.1.1"
2. clientTimestamp   
  * a string that represents the timestamp at which the event was sent in UTC
  * should be formatted as: "YYYY-MM-DD'T'HH:mm:ss.SSSSSSZZ"
  * ex: “2012–03–14T02:33:42.416587+00:00”
3. data
  * a JSON array of JSON objects, each of which represents a single event from your client of any format.
  * ex:
  ```javascript
  {
    "data": {
      "winner": {
        "name": "JaneDoe",
        "experiencePoints": "500",
        "weapon": "Ragnarok",
        "money": 2350
      },
      "loser": {
        "name": "Action888",
        "experiencePoints": "200",
        "weapon": "Buster Sword",
        "money": 1200
      },
      "battleTime": 52030
    }
  }
  ```
4. device
  * a string that represents the device type
  * ex: "iPhone 4S"
5. eventType
  * a string that represents the event type. Please refrain from using periods in the string.
  * ex: "store_purchase"
6. userId
  * a UUID1 string that uniquely identifies the user within your product
  * ex: "3dc6b97d-7388-11e4-bfb3-b8e8563b3f9a"
7. userName
  * a userName string that uniquely identifies the user
  * ex: "typicalGamer2000"
8. sessionId
  * a UUID1 string that represents the current session of the user
  * ex: "fff324c6-30a2-11e3-ad80-485d60066bda"
9. serverTimestamp (optional)
  * this field is optional; same format as clientTimestamp 
  * use this only when resending data from the past. For real-time events Traintracks will automatically set this on event arrival. 
  * should be formatted as: "YYYY-MM-DD'T'HH:mm:ss.SSSSSSZZ"
  * ex: “2012–03–14T02:33:42.416587+00:00”
10. location (optional)
  * this field is optional
  * use this only for geoqueries
  * should be formatted as an object with latitude and longitude numeric values
  * ex: 
  ```javascript
  {
    "location": {
      "lat": 37.871984,
      "lon": -122.259420
    }
  }
  ```  

### Example

Here is an example of a JSON array batch of 2 events you can send to our API:

```javascript
[
  {
    "build": "1.1.1",
    "clientTimestamp": "2012-03-14T02:33:42.416587-07:00",
    "data": {
      "winner": {
        "name": "JaneDoe",
        "experiencePoints": "500",
        "weapon": "Ragnarok",
        "money": 2350
      },
      "loser": {
        "name": "Action888",
        "experiencePoints": "200",
        "weapon": "Buster Sword",
        "money": 1200
      },
      "battleTime": 52030
    },
    "device": "iPhone 5S",
    "eventType": "sword_battle",
    "userId": "3dc6b97d-7388-11e4-bfb3-b8e8563b3f9a",
    "userName": "JaneDoe",
    "sessionId": "fff324c6-30a2-11e3-ad80-485d60066bda",
    "location": {
      "lat": 37.871984,
      "lon": -122.259420
    }
  },
  {
    "build": "1.1.1",
    "clientTimestamp": "2012-03-14T02:43:43.416587-07:00",
    "data": {
      "user": {
        "experiencePoints": 500,
        "money": 2326
      },
      "item": {
        "name": "Excalibur",
        "cost": 24,
        "type": "sword"
      }
    },
    "device": "iPhone 5S",
    "eventType": "store_purchase",
    "userId": "3dc6b97d-7388-11e4-bfb3-b8e8563b3f9a",
    "userName": "JaneDoe",
    "sessionId": "fff324c6-30a2-11e3-ad80-485d60066bda",
    "location": {
      "lat": 37.871984,
      "lon": -122.259420
    }
  }
]
```

### Javascript SDK

We currently have a Javascript sdk available to facilitate the sending of events to our api: <a target="_blank" href="https://github.com/traintracks/javascript-sdk">https://github.com/traintracks/javascript-sdk</a>
