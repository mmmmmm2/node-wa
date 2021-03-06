# Node.js Whatsapp API Client

Loosely based on the WhatsAPI and Wazapp projects that you can find on github. Appears to work in all versions of Node from 0.2 to 0.8.

Usage example:

```js
var waApi = require('node-wa');
var wa = new waApi(userId, password, { displayName: 'Blade', debug: true });
```

Your userId is the Phone Number that you have registered with WA, and your password should be either your device's MAC ID (for iOS) duplicated then MD5'd, such as md5("AA:BB:CC:DD:EE:FFAA:BB:CC:DD:EE:FF"), or the reverse of your device's IMEI number, then MD5'd, such as md5(strrev(device imei)) for all other WA systems besides iOS.

It is an EventEmitter that will emit several events when things happen. You can call different functions within it to cause different things to happen.

At this time, if someone sends you custom image data, it'll probably crash out, as it has no idea how to parse it.


### Events

* close: emitted when the connection to the server is closed
* loggedin: emitted when the client is actually logged into the server
* presence: emits { from: "userid-who-sent", type: "[available|unavailable]" } when a user presence is received
* streamError: emits a ProtocolTreeNode describing the received error
* msgReceived: emis { from: "userid-who-received", id: "messageId" } when a message-received message is received (ie, when the system tells you that another user has received a message you sent)
* composing: emitted when we receive notification that a user is typing, contains just the userid who is typing
* paused: emitted when we receive notification that a user has stopped typign, contains just the userid 
* message: emitted when we receive a full message, the entire ProtocolTreeNode is sent
* serverAccept: emits { from: "userid-who-message-was-sent-to", id: "messageId" } when a message is accepted by the server (msgReceived should be emitted when the actual recipient has received the message)
* message: also emitted when we receive something that looks like a message, but we have no idea how to parse it internally
* connectionReplaced: when another WA client logs into your account, you'll receive this event
* media: emits a bunch of information about media received - theoretically this works, but realistically, it does not, as media includes all sorts of data that doesn't fit into a single packet, so right now the packet reader blows up on it.


### Functions

* sendTyping(jid) - inform server that you are typing
* sendPaused(jid) - inform server that you are not typing
* getLastOnline(jid) - ask the server to tell us when someone was last seen
* sendAvailable() - inform server we are available
* sendAvailableForChat() - inform server we are available for chat (WA doesn't seem to care about that setting though)
* sendUnavailable() - inform server we are not available
* sendMessageWithBody({ content: "Message Content", to: "userid to send to" }) - send a message
* sendStatusUpdate(message) - update your custom status setting on the WA server - clients need to query this, somehow or other. I haven't yet located any way to subscribe to be automatically notified on status changes.
* sendMessageWithMedia() - not implemented yet
* sendGetStatus(jid) - should query the server for a status update from a user
* sendNotificationReceived(jid, msgid) - untested
* sendPresenceSubscriptionRequest(jid) - untested
* sendRelayComplete(id, milliseconds) - untested
* sendActive() - set status to Active - not sure how this differs from Available
* sendInactive() - set status to Inactive - not sure how this differs from Unavailable
* sendRelayTimeout - untested
* sendUnsubscribeMe - untested
* sendUnsunscribeHim - untested
* sendDeleteFromRoster - untested
* sendClose - currently mirrors sendUnavailable, should also inform the server we are closing down the connection
* setGetPrivacyList() - request a "Privacy List". I believe this is a list of JIDs that are blocked from sending to us
* sendSetPrivacyBlockedList([ jid array]) - send a list of users to set in Privacy List

### Author & Credits
* Original Author - https://github.com/ericblade
* Thanks to vkotovv for the fix for login issues Aug 29!
