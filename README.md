Pokémon Showdown Client (this is mostly from the og read me, to pair up with the server repo. but below are some important notes for adding mons/ getting this running)


~~~~
list of all things to do (feel free to add to this):
~~~~

* have not messed around with sprites and in game matches, but this just gets the custom mon to appear on the client side's pokemon list (tldr for this, it looks like the main showdown server connects to a different one to load the sprites (we COULD download all of them from the spreadsheet, I am curious if there is a method for connecting to that directly in case of file size/streamlining edits a bit more, I think to add this it would be in the js files that load the html? but haven't investigated too much)
* add other pokemon in
* add mechanics in
 ^ like what was mentioned with the sprites, there might be a way to pull from the spreadsheet to make doing that easier

~~~~
how to host the server locally *prereq is that you have node/npx installed, easier to do on linux, but if on other os I think you can use on a docker (main showdown github has more detailed instructions):
~~~~

clone pokemon showdown server and client (put them in two separate folders)

in server (in the data folder as of writing this):
* add pokemon to formats-data.ts
* add pokemon to pokedex 
* add pokemon to learnsets
^ main thing to note is to specify them as the same gen in the dex as the one that they are also able to learn the moves in

in client:
* copy the entire pokemon showdown server folder to the cache one
* in the build tools folder, run the build-indexes file (that has small edits made to look at the new directory and not the online one, a copy of the edited version is in this refrences folder)
* this should populate the data directory in the client with the new info

to launch:
(after downloading node), in the /pokemon-showdown folder (server), in a terminal type 'node pokemon-showdown'

with this terminal open (and when the server is up) you can have a client join the server by opening up a terminal in the lient directory '/pokemon-showdown-client' and running 'npx http-server'. In a browser, then go to 'http://localhost:____/play.pokemonshowdown.com/testclient-old.html?~~localhost:XXXX' to play as a client 'localhost:____' also lets u play, but its from the server pov so new mons actually don't show up (server launcher calls to the like actual main showdown server, but when we run our own, and a client gets the data from the server, it overwrites it and adds custom mons). 

Where '____' = is the port specified in the server terminal (should say something like "Test your server at http://localhost:____")

and 'XXXX' = is the port specified in the client terminal (should say something like "http:10.0.0.??:XXXX" (the '??' represent random numbers that your computer picks based on availible slots on the router, it doesn't matter for connecting)

Only other notes are that there are prompts for copying and pasting stuff that has to do with the fact that we don't have a sign in function rn (haven't looked into too much), and that you have to add the mons in the teambuilder/select a format that they are viable in. Otherwise, opening up the same link in a new tab can allow for a new user to be simulated on the same machine and you can battle with them. VERY IMPORTANT NOTE THAT I LEARNED THE HARD WAY: be careful about updating/cloning from the main github for showdown rn as they are experimenting with adding a new client website that might break some things

^ * this might not be the best way to set up the server, but its the one that I've found. If there are better ones, we can ofc change it. fwiw I think we should be able to call "git fetch upstream" if we want to pull from the main showdown repo (if I set that up correctly, might not have, have not tested this lol)








========================================================================

Navigation: [Website][1] | [Server repository][2] | **Client repository** | [Dex repository][3]

  [1]: http://pokemonshowdown.com/
  [2]: https://github.com/smogon/pokemon-showdown
  [3]: https://github.com/Zarel/Pokemon-Showdown-Dex

Introduction
------------------------------------------------------------------------

This is a repository for most of the client code for Pokémon Showdown.

This is what runs `play.pokemonshowdown.com`.

**WARNING: You probably want the [Pokémon Showdown server][4]**, if you're
setting up a server.

  [4]: https://github.com/smogon/pokemon-showdown

Browser support
------------------------------------------------------------------------

Pokémon Showdown currently supports, in order of preference:

 - Chrome
 - Chromium browsers (Edge, Vivaldi, Brave, Opera...)
 - Firefox
 - Safari 5+
 - IE11+ and Edge
 - Chrome/Firefox/Safari for various mobile devices
 - any remotely modern browser

Pokémon Showdown is usable, but expect degraded performance and certain features not to work in extremely legacy browsers like:

 - Safari 4
 - IE9-10

Pokémon Showdown is mostly developed on Chrome, and Chrome or the desktop client is required for certain features like dragging-and-dropping teams from PS to your computer. However, bugs reported on any supported browser will usually be fixed pretty quickly.

New client
------------------------------------------------------------------------

Development is proceeding on the client rewrite! The live version is
available at https://play.pokemonshowdown.com/beta

You can contribute to it yourself using the same process as before, just
use `testclient-new.html` rather than `testclient-old.html`.

Testing (the old client)
------------------------------------------------------------------------

Client testing requires a build step! Install the latest Node.js (we
require v20 or later) and Git, and run `node build` (on Windows) or `./build`
(on other OSes) to build.

You can make and test client changes simply by building after each change,
and opening `play.pokemonshowdown.com/testclient-old.html`. This will allow you
to test changes to the client without setting up your own login server.

### Test keys

For security reasons, browsers [don't let other websites control PS][5], so
they can't screw with your account, but it does make it harder to log in on
the test client.

The default hack makes you copy/paste the data instead, but if you're
refreshing a lot, just add a `config/testclient-key.js` file, with the
contents:

    const POKEMON_SHOWDOWN_TESTCLIENT_KEY = 'sid';

Replace `sid` with the contents of your actual PS `sid` cookie. You can quickly
grab it from:

> https://play.pokemonshowdown.com/testclient-key.php

Make sure to put it in `config/` and not `play.pokemonshowdown.com/config/`.

(This is the only supported method of logging in on the beta testclient.)

  [5]: https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS

### Other servers

You can connect to an arbitrary server by navigating to
`testclient-old.html?~~host:port`. For example, to connect to a server running
locally on port 8000, you can navigate to `testclient-old.html?~~localhost:8000`.

**NOTE**: Certain browsers will convert `'?'` to `'%3F'` when reading files off
of the local filesystem. As a workaround, try using a different browser or
serving the files locally first (ie. run `npx http-server` from the
directory this README is in, then navigate in your browser to
`http://localhost:8080/testclient-old.html?~~localhost:8000`).

### Limitations

Even with a test key, the following things will fail in `testclient`:

+ Registering
+ Logging into other accounts (you can still switch to other unregistered
  accounts and back, though)

Everything else can be tested.

Warning
------------------------------------------------------------------------

This repository is not "batteries included". It does NOT include instructions
to run a full Pokémon Showdown login server, and we will not provide them.
Please do not ask for help on this; you will be turned away.

If you make a mistake hosting a login server, your users' passwords can get
stolen, so we do not want anyone to host a login server unless they can
figure out how to do it without help.

It also doesn't include several resource files (namely, the `/audio/` and
`/sprites/` directories) for size reasons.

On the other hand, as long as you don't want to run your own login server,
this repository contains everything you need to test changes to the client;
just see the "Testing" section above.

License
------------------------------------------------------------------------

Pokémon Showdown's client is distributed under the terms of the [AGPLv3][6].

The reason is mostly because I don't want low-effort proprietary forks that add bad code that steals everyone's passwords, or something like that.

If you're doing _anything_ else other than forking, _especially_ if you want to some client code files in your own open-source project that you want to release under a more permissive license (like, if you want to make your own multiplayer open-source game client for a different game), please ask at `staff@pokemonshowdown.com`. I hold all the copyright to the AGPLv3 parts and can relicense them to MIT for you.

  [6]: http://www.gnu.org/licenses/agpl-3.0.html

**WARNING:** This is **NOT** the same license as Pokémon Showdown's server.
