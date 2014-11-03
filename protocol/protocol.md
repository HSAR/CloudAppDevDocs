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
    "chan": number,
    "note": number,
    "vol": number,
    "noteOn": boolean
  }
}

noteRm
======
{
  "action": "noteRm",
  "actionId": uniqueActionId,
  "noteId": noteid
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
    "chan": number,
    "inst": number
  }
}

instrumentRm
============
{
  "action": "instrumentRm",
  "actionId": uniqueActionId,
  "instrumentChan": instrumentChannel
}

instrumentEdit
==============
{
  "action": "instrumentEdit",
  "actionId": uniqueActionId,
  "instrumentChan": instrumentChannel,
  "instrumentNumber": instrumentNumber
}

stateDump
=========
{
  "action": "stateDump",
  "actionId": uniqueActionId,
  "state":
  {
    /*Object as documented in "Updated Json format.txt"*/
  }
}


State dump
----------

The server can send the entire state to the client, by sending the "stateDump"
action.





A adds note 1 to position x
B adds note 2 to position x

A locally has note 1
B locally has note 2

Server sends to all clients note 1 at x
Server sends to all clients note 2 at x


