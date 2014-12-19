Protocol
========

Function
--------

The protocol is based on the idea of having an action that would still produce
the same result if applied twice. This allows the protocol to function without
too much work with synchronisation.

There are two basic actions - adding a note, and deleting a note. Moving can be
handled client-side with a delete then an add request.

You can also edit the tempo and subDivisions values, add an instrument, remove
an instrument and change the instrument ID by track ID.

Each action has an ID. This allows clients to keep track of which actions have
been acted upon, to help check sync (see below).

Everything is sent with JSON.

Sync
----

Some checksum is performed on state, in both the server and the client. This
allows sync to be checked for debug purposes. The checksum is sent with every
action, and checked only when the client has no outstanding actions.

Action
------
These are sent from client to server.

An action object has the following attributes:

"action": "noteAdd", "noteRm", "tempo", "subDivisions", "instrumentAdd",
"instrumentRm", "instrumentEdit", "stateDump"

noteAdd
=======
{
  "action": "noteAdd",
  "actionId": uniqueActionId,
  "note": {
    "id": number,
    "pos": number,
    "track": number,
    "pitch": number,
    "length": number
  }
}

noteRm
======
{
  "action": "noteRm",
  "actionId": uniqueActionId,
  "noteId": noteid,
  "track": trackNumber
}

tempo
=====
{
  "action": "tempo",
  "actionId": uniqueActionId,
  "tempo": newTempo
}

subDivisions
============
{
  "action": "subDivisions",
  "actionId": uniqueActionId,
  "subDivisions": newSubDivisions
}

instrumentAdd
=============
{
  "action": "instrumentAdd",
  "actionId": uniqueActionId,
  "instrument": {
    "track": number,
    "inst": number
  }
}

instrumentRm
============
{
  "action": "instrumentRm",
  "actionId": uniqueActionId,
  "instrumentTrack": instrumentTrack
}

instrumentEdit
==============
{
  "action": "instrumentEdit",
  "actionId": uniqueActionId,
  "instrumentTrack": instrumentTrack,
  "instrumentNumber": instrumentNumber
}

stateDump
=========
This is a request for a stateDump
{
  "action": "stateDump",
  "actionId": uniqueActionId,
}

Server broadcasts
-----------------

The server broadcasts an action to all clients editing that song with the same
action contents, but with an additional field, "checksum". This contains a
32-bit unsigned integer checksum (computed using adler32 of the UTF-8 bytes of
the string, with the  of the state after this action has been applied. Once
clients have no outstanding actions to be serviced (they can check this by
keeping a list of action IDs sent, remembering to clear it if a state dump is
required), they can check the state by comparing the checksum against a
calculated checksum of their own state. If the states don't match, they can
flag up an error and/or request a state dump.

The checksum format is an unsigned adler32 checksum of the JSON state with
notes ordered by note ID then noteOn (it's invalid to have a note with the same
ID and noteOn status), instruments ordered by channel, no whitespace, and
fields in an object ordered ASCIIbetically. If anyone has a better idea, feel
free to correct this.

State dump
==========

The server can send the entire state to the client, by sending the "stateDump"
action after a client sends an empty stateDump action. This action contains an
additional object, the "state" object, containing JSON as described in
"Updated Json format.txt".
