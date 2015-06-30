Game Jolt API for Game Maker Tutorial
=====================================

Table of Contents
-----------------
1. [Introduction](#introduction)
2. [Important Concepts](#important-concepts)  
    * [Local vs. Online Storage](#local-vs-online-storage)  
    * [Async vs. Sync commands](#async-vs-sync-commands)
3. [Setting the API up for Your Game](#setting-the-api-up-for-your-game)  
    * [Finding Your Game's Details](#finding-your-games-details)  
    * [Setting Your Game Up](#setting-your-game-up)  
    * [Logging In](#logging-in)
4. [Running the API Commands](#running-the-api-commands)  
    * [Users](#users)
        * [How About Other Users?](#how-about-other-users)
    * [Sessions](#sessions)
    * [Trophies](#trophies)
        * [Earning Trophies](#earning-trophies)
    * [Scores](#scores)
        * [Getting Scores](#getting-scores)
        * [Submitting Scores](#submitting-scores)
    * [Data Storage](#data-storage)
        * [Online Management](#online-management)
        * [In-Game Management](#in-game-management)
            * [Fetching Online Data](#fetching-online-data)
            * [Storing Online Data](#storing-online-data)
            * [Getting Local Data](#getting-local-data)
            * [Setting Local Data](#setting-local-data)
            * [Updating Data](#updating-data)
            * [Removing Data](#removing-data)
    * [Cleaning Up](#cleaning-up)
        * [Cleaning Fetchables](#cleaning-fetchables)
5. [Conclusion](#conclusion)

Introduction
------------

This tutorial is written to be read from start to finish. I try to keep things
short and simple, but I accomplish this by introducing a little bit at each
section of the tutorial, and asking you to recall previous sections instead of
repeating myself. This is my preferred tutorial style, but I'm open to
elaborating on parts that are confusing or trimming down parts that are too
verbose, just leave a comment saying so!

The API follows many patterns that attempt to make command usage intuitive. If
you understand how a specific set of commands work, you can figure out how the
rest work. I'll point out the patterns throughout the tutorial, so when you
encounter one try your best to remember it!

Finally, I highly recommend that you take an hour or two to sit down and give it
a good read, instead of in bits and pieces. When you finish you'll have a much
better understanding of the system and will be well on your way to doing all
sorts of cool and great stuff with Game Jolt!

Good luck! :)


Important Concepts
------------------

### Local vs. Online Storage

The implementation uses the concept of a *local* and an *online* storage.
The local storage exists only in your game instance, and any changes you make to
it _will not_ be reflected on the Game Jolt server unless you explicitly call a
command that does so.  The online storage is Game Jolt itself! When you want to
store data into Game Jolt, you _push your changes from the local store onto the
online store_!

This is done so that you don't need to update the server for every little
change. It's important to understand when you _need_ to update data vs. when you
_want_ to.

### Async vs. Sync Commands

Asynchronous (async) and synchronous (sync) are actually very important terms
for programming in general. Synchronous commands will _freeze the program until
the task it's performing completes_. Asynchronous commands will _run the task in
the background and allow your program to continue normally_.

Thus:  
 * *Async* will allow your game to seemlessly work with online data, *but it's
your responsibility to check that the data has been downloaded before using it.*
 * *Sync* will freeze your game up (I recommend assuming it takes a while so you
try not to rely on it too much), *but the next command run after the call is
garunteed to have usable data.*

It's important for you really understand these concepts, so if you have
questions get in contact w/ me in some way and I'll try to help!


Setting the API up for Your Game
--------------------------------

### Finding Your Game's Details

To use the API you must first find your *GameID & Private Key*. Navigate to your
game's dashboard and select API settings. Here's the page for this game as an
example:

![The Game Dashboard][dash image]  
(As you can see, all of the API data involved with your game is also on this
page)

### Setting Your Game Up

The two scripts neccesary for the API to run correctly is `GJ_begin` and
`GJ_step`. `GJ_begin` should be called *once in the lifetime of your program*
before any other `GJ_*` call is made.
`GJ_step` should be called *once per step.* The script relies on the
assumption it is, so if your API acts strangely it's most likely because you
have extra calls being made.

My recommended approach is creating a persistant `GJControl` object, and

 * In it's `Game Start` event, run `GJ_begin` with your *GameID & Private Key*
as arguments.
 * In it's `Begin Step` event, run `GJ_step`.

I like to have its depth as the lowest number possible, because this means that
it's run _before_ any other object's step events. This gives you the nice
garauntee that as soon as something has finished downloading, _every other step
event code will be able to access it_.

![GJControl Example][gjcontrol image]

Now drop _one_ instance of it in your first room, and for the rest of the game
you won't ever have to worry again!

**Beware!** If at any time you deactivate instances, make sure that the
`GJControl` object stays active!

### Logging In

Logging in requires a username and token passed to `GJ_login`, and much like
`GJ_begin`, you will run this _once_ with the details of a user. How you fetch
the two details is up to you, generally games have a textbox input which they
then pass to the command.

The API gives you a convinience command, `GJ_isLoggedIn` which will return
true when a successful `GJ_login` has gone through.

To log a user out, there is a `GJ_logout` that can do this for you. After
logging out you can use the `GJ_login` again for a different user.

Although it's handled by `GJ_step` already, I feel it's important to mention
that in order to _remain logged in_, you must occasionally ping the GJ servers.
If your game goes past one minute without pinging, you will be logged out.
Again, though, `GJ_step` takes care of this for you already.


Running the API Commands
------------------------

### Users

After a user logs in, some of their Game Jolt profile data is downloaded as
well, namely: avatar (the image will be downloaded and loaded as a sprite for
you), developer name, website url, last time of login, age, and a way to check
their user type (Admin, Banned, Developer, Gamer, Moderator).

The logged in user has a special ID value of 0. Thus, to access these values,
you would use `GJ_user_get*(0)`. For the full list of commands, see [the
documentation][user getters].

#### How About Other Users?

You can get the same data from other users via their _user ID_. A user's ID can
be found in their profile url. For example, [mine is 1165][my profile]. If
you're interested in the logged in user's ID, you can call `GJ_user_getLoggedID`
which will return it to you.

The command to download user data is `GJ_fetch_user`. `GJ_fetch_user` has two
arguments, `now?` and `userID`. `userID` is as previously described. `now?` is
how you choose between a *synchronous call* (`now? = true`) and an
*asynchronous call* (`now? = false`). If you don't remember the difference, go
back to the Concepts section of the tutorial!

When a command is asynchronously called, there will be a corresponding
`GJ_*Ready` command that will let you know when it's data is ready. For
example, for users, the command is `GJ_user_isReady` and takes an ID. (Recall:
the special ID value 0 refers to the currently logged in user)
When a command is synchronously called, then the matching `GJ_*Ready` command
is garunteed to return `true`.

### Sessions

Recalling the logging in section, I mentioned that in order to stay logged in to
Game Jolt you must occasionally ping the server. This pinging is used as a way
of creating a _game session_. Game sessions appear on your profile, and allow
other users to know that they are playing your game! Although you don't have to
worry about pinging, you do have some control over it: you can change the ping
rate through the command `GJ_setPingRate`. If you ever feel you need to do this,
although I can't think of a reason why as of writing this, you can set the # of
seconds to wait between pings using this.

You also have the ability to set the "status" of a player. As of writing, there
are two statuses: `Active` and `Away`. As such, you can use the command
`GJ_setActive` which takes a boolean `active?` to change the user's state.
`GJ_isActive` will return the state of the user for you as well. How you decide
whether a user is active or inactive is completely up to you, you don't even
have to implement it if you don't want to.

By default, after logging in your status is set to `Active`.

### Trophies

Before you can handle trophies you must have a user logged in. After that, you
call `GJ_fetch_trophies` (this, and all `GJ_fetch*` commands, have the `now?`
argument as well)

Trophy data can be accessed (like user data) using a collection of getter
commands. These commands take an ID, which is found in your game's Trophy tab.
The list of getters is listed by [the documentation][trophy getters]. Trophies
are checked for readiness using `GJ_trophiesReady()`.

#### Earning Trophies

To earn a trophy, run the `GJ_store_trophyEarned` command, which takes a `now?`
and ID. The `GJ_store*` commands are the counterparts of fetch, and  all have
`now?` arguments like the fetches. Where fetching retrieves data from  the Game
Jolt servers, storing updates them.  
Recalling our concept of a local and online store:

 * Fetching is _letting a value in your **local** storage copy its value from
 the **online** storage_.
 * Storing is _letting a value in your **online** storage copy its value from
 the **local** storage_.

Finally, like fetching trophies, you must have a user logged in to store them.
Earning trophies also updates your local store of trophy information as a
convinience.

### Scores

Before using score-table commands, you are required to download your score-
table metadata using `GJ_fetch_tables`. This allows subsequent calls to score-
table commands to organize the data for you. You'd call this once in a program,
and use `GJ_tablesReady` to test for readiness.

The reasoning behind this indirection is that some games don't implement tables.
Thus those developers shouldn't have to waste time and resources trying to fetch
the information, and with this format they don't have to.

After this command, you now have the option of downloading specific tables from
the Game Jolt servers. The commands to acomplish this are `GJ_fetch_tableScores`
and `GJ_fetch_tableScores_user`. They both ask for `now?`, a table ID, and count
(which describes how many of the top entries to fetch). Also, similar to how the
special user ID 0 represents the currently logged in user, the special table ID
0 represents the _primary table_. For other table IDs, or to change your primary
table, you can visit your game's dashboard and go to the Scores section.

The `GJ_*_user` set of commands are another pattern the API will use. For Game
Jolt, there is a distinction between the _global_ values and _user-specific_
values. In the case of the `GJ_fetch_tableScores`, you can choose to download
the all-time top scores, or the personal best scores of the currently logged in
user.

#### Getting Scores

The score data, like all other fetchables, can be accessed using a set of
getters listed in [the documentation][score getters]. The table commands take a
tableID and a rank. rank 0 is the _best_ score, rank 1 is the _2nd best_
score, and so on. The rank is in the range: `[0, GJ_table_size)` (if you are
unfamiliar with this notation, it translates to: `0 ≤ rank < GJ_table_size`),
with `GJ_table_size` being _at most_ the count you requested in your fetch call.
You should query a table's size using the command `GJ_table_size`, since there
may not always be as many entries in a table as you request.

Recalling that the `GJ_fetch_tableScores` follows the `GJ_*_user` pattern, the
getters also have `GJ_*_user` counterparts. This allows you to download both
types of tables and access each one with the same notation. However, some of the
getters like `GJ_tableScore_isUserScore` and `GJ_tableScore_getGuestName`, do
not have `GJ_*_user` counterparts because they are useless for the `user`
versions and/or there are already methods of retrieving the data.

#### Submitting Scores

Score submissions follow the `GJ_store*` pattern. There are two types of scores
that can be submitted: _user scores_ and _guest scores_. User scores require a
logged in user before use, and will associate that user with the entry you
submit. Guest scores can be used without a logged in user, but requires a name
to be associated with the entry in place of a user.

Scores have three special values associated with them:
 * *Score*: Despite the name, this does not actually describe what the ordering
of the entry will be. Instead, this is _a string that is displayed in place of
that ordering_.
 * *Sort*: _This is the value that is used to sort by_. Thus, it should be a
number. You can choose between ascending and descending ordering on your game's
dashboard.
 * *Extra* Data: This is a wild card string that has no influence on the table.
It's purpose is solely for you to store anything else you may want to associate
with a score. (Maybe "you died from a 'Bug Bite/Car Crash/Old Age'" message, for
example)

With this knowledge in hand, see [the documentation][score storers] for usage
instructions.

### Data Storage

The data storage is Game Jolt's most flexible and useful feature! It gives you
the ability to store and fetch strings from its servers that are exclusive to
your game. Data is kept in key-value pairs and can be fetched and stored as you
please. The data storage is split into two parts: **Global** data and **User**
data, where each user has their own personal storage space.  The things you can
accomplish with this simple interface are only bounded by your imagination! Some
examples of things that you _could_ do include:

 * Online turn-based games
 * Communication between different games (if you own them)
 * In-game messaging
 * Global player stats
 * Shared user-created content
 * Save/Load syncing (you can play your same file on any computer!)

and these are just a few I can think of from the top of my head!

#### Online Management
Global data can be managed through your game's dashboard.  
As of writing, there is no way to modify user data.

#### In-Game Management
The commands that handle global and user data follow the same pattern as the
previously described `GJ_*_user` commands. Thus, when using any of the
user data commands, you must have a user logged in. That user's personal
storage space is the target of the subsequent user data commands you
call.  
Otherwise, for the global data commands, the global game storage space
will be the target.

##### Fetching Online Data
There are two methods of retrieving data from storage. The first is by
downloading the data through its key, using `GJ_fetch_*Data`. It takes `now?`
and the `key` you want. The second is by downloading the data as a file,
although the file won't be opened for you automatically, you'll have to do that
yourself.  The function for this is `GJ_fetch_*File`, and along with the `now?`
and `key` arguments, takes a `file` argument which will be the path to write the
data to.

Since you can only download data one key at a time, you must download the entire
storage by fetching each key individually. To fetch all of the _keys_ available
on the server, call the command `GJ_fetch_*Keys`. There is no readiness function
as of writing this. I don't think it really applies that well since keys are so
dynamic, saying that the keys are ready for use could be misleading since some
other instance of the game could store a key after you fetch the keys. I suggest
just remembering which keys you use and test for their existance instead, but
the command remains here regardless.

##### Storing Online Data
The `GJ_store_*Data` commands take the same arguments as their `GJ_fetch_*Data`
counterparts. This is because the API relies on you _modifying your local data
storage_. The value currently stored in your local storage will be the value
stored onto the server, except in the case of the `GJ_store_*File`, which will
store the contents of the file path you enter.

##### Getting Local Data
Getting and setting are to the local storage as fetching and storing are to the
online storage. However, they also act as a convinient way to create Game Maker
values. Thus, you have `GJ_data_*_getReal` and `GJ_data_*_getString` commands,
which will convert the value of the given key into a number and string,
respectively.

You also have a method of checking for a key's existance through
`GJ_data_*_exists`. When `true`, that means that your local data storage
contains the key. **It does not, however, tell anything about its existance on
the online storage**.

Since keys are so important to the API, you have a few commands that allow you
to iterate through them. The `GJ_data_*_key_get` will return a unique key for
each n you give, where n is in the range `[0, GJ_data_*_key_count)`.  
As an inverse, you may request for the 'n' of a key through
`GJ_data_*_key_find`, which takes the name of a key as an argument. If the key
does not exist in local storage, it will return -1 instead. Like the
`GJ_data_*_exists` command, it tells nothing about the key's existance on the
online storage.

##### Setting Local Data
Setting values is very simple, you make a call to `GJ_data_*_set`. It takes a
key and value as arguments. The value will be converted to a string on upload,
but if it's a numeric, you can still perform numeric operations on it.

Setting values will create them if they do not exist in the local storage.
Storing keys that do not exist on the online storage creates them there as well.

##### Updating Data
A perceptive reader will notice there is a problem with using only store and
fetch commands on the data store: _What if someone fetches a value before you
get a chance to store it's new value?_ Indeed, this is a problem that exists in
many types of programs, and is known as a **race condition**.

Game Jolt addresses this problem by giving us a set of _update commands_. Update
commands allow us to, through our eyes as developers, *fetch and store a value
simultaneously*. This means when we call an update command, we're garaunteed
that we aren't missing anyone else's changes to the value **so long as they
modify it through an update command as well**. Update commands change your
local store to the final value made by the command, which will be the same
value stored online.  
Recalling our description of fetch and store commands along with our new update
commands, the descriptions are now:

 * Fetching is _letting a value in your **local** storage copy its value from
 the **online** storage_.
 * Storing is _letting a value in your **online** storage copy its value from
 the **local** storage_.
 * Updating is _letting a value in your **local** and **online** storage share
some new value_.

That said, the update commands work with _operations_. This means number
operations (addition, subtraction, multiplication, division) and string
operations (append, prepend). See [the documentation][data updaters] for usage
instructions.

Update commands should be used on values that can be touched by multiple
game instances at the same time. These include values such as global kill count,
global play time, a dynamic list of users, etc. You should use your own
judgement to discern if a data value you use has a race condition. If it
doesn't, then just stick to using the local data storage to make changes.

##### Removing Data
Finally, data keys can be removed from **both the local & online storage**
through the removal functions. They are `GJ_remove_*Data`, and take a `now?` and
`key` as arguments.

### Cleaning Up

The API has an interface for cleaning up the data you download.  

To destroy absolutely everything, you can call `GJ_cleanUp()`. **This brings
your program to a state that's as if _you never called
`GJ_begin` in the first place_**. This means to begin using the API again, you
would have to call `GJ_begin`. This can be useful for accessing the storages of
different games (maybe a sequel that checks the previous game's inventory, for
example).

`GJ_cleanUp` is called for you at the end of your game's lifetime.

#### Cleaning Fetchables

All of the fetchable data (users, trophies, score tables, data storage) can be
wiped if you ever want to.

For users and score tables, you have a choice between a specific entry (using an
ID as the identifier), or every entry that's loaded. These commands are
`GJ_clear_user`/`GJ_clear_tableScores` and
`GJ_clear_user_all`/`GJ_clear_tableScores_all` respectively.

Recall that score tables follow the `GJ_*_user` pattern, and so the cleaning
commands also contain a separate `_user` method.

Trophies can be cleaned using the `GJ_clear_trophies` command.

Data storage (which follows the `GJ_*_user` pattern) can also be cleaned,
_without_ affecting the online storage. Use `GJ_clear_data*` to remove all data
loaded into your local storage. There is no way as of writing to remove a
specific key.


Conclusion
----------

This concludes the tutorial. If you've understood everything to this point, you
are ready to make some great GJ API supported games, and I can't wait to see
them up and in action!

Remember to check [the documentation][documentation] for the full list of
commands and precise explainations for each command.

Good luck with your projects, and long live Game Jolt! :)


[my profile]: http://gamejolt.com/profile/thatbrod/1165
[user getters]: https://github.com/thatbrod/GameMaker-GJAPI/blob/master/README.md#userGetters
[score getters]: https://github.com/thatbrod/GameMaker-GJAPI/blob/master/README.md#GJ_score_getters
[score storers]: https://github.com/thatbrod/GameMaker-GJAPI/blob/master/README.md#user-content-GJ_store_score
[trophy getters]: https://github.com/thatbrod/GameMaker-GJAPI/blob/master/README.md#trophyGetters
[data updaters]: https://github.com/thatbrod/GameMaker-GJAPI/blob/master/README.md#user-content-UpdateFunctions
[documentation]: https://github.com/thatbrod/GameMaker-GJAPI/blob/master/README.md
[dash image]: http://i.imgur.com/bII8ss4.png
[gjcontrol image]: http://i.imgur.com/WaRWegJ.png
