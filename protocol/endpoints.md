REST Endpoints
========

/web/users/* - User Webpages

/api/users/* - User Actions/Data

/web/songs/* - Song Webpages

/api/songs/* - Song Actions/Data

Create Song = /api/songs/ PUT
===
Creates a new song. Expects JSON.

* title
* genre (optional)
* tags (optional)

Get Song By ID = /api/songs/{songid} GET
===
Returns all held song data for the song with the given ID.

Change Song Properties = /api/songs/ PATCH
=======
Changes user properties, but only if they are present in the request body. Expects JSON.

* title (optional)
* genre (optional)
* tags (optional)

Returns {"jingleKey" : key} on success, or standard error message 500 if one or more failed or 400 if no fields were given.

NOT TRANSACTIONAL, RETRY IF FAILURE
RECOMMEND CHANGING ONE AT A TIME ONLY

Get Song JSON = /api/songs/{songid}/json GET
===
Returns the held JSON for the song with the given ID.

Get Song MIDI by ID = /api/songs/{songid}/midi GET
===
Returns the song MIDI (base64 encoded) for the song with the given ID

Add Note = /api/songs/{songid}/notes PUT
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

Remove Note = /api/songs/{songid}/notes DELETE
======
Removes a note from the specified song. Expects URI parameters.

* actionId
* track
* noteId

Add Instrument = /api/songs/{songid}/instruments PUT
=======
Adds a track (instrument) to the specified song. Expects JSON.

* action
* actionId
* instrument
  * track
  * inst

Remove Instrument = /api/songs/{songid}/instruments DELETE
======
Removes a track (instrument) from the specified song. Expects URI parameters.

* actionId
* instrumentTrack

Remove Instrument = /api/songs/{songid}/instruments PATCH
======
Changes a track (instrument) in the specified song. Expects JSON.

* action
* actionId
* instrumentTrack
* instrumentNumber

Change Tempo = /api/songs/{songid}/tempo PUT
=======
Changes the tempo to a new value. Expects JSON.

* action
* actionId
* tempo

Change Subdivisions = /api/songs/{songid}/subdivisions PUT
=======
Changes the subdivision value to a new value. Expects JSON.

* action
* actionId
* subDivisions

Request State Dump = /api/songs/{songid}/state GET
=======
DEPRECATED - use /songs/{songid}/ GET

Request Token = /api/songs/{songid}/token GET
=======
Request token at start of editing. Expects URI parameters.

* prevToken (optional)

If prevToken is given, the endpoint will attempt to refresh an old token that has expired after the 2-hour limit.
If no parameters given, will generate a new token for a new channel. To be used for clients requesting a connection at page load.

Search = /api/songs/search?query=XYZ&sort=ABC&token=DEF GET
===========================================================

If token specified, continue the previous search defined by the token.

Else, search for song by all text fields using query (search for title works
with prefix matching, author tags and genre are exact match only due to
datastore limitations) (if query not specified, all songs are returned) sorted
by the field specified in sort (if query is present, sorting by title is
enforced due to datastore limitations)

Returns a JSON object with "results", "token" and "more" fields. Results is an
array of songs (minus the actual jingle), token is the token to be given in the
URL (already URL-safe), and more if false means there are no more results (if
true, there are probably but not definitely more)

Get User by Username = /api/users GET
=======
Retrieves user entity. Expects URI parameters.

* username (optional)

If username is given, will attempt to look up (exact match) the given parameter.
If no parameters given, will retrieve a list of all registered users.

Autocomplete username = /api/users/complete GET
=======
Retrieves list of max 10 users whose usernames start with supplied username.
Expects URI parameters.

* username

Create New User = /api/users PUT
=======
Retrieves user entity. Expects URI parameters.

* uid
* username

See datastore functions / createUser()

Get User by UID = /api/users/{uid} GET
=======
Retrieves user entity.

Change User Properties = /api/users/{uid} PATCH
=======
Changes user properties, but only if they are present in the request body. Expects JSON.

* bio (optional)
* tags (optional)
* username (optional)

Returns {"userKey" : key} on success, or standard error message 500 if one or more failed or 400 if no fields were given.

NOT TRANSACTIONAL, RETRY IF FAILURE
RECOMMEND CHANGING ONE AT A TIME ONLY

Get User Songs = /api/users/{uid}/songs GET
=======
Retrieves list of songs owned by user.

Get User Collabs = /api/users/{uid}/collabs GET
=======
Retrieves list of songs worked on by user.

Remove Collab = /api/users/{uid}/collabs/{jid} DELETE
=======
Removes a user from the list of jingle collaborators.

This action can only be done if you own the jingle or if you are the user being removed.

Get User Invites = /api/users/{uid}/invites GET
=======
Retrieves list of songs user has been invited to work on.

Invite User to Collab = /api/users/{uid}/invites/{jid} DELETE
=======
Send an invite.

Respond to User Invites = /api/users/{uid}/invites/{jid} DELETE
=======
Respond to an invite. Expects URI parameters.

* response

Parameter MUST BE "true" or "false", any other value will result in Error 400.

Get Current UID = /uid
=====
Returns JSON of current UID.
