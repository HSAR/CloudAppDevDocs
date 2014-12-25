REST Endpoints
========
/users/* - Public User Actions/Data

/songs/* - Song Actions/Data

Create Song = /songs/ PUT
===
Creates a new song. Expects JSON.

* title
* genre (optional)
* tags (optional)

Get Song By ID = /songs/{songid} GET
===
Returns all held song data for the song with the given ID.

Change Song Properties = /songs/ PATCH
=======
Changes user properties, but only if they are present in the request body. Expects JSON.

* title (optional)
* genre (optional)
* tags (optional)

Returns {"jingleKey" : key} on success, or standard error message 500 if one or more failed or 400 if no fields were given.

NOT TRANSACTIONAL, RETRY IF FAILURE
RECOMMEND CHANGING ONE AT A TIME ONLY

Get Song JSON = /songs/{songid}/json GET
===
Returns the held JSON for the song with the given ID.

Get Song MIDI by ID = /songs/{songid}/midi GET
===
Returns the song MIDI (base64 encoded) for the song with the given ID

Add Note = /songs/{songid}/notes PUT
=======
Adds a note to the specified song. Expects JSON.

* action
* actionId
* note
  * id
  * pos
  * track
  * pitch
  * length

Remove Note = /songs/{songid}/notes DELETE
======
Removes a note from the specified song. Expects URI parameters.

* actionId
* track
* noteId

Add Instrument = /songs/{songid}/instruments PUT
=======
Adds a track (instrument) to the specified song. Expects JSON.

* action
* actionId
* instrument
  * track
  * inst

Remove Instrument = /songs/{songid}/instruments DELETE
======
Removes a track (instrument) from the specified song. Expects URI parameters.

* actionId
* instrumentTrack

Remove Instrument = /songs/{songid}/instruments PATCH
======
Changes a track (instrument) in the specified song. Expects JSON.

* action
* actionId
* instrumentTrack
* instrumentNumber

Change Tempo = /songs/{songid}/tempo PUT
=======
Changes the tempo to a new value. Expects JSON.

* action
* actionId
* tempo

Change Subdivisions = /songs/{songid}/subdivisions PUT
=======
Changes the subdivision value to a new value. Expects JSON.

* action
* actionId
* subDivisions

Request State Dump = /songs/{songid}/state GET
=======
DEPRECATED - use /songs/{songid}/ GET

Request Token = /songs/{songid}/token GET
=======
Request token at start of editing.

Get User by Username = /users GET
=======
Retrieves user entity. Expects URI parameters.

* username

Create New User = /users PUT
=======
Retrieves user entity. Expects URI parameters.

* uid
* username

See datastore functions / createUser()

Get User by UID = /users/{uid} GET
=======
Retrieves user entity.

Change User Properties = /users/{uid} PATCH
=======
Changes user properties, but only if they are present in the request body. Expects JSON.

* bio (optional)
* tags (optional)
* username (optional)

Returns {"userKey" : key} on success, or standard error message 500 if one or more failed or 400 if no fields were given.

NOT TRANSACTIONAL, RETRY IF FAILURE
RECOMMEND CHANGING ONE AT A TIME ONLY

Get User Songs = /users/{uid}/songs GET
=======
Retrieves list of songs owned by user.

Get User Collabs = /users/{uid}/collabs GET
=======
Retrieves list of songs worked on by user.

Remove Collab = /users/{uid}/collabs/{jid} DELETE
=======
Removes a user from the list of jingle collaborators.

This action can only be done if you own the jingle or if you are the user being removed.

Get User Invites = /users/{uid}/invites GET
=======
Retrieves list of songs user has been invited to work on.

Invite User to Collab = /users/{uid}/invites/{jid} DELETE
=======
Send an invite.

Respond to User Invites = /users/{uid}/invites/{jid} DELETE
=======
Respond to an invite. Expects URI parameters.

* accept

Parameter MUST BE "true" or "false", any other value will result in Error 400.

Get Current UID = /uid
=====
Returns JSON of current UID.
