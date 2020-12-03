# Global and Node Touch and Mouse Events API

## Mouse Event API

| Function name                 | Returns value type             | Meaning             |
| :-------------             | :----------            |   :----------        |
| getScrollY      |  Number             | get the y axis distance wheel scrolled, effective only when scrolling                      |
| getButton       | Number | cc.Event.EventMouse.BUTTON_LEFT or cc.Event.EventMouse.BUTTON_RIGHT or cc.Event.EventMouse.BUTTON_MIDDLE。                     ｜

### Global Mouse Events API

| Function name                 | Returns value type             | Meaning             |
| :-------------             | :----------            |   :----------        |
| getLocation          | Vec2     |    get mouse location object which includes x and y properties   |
| getLocationX         | Number   | get x-axis location of the mouse                     |
| getLocationY         | Number   | get y-axis location of the mouse                      |
| getPreviousLocation  | Vec2     | gets the position object when the mouse event was last triggered, with x and y attributes  |
| getDelta             | Vec2     | get the distance object the mouse moves since last event, which includes x and y properties    |
| getDeltaX             | Number | get x-axis delta of the mouse                       |
| getDeltaY             | Number | get y-axis delta of the mouse    |

### Node Mouse Events API

| Function name                 | Returns value type             | Meaning             |
| :-------------             | :----------            |   :----------        |
| getUILocation           | Vec2   | get mouse location object relative to the lower-left corner of the UI window, and the object contains x and y attributes                      |
| getUILocationX          | Number | get x-axis location relative to the lower-left corner of the UI window of the mouse                      |
| getUILocationY          | Number | get y-axis location relative to the lower-left corner of the UI window of the mouse                      |
| getUIPreviousLocation   | Vec2   | get the location of the last mouse relative to the lower-left corner within the UI window, and the object contains x and y attributes                      |
| getUIDelta              | Vec2   | get the mouse distance object with x and y properties from the last event movement in the UI coordinate system                      |
| getUIDeltaX             | Number | gets the x-axis distance of the current mouse relative to the lower-left corner of the UI window from the last mouse move                      |
| getUIDeltaY             | Number | get the y-axis distance of the current mouse relative to the lower-left corner of the UI window from the last mouse    |

## Touch Events API

| API name                 | type             | Meaning             |
| :-------------             | :----------            |   :----------        |
| touch | Touch | contact object related to the current event                     |
| getID | Number | identification ID of the touch spot, can be used in multi-touch to track the touch spot                      |

### Global Touch Events API

| Function name                 | Returns value type             | Meaning             |
| :-------------             | :----------            |   :----------        |
| getLocation          | Vec2     |    get location object of the touch spot which includes x and y propertites   |
| getLocationX         | Number   | get x-axis location of the touch spot                      |
| getLocationY         | Number   | get y-axis location of the touch spot                      |
| getStartLocation | Vec2 | get the location object the where touch spot gets down which includes x and y properties                      |
| getPreviousLocation  | Vec2     | get the location object of the touch spot at the last event which includes x and y properties  |
| getDelta             | Vec2     | get the distance object the touch spot moves since the last event, which includes x and y properties    |

### Node Touch Events API

| Function name                 | Returns value type             | Meaning             |
| :-------------             | :----------            |   :----------        |
| getUILocation           | Vec2   | get mouse location object relative to the lower-left corner of the UI window, and the object contains x and y attributes                      |
| getUILocationX          | Number | get x-axis location relative to the lower-left corner of the UI window of the touch                      |
| getUILocationY          | Number | get x-axis location relative to the lower-left corner of the UI window of the touch                       |
| getUIStartLocation      | Vec2   | get the location of the initial point of contact within the UI window relative to the lower-left corner of the object, which contains the x and y attributes                      |
| getUIPreviousLocation   | Vec2   | get the location of the last touch relative to the lower-left corner within the UI window, and the object contains x and y attributes.                      |
| getUIDelta              | Vec2   | get the touch distance object with x and y properties from the last event movement in the UI coordinate system                      |
