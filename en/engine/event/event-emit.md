# Listen to and launch events

## Listen to events

Event processing is done in the `cc.Node`. Components can register and monitor events by visiting the node this.node. Listen to events can be registered by the function `this.node.on()`. The methods are as follows:

```ts
// This event monitor is triggered every time and needs to be unregistered manually.
xxx.on(type, func, target?);
```

The `type` is the event registration string. `func` is the callback to listen to when the event is executed. And `target` is the event receive object. If `target` is not set，then `this` in the callback refers to the object that is currently executing the callback.

What's worth mentioning is that the event listener function `on` can pass to the third parameter target to bind the caller of the response function. The following two calling methods have the same effect:

```ts
// Using Function Binding
this.node.on(Node.EventType.MOUSE_DOWN, function ( event ) {
  this.enabled = false;
}.bind(this));

// Using the third parameter
this.node.on(Node.EventType.MOUSE_DOWN, (event) => {
  this.enabled = false;
}, this);
```

Besides listening with `on`, we can also use the `once` method. The `once` listener will shut the event being listened to after the listener function responds.

## Shut listener

We can shut the corresponding event listener using `off` when we don't care about a certain event anymore.

The `off` method can be used in two ways

```ts
// Cancel all registered events of this type on the object.
xxx.off(type);
// Cancels events on objects with this type of callback designation target.
xxx.off(type, func, target);
```

One thing to note is that the parameter of `off` must be in one-to-one correspondence with the parameter of `on` in order to shut it.

Below are what we recommend you to put in:

```ts
import { _decorator, Component, Node } from "cc";
const { ccclass } = _decorator;

@ccclass("Example")
export class Example extends Component {
    onEnable(){
        this.node.on('foobar', this._sayHello, this);
    }

    onDisable(){
        this.node.off('foobar', this._sayHello, this);
    }

    _sayHello(){
        console.log('Hello World');
    }
}
```

## Event Dispatches

There are two ways to trigger an event: `emit` and `dispatchEvent`. The difference between the two is that the latter can do event passing. Let's start with a simple example of the `emit` event.

```ts
// At most 5 args could be emit.
xxx.emit(type, ...args);
```

## Explanation for event arguments

When emitting event, We can start passing our event parameters at the second argument of the emit function. At the same time, the corresponding event parameters are available in the `on` registered callback.

```ts
import { _decorator, Component, Node } from "cc";
const { ccclass } = _decorator;

@ccclass("Example")
export class Example extends Component {
    onLoad () {
        this.node.on('foo', (arg1, arg2, arg3) => {
            console.log(arg1, arg2, arg3);  // print 1, 2, 3
        });
    }

    start () {
        let arg1 = 1, arg2 = 2, arg3 = 3;
        // At most 5 args could be emit.
        this.node.emit('foo', arg1, arg2, arg3);
  }
}
```

Note that only up to 5 event parameters can be passed here for the performance of the underlying event distribution. Therefore, care should be taken to control the number of parameters passed when passing a parameter.

## Event delivery

Events launched by the `dispatchEvent` method mentioned above would enter the event delivery stage. In Cocos Creator's event delivery system, we use bubble delivery. Bubble delivery will pass the event from the initiating node continually on to its parent node,  until the root node is reached or an interrupt `event.propagationStopped = true` is made in the response function of a node.

![bubble-event](bubble-event.png)

As shown in the picture above, when we send the event `“foobar”` from node c, if both node a and b listen to the event `“foobar”`, the event will pass to node b and a from c. For example:

```ts
// In the component script of node c
this.node.dispatchEvent( new Event.EventCustom('foobar', true) );
```

If we want to stop the event delivery after node b intercepts the event, we can call the function `event.propagationStopped = true` to do this. Detailed methods are as follows:

```ts
// In the component script of node b
this.node.on('foobar', (event: EventCustom) => {
  event.propagationStopped = true;
});
```

Be noted, when you want to dispatch a custom event, please do not use `cc.Event` because it's an abstract class, instead, you should use `cc.Event.EventCustom` to dispatch a custom event.

## Event object

In the call-back of the event listener, the developer will receive an event object event of the cc.Event type. `propagationStopped` is the standard API of cc.Event, other important API include:

| API name                 | type             | meaning             |
| :-------------             | :----------            |   :----------        |
| type           | String   | type of the event (event name)                      |
| target          | cc.Node | primary object received by the event                      |
| currentTarget          | cc.Node | current object receiving the event; current object of the event in the bubble stage may be different from the primary object                     |
| getType      | Function   | get the type of the event                   |
| propagationStopped   | Boolean   | stop the bubble stage, the event will no longer pass on to the parent node while the rest of the listeners of the current node will still receive the event                     |
| propagationImmediateStopped              | Boolean   | stop delivering the event. The event will not pass on to the parent node and the rest of the listeners of the current node。                      |
| detail             | Function | custom event information (belongs to cc.Event.EventCustom)    |
| setUserData             | Function | set custom event information (belongs to cc.Event.EventCustom)    |
| getUserData             | Function | get custom event information (belongs to cc.Event.EventCustom)    |

You can refer to the cc.Event and API files of its child category for a complete API list.

## System built-in event

Above are the general rules for listening to events and emitting events. Cocos Creator has built in system events. You can refer to the following documents:

[Node System Events](event-builtin.md)

[Global System Events](event-input.md)
