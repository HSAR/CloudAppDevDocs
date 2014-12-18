REST Endpoints
========
/auth/* - Authenticated Actions/Data

/users/* - Public User Actions/Data

/songs/* - Song Actions/Data

Get Song By ID = /songs/get/{songid}
====================================
Returns the song JSON for the song with the given ID.
(Incomplete - uses dummy data, but can be switched to datastore by uncommenting a few lines)

Get Song MIDI by ID = /songs/get/midi/{songid}
==============================================
Returns the song MIDI (base64 encoded) for the song with the given ID
(Incomplete - uses dummy data, but can be switched to datastore by uncommenting a few lines)

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
* trackId
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
Changes the subdivision value to a new value. Expects URI parameters.

* actionId

Request Token = /songs/{songid}/token GET
=======
Request token at start of editing.

Get User by Username = /users GET
=======
Retrieves user entity. Expects URI parameters.

* username

Get User by UID = /users/{uid} GET
=======
Retrieves user entity.

Get User Songs = /users/{uid}/songs GET
=======
Retrieves list of songs owned by user.

Get User Collabs = /users/{uid}/collabs GET
=======
Retrieves list of songs worked on by user.

Get User Invites = /users/{uid}/invites GET
=======
Retrieves list of songs user has been invited to work on.

