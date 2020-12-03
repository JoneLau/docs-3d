# Global System Events

In this section, we will introduce the global system events of Cocos Creator.

Global system events are irrelevant with the node hierarchy, so they are dispatched globally by `cc.systemEvent`, currently supported:

- Mouse
- Touch
- Keyboard
- DeviceMotion

The mouse event and the touch event are similar to the node system events, except that the area of action is different. The following is a description of these events.

## The difference between node events and global mouse and touch events

Before we begin this section, I hope you'll read up on [Auto fit for multi-resolution](../../ui-system/components/engine/multi-resolution.md#Design-resolution-and-screen-resolution) and  understand the screen area and UI display area. When listening for global mouse/touch events, the acquired contacts are calculated based on the bottom left corner of the screen area (device display resolution). The contacts fetched by the UI node listener are not the same as the contacts fetched by the global event, which are converted to the points calculated in the lower left corner of the adapted UI viewable area. Global touch points are better suited for manipulating the behavior of 3D nodes by tapping directly on the screen, without having to add UI nodes to the scene to listen for mouse/touch events.

## How to define the input events

You can use `cc.systemEvent.on(type, callback, target)` to register Keyboard and DeviceMotion event listeners.

Event types included:

1. cc.SystemEventType.KEY_DOWN
2. cc.SystemEventType.KEY_UP
3. cc.SystemEventType.DEVICEMOTION

### Keyboard Events

- Type: `cc.SystemEventType.KEY_DOWN` and `cc.SystemEventType.KEY_UP`
- Call Back:
    - Custom Event：callback(event);
- Call Back Parameter:
    - KeyCode: [API Reference](https://docs.cocos.com/creator3d/api/en/classes/event.eventkeyboard-1.html)
    - Event: [API Reference](https://docs.cocos.com/creator3d/api/en/classes/event.event-1.html)

```ts
import { _decorator, Component, Node, systemEvent, SystemEventType, EventMouse, macro } from "cc";
const { ccclass } = _decorator;

@ccclass("Example")
export class Example extends Component {
    onLoad () {
        systemEvent.on(SystemEventType.KEY_DOWN, this.onKeyDown, this);
        systemEvent.on(SystemEventType.KEY_UP, this.onKeyUp, this);
    }

    onDestroy () {
        systemEvent.off(SystemEventType.KEY_DOWN, this.onKeyDown, this);
        systemEvent.off(SystemEventType.KEY_UP, this.onKeyUp, this);
    }

    onKeyDown (event: EventMouse) {
        switch(event.keyCode) {
            case macro.KEY.a:
                console.log('Press a key');
                break;
        }
    }

    onKeyUp (event: EventMouse) {
        switch(event.keyCode) {
            case macro.KEY.a:
                console.log('Release a key');
                break;
        }
    }
}
```

### DEVICE MOTION

- Type: `cc.SystemEventType.DEVICEMOTION`
- Call Back:
    - Custom Event: `callback(event);`;
- Call Back Parameter:
    - Event: [API Reference](https://docs.cocos.com/creator3d/api/en/classes/event.event-1.html)

```ts
import { _decorator, Component, Node, systemEvent, SystemEventType, EventMouse, macro, log } from "cc";
const { ccclass } = _decorator;

@ccclass("Example")
export class Example extends Component {
    onLoad(){
        systemEvent.setAccelerometerEnabled(true);
        systemEvent.on(SystemEventType.DEVICEMOTION, this.onDeviceMotionEvent, this);
    }

    onDestroy () {
        systemEvent.off(SystemEventType.DEVICEMOTION, this.onDeviceMotionEvent, this);
    }

    onDeviceMotionEvent (event: EventAcceleration) {
        log(event.acc.x + "   " + event.acc.y);
    }
}
```

You can go to [test-cases-3d](https://github.com/cocos-creator/test-cases-3d/tree/master/assets/cases/event)（This includes the keyboard, accelerometer, single point touch, multitouch examples）。
