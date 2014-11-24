Channels
========

Overview
--------
- One-way, asynchronous, persistent communication connection from server to client
- Server creates a channel using a unique client ID and a duration (default in GAE is 2 hours), producing a token
- Client uses the token to connect to the channel and creates a socket to listen to
- Server can then send a message to a specific client using its client ID, which the client receives & uses

Creation by server
------------------
- Server includes: `from google.appengine.api import channel`
- The server calls channel.create_channel(clientID, duration_minutes) to create the channel
- This returns a token variable which is embedded in the page
- ClientID can be created by the server or passed from the client in a request for a channel
- ClientID can be anything you like as long as it's unique (a combo of user and song would probably be appropriate)
- duration_minutes is how long channel remains open - between 0 and 24*60 (a day), default 120. After this time, channel is closed.

Connection by client
--------------------
- Client includes: `<script type="text/javascript" src="/_ah/channel/jsapi"></script>`
- Client opens the channel using the token embedded in the page and creates a socket:
- `channel = new goog.appengine.Channel('{{ token }}');`
- `socket = channel.open();`
- Callbacks are then assigned to the socket for `onOpened` (when socket ready), `onMessage` (when data is received), `onError` (error e.g. timeout) and `onClose` (closed)

Usage
-----
- Server calls `channel.send_message(clientID, message)` - clientID can also be the token create returns
- Message limited to 32K, should be a JSON
- When client receives data, `socket.onMessage` callback called with the message as a parameter
- Client can close its socket using `socket.close()` - this does not close the channel, a new socket can be opened from the channel object
- Channels cannot be closed, they simply expire after their predetermined timeout

Errors
------
- In the event of a message send failure, no error is produced on the server unless message/clientId was invalid
- In the event of token expiring, or being invalid altogether, `socket.onError` is called on the client, followed by `socket.onClose`
- `onError` passed a parameter - object with a code field (401) and a description: `Invalid+token` or `Token+timed+out`
- A new channel will have to be created by the server if `onError` is called

Restrictions
------------
- Only one client can use a clientID at once - you can't share and you can't broadcast to multiple clients at once. Sending to multiple clients? Do them one at a time with their individual IDs
- Clients can only have one channel per page. Sending different kinds of data? Client has to deal with it
