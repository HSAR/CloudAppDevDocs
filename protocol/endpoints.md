REST Endpoints
========
/auth/* - Authenticated Actions/Data

/users/* - Public User Actions/Data

/songs/* - Song Actions/Data


Add Note = /songs/<songid>/notes PUT
=======
Adds a note to the specified song. Expects JSON.

* actionId
* note
  * id
  * pos
  * track
  * note
  * length

Remove Note = /songs/<songid>/notes GET
======
Removes a note from the specified song. Expects URI parameters.

* actionId
* trackId
* noteId