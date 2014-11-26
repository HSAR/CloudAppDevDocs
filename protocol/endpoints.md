REST Endpoints
========
/auth/* - Authenticated Actions/Data

/users/* - Public User Actions/Data

/songs/* - Song Actions/Data


Add Note = /songs/{songid}/notes PUT
=======
Adds a note to the specified song. Expects JSON.

* actionId
* note
  * id
  * pos
  * track
  * note
  * length

Remove Note = /songs/{songid}/notes GET
======
Removes a note from the specified song. Expects URI parameters.

* actionId
* trackId
* noteId

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