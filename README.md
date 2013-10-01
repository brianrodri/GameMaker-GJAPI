<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
<link href="style.css" rel="stylesheet" type="text/css" />
<title>Game Maker 8.0 GameJolt Game Achievements System</title>
</head>
<body>
<div class="header">
	<h1>Game Achievements System<br />for Game Maker 8.0</h1>
	<em>by thatbrod</em>
</div>
<div class="wrapper">
	<h3>Getting Started</h3>
	<p>You need to find the ID and private key of your game of choice. You can find these by going to your dashboard, clicking on your game (or creating one if you haven't yet), and then clicking the "Achievements" tab. Your details will be displayed on the side area titled "<strong>Game Info</strong>."</p>
	<p>Now, in your project, you're going to run the <a class="functionName" href="#GJ_begin">GJ_begin()</a> script (preferably in some Game Start event), which uses the game ID and private key found on that page as arguments. Next you need to find a place to run <a class="functionName" href="#GJ_step">GJ_step()</a>. This function needs to be run <em>once every step</em>. There are many ways to do this, personally though, I like to have a persistent object run it in it's step event. With these two steps finished, you're all ready to use the API! You can now call any function and play w/ it's outputs to achieve whatever you'd like with Game Jolt's Achievement System!</p>
	<h3>Using Trophies</h3>
	<p></p>
	<h3>Using Highscores</h3>
	<p></p>
	<h3>Using Data Storage</h3>
	<p></p>
	<h3>Function List</h3>
	
	<table class="functionSet">
		<!-- MAIN FUNCTIONS -->
		<tr class="header"><th id="MainFunctions" colspan="2">Main Functions</th></tr>
		<tr>
			<th id="GJ_begin">GJ_begin(gameID, privateKey)</th>
			<td>Sets up the API to work with your particular game of choice. No other API functions will work without this being called first. It should only be called once; Unless you use <a class="functionName" href="#GJ_cleanUp">GJ_cleanUp()</a> in which case you need to call <a class="functionName" href="#GJ_begin">GJ_begin()</a> again before continuing the use of API functions.<br /><br />
			<strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">GJ_STATUS_OK</span> - If used correctly</li>
				<li><span class="returnValue">GJ_STATUS_INVALID_CALL</span> - Otherwise</li>
			</ul>
			</td>
		</tr>
		<tr>
			<th id="GJ_step">GJ_step()</th>
			<td>Handles all the grunt work of the API. Must be run once every step of the game in order for the API to function correctly.<br /><br />
			<strong>Returns: </strong>Nothing</td>
		</tr>
		<tr>
			<th id="GJ_login">GJ_login(username, usertoken)</th>
			<td>Takes the given inputs (both <span class="returnValue">strings</span>) and checks with Game Jolt to see if the details are correct. This function forces the game to wait and see if the login was successful. It also automatically downloads the details of that user. (See the <a class="functionName" href="#userGetters">User Getter</a> functions for more information)<br /><br />
			<strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">GJ_STATUS_OK</span> - If all goes well</li>
				<li><span class="returnValue">GJ_STATUS_INVALID_CALL</span> - If you are already logged in or haven't set up the API correctly</li>
				<li><span class="returnValue">GJ_STATUS_NETWORK_ERROR</span> - If there was an issue retrieving data from Game Jolt</li>
				<li><span class="returnValue">GJ_STATUS_REQUEST_FAILED</span> - If the user details were incorrect</li>
			</ul>
			</td>
		</tr>
		<tr>
			<th id="GJ_logout">GJ_logout()</th>
			<td>Removes all of the downloaded data associated with the currently logged in user from memory and allows another user to attempt to login.<br /><br />
			<strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">GJ_STATUS_OK</span> - If used correctly</li>
				<li><span class="returnValue">GJ_STATUS_INVALID_CALL</span> - Otherwise</li>
			</ul>
			</td>
		</tr>
		<tr>
			<th id="GJ_isLoggedIn">GJ_isLoggedIn()</th>
			<td><strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">true</span> - If a user is logged in</li>
				<li><span class="returnValue">false</span> - Otherwise</li>
			</ul>
			</td>
		</tr>
		
		<!-- UPDATING FUNCTIONS -->
		<tr class="header"><th id="UpdateFunctions" colspan="2">Update Functions</th></tr>
		<tr><td colspan="2">Sometimes you need to update data, not just set it to something new. Take global enemy kills, for example. If you just fetch the data, modify it locally, and store it back online, you miss the chance to account for people who may have stored their data in between the time you've been modifying and storing it. By letting Game Jolt update the value, however, it ensures that all of the updates are accounted for because the server must process updates one at a time. As an added benefit, the new value the server holds after applying your update is downloaded and stored into your local data store.</td></tr>
		<tr>
			<th>
			<span id="GJ_update_userData_add">GJ_update_userData_add(now?, key, value)</span><br />
			<span id="GJ_update_userData_subtract">GJ_update_userData_subtract(now?, key, value)</span><br />
			<span id="GJ_update_userData_multiply">GJ_update_userData_multiply(now?, key, value)</span><br />
			<span id="GJ_update_userData_divide">GJ_update_userData_divide(now?, key, value)</span><br />
			<span id="GJ_update_userData_append">GJ_update_userData_append(now?, key, value)</span><br />
			<span id="GJ_update_userData_prepend">GJ_update_userData_prepend(now?, key, value)</span><br />
			<span id="GJ_update_globalData_add">GJ_update_globalData_add(now?, key, value)</span><br />
			<span id="GJ_update_globalData_subtract">GJ_update_globalData_subtract(now?, key, value)</span><br />
			<span id="GJ_update_globalData_multiply">GJ_update_globalData_multiply(now?, key, value)</span><br />
			<span id="GJ_update_globalData_divide">GJ_update_globalData_divide(now?, key, value)</span><br />
			<span id="GJ_update_globalData_append">GJ_update_globalData_append(now?, key, value)</span><br />
			<span id="GJ_update_globalData_prepend">GJ_update_globalData_prepend(now?, key, value)</span>
			</th>
			<td>Applies the corresponding update to the given key on the Game Jolt servers. For add, subtract, multiply, &amp; divide; the value must be a real number. For append &amp; prepend, the value must be a string. Global data applies to the global data store, user data applies to the currently logged in user's data store.<br /><br />
			<strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">GJ_STATUS_OK</span> - If placed in the background or if the data has been updated</li>
				<li><span class="returnValue">GJ_STATUS_INVALID_CALL</span> - If used incorrectly</li>
				<li><span class="returnValue">GJ_STATUS_NETWORK_ERROR</span> - If there was an issue with a connection</li>
				<li><span class="returnValue">GJ_STATUS_REQUEST_FAILED</span> - If there was an issue with your input parameters.</li>
			</ul></td>
		</tr>
		
		<!-- CLEARING FUNCTIONS -->
		<tr class="header"><th id="ClearFunctions" colspan="2">Clearing Functions</th></tr>
		<tr>
			<th id="GJ_cleanUp">GJ_cleanUp()</th>
			<td>Wipes all data that the API may have downloaded. In order to use any API functions again after this, you must call <a class="functionName" href="#GJ_begin">GJ_begin()</a> again.<br /><br />
			<strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">GJ_STATUS_OK</span> - If used correctly</li>
				<li><span class="returnValue">GJ_STATUS_INVALID_CALL</span> - Otherwise</li>
			</ul></td>
		</tr>
		<tr>
			<th id="GJ_clear_data"><span id="GJ_clear_data_user">GJ_clear_data_user()</span><br /><span id="GJ_clear_data_global">GJ_clear_data_global()</span></th>
			<td>Removes all of the local data associated with the given type of store. Global being the global store, user being the currently logged in user's store.<br /><br />
			<strong>Returns: </strong>Nothing</td>
		</tr>
		<tr>
			<th id="GJ_clear_trophies">GJ_clear_trophies()</th>
			<td>Removes all of the local data associated with trophies.<br /><br />
			<strong>Returns: </strong>Nothing</td>
		</tr>
		<tr>
			<th id="GJ_clear_user">GJ_clear_user(userID)</th>
			<td>Removes all of the local data associated with the given user.<br /><br />
			<strong>Returns: </strong>Nothing</td>
		</tr>
		<tr>
			<th id="GJ_clear_user_all">GJ_clear_user_all()</th>
			<td>Removes all local data of all of the users that have been downloaded, with the exception of the currently logged in user.<br /><br />
			<strong>Returns: </strong>Nothing</td>
		</tr>
		<tr>
			<th id="GJ_clear_table"><span id="GJ_clear_tableScores">GJ_clear_tableScores(tableID)</span><br /><span id="GJ_clear_tableScores_user">GJ_clear_tableScores_user(tableID)</span></th>
			<td>Removes all local data of the scores downloaded in the given score table. If the _user version is used, only the logged in user's list will be removed.<br /><br />
			<strong>Returns: </strong>Nothing</td>
		</tr>
		<tr>
			<th id="GJ_clear_table_all"><span id="GJ_clear_tableScores_all">GJ_clear_tableScores_all(tableID)</span><br /><span id="GJ_clear_tableScores_user_all">GJ_clear_tableScores_user_all(tableID)</span></th>
			<td>Removes all scores from every table that has been downloaded. If the _user version is used, only the logged in user's set of scores will be removed.<br /><br />
			<strong>Returns: </strong>Nothing</td>
		</tr>
		
		<!-- REMOVAL FUNCTIONS -->
		<tr class="header"><th id="RemoveFunctions" colspan="2">Removal Functions</th></tr>
			<tr>
				<th id="GJ_remove_data"><span id="GJ_remove_userKey">GJ_remove_userData(now?, key)</span><br /><span id="GJ_remove_globalKey">GJ_remove_globalData(now?, key)</span></th>
				<td>Removes the given key from the local and online data storage. The global storage when using the global version, and the currently logged in user's storage when using the user version.<br /><br />
				<strong>Returns: </strong>
				<ul>
					<li><span class="returnValue">GJ_STATUS_OK</span> - If placed in the background or if the data has been removed</li>
					<li><span class="returnValue">GJ_STATUS_INVALID_CALL</span> - If used incorrectly</li>
					<li><span class="returnValue">GJ_STATUS_NETWORK_ERROR</span> - If there was an issue with a connection</li>
					<li><span class="returnValue">GJ_STATUS_REQUEST_FAILED</span> - If there was an issue with your input parameters.</li>
				</ul></td>
			</tr>
			
		<!-- SESSION FUNCTIONS -->
		<tr class="header"><th id="SessionFunctions" colspan="2">Session Functions</th></tr>
			<tr>
				<th id="GJ_isActive">GJ_isActive()</th>
				<td><strong>Returns: </strong>
				<ul>
					<li><span class="returnValue">true</span> - If you have defined the current user to be "Active". User is set as active automatically upon use of <a class="functionName" href="GJ_login">GJ_login()</a></li>
					<li><span class="returnValue">false</span> - If you have defined the current user to be "Away"</li>
				</ul></td>
			</tr>
			<tr>
				<th id="GJ_setActive">GJ_setActive(active?)</th>
				<td>Sets the state of the current user. Use <span class="returnValue">true</span> to define as "Active", use <span class="returnValue">false</span> to define as "Away".<br /><br /><strong>Returns: </strong>Nothing</td>
			</tr>
			<tr>
				<th id="GJ_setPingRate">GJ_setPingRate(seconds)</th>
				<td>Sets the rate at which to update the Game Jolt servers. By default, it's 30 seconds. You can set it to whatever you'd like, but be warned that if the user doesn't successfully ping into the Game Jolt server soon enough, the servers and the API will automatically log the user out.<br /><br /><strong>Returns: </strong>Nothing</td>
			</tr>
			
		<!-- FETCHING FUNCTIONS -->
		<tr class="header"><th id="FetchFunctions" colspan="2">Fetching Functions</th></tr>
		<tr><td colspan="2">Fetch functions (and all functions that actually connect to Game Jolt) have a special argument called <span class="returnValue">now?</span><br />When set to true, this argument will allow you to stop the game from continuing until all of the requested data has been downloaded. When false, it will be worked on in the background and the game will continue normally.</td></tr>
		<tr>
			<th id="GJ_fetch_user">GJ_fetch_user(now?, userID)</th>
			<td>Requests the user w/ the given ID to Game Jolt, and downloads the data for use with the <a class="functionName" href="#userGetters">User Getter</a> functions.<br /><br />
			<strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">GJ_STATUS_OK</span> - If placed in the background or if the user exists and has been downloaded</li>
				<li><span class="returnValue">GJ_STATUS_INVALID_INPUT</span> - If userID is not a real number</li>
				<li><span class="returnValue">GJ_STATUS_INVALID_CALL</span> - If used incorrectly</li>
				<li><span class="returnValue">GJ_STATUS_NETWORK_ERROR</span> - If there was an issue retrieving data from Game Jolt</li>
				<li><span class="returnValue">GJ_STATUS_REQUEST_FAILED</span> - If the user doesn't exist on Game Jolt servers</li>
			</ul>
			</td>
		</tr>
		<tr>
			<th id="GJ_fetch_trophies">GJ_fetch_trophies(now?)</th>
			<td>Requests the trophies associated with your game and downloads them for use with the <a class="functionName" href="#trophyGetters">Trophy Getter</a> functions.<br /><br />
			<strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">GJ_STATUS_OK</span> - If placed in the background or if the trophies have been downloaded</li>
				<li><span class="returnValue">GJ_STATUS_INVALID_CALL</span> - If used incorrectly/trophies have already been loaded</li>
				<li><span class="returnValue">GJ_STATUS_NETWORK_ERROR</span> - If there was an issue retrieving data from Game Jolt</li>
			</ul>
			</td>
		</tr>
		<tr>
			<th id="GJ_fetch_tables">GJ_fetch_tables(now?)</th>
			<td>Requests the information associated with all of your high score tables. <em>You must call this function before using any high score functions!</em><br /><br />
			<strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">GJ_STATUS_OK</span> - If placed in the background or if the trophies have been downloaded</li>
				<li><span class="returnValue">GJ_STATUS_INVALID_CALL</span> - If used incorrectly/tables have already been loaded</li>
				<li><span class="returnValue">GJ_STATUS_NETWORK_ERROR</span> - If there was an issue with a connection</li>
				<li><span class="returnValue">GJ_STATUS_REQUEST_FAILED</span> - If there was an issue with your input parameters.</li>
			</ul>
			</td>
		</tr>
		<tr>
			<th id="GJ_fetch_data"><span id="GJ_fetch_userData">GJ_fetch_userData(now?, key)</span><br /><span id="GJ_fetch_globalData">GJ_fetch_globalData(now?, key)</span><br /><span id="GJ_fetch_userFile">GJ_fetch_userFile(now?, key, file)</span><br /><span id="GJ_fetch_globalFile">GJ_fetch_globalFile(now?, key, file)</span></th>
			<td>This gives you access to Game Jolt's data storage feature! You can achieve all sorts of neat things that involve online communications. Trading systems, turn-based battles, messaging, etc. can all be achieved with clever use of Game Jolt's data storage.<br />The difference between USER and GLOBAL data functions, is that user data requires you to be logged in as it only returns data for a user that gives you their token. Global data is data that can be accessed and modified by any user.<br />The File version of the scripts work the same way, but instead of storing them into your dynamic memory it stores the downloaded information into the file location you feed in.<br /><br />
			<strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">GJ_STATUS_OK</span> - If placed in the background or if the data has been downloaded</li>
				<li><span class="returnValue">GJ_STATUS_INVALID_CALL</span> - If used incorrectly</li>
				<li><span class="returnValue">GJ_STATUS_NETWORK_ERROR</span> - If there was an issue with a connection</li>
				<li><span class="returnValue">GJ_STATUS_REQUEST_FAILED</span> - If there was an issue with your input parameters.</li>
			</ul></td>
		</tr>
		<tr>
			<th><span id="GJ_fetch_userKeys">GJ_fetch_userKeys(now?)</span><br /><span id="GJ_fetch_globalKeys">GJ_fetch_globalKeys(now?)</span></th>
			<td>Downloads all of the keys stored in a user's data store or the global store. Can be accessed using the <a class="functionName" href="keyGetters">Data Key Getter</a> functions.<br /><br />
			<strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">GJ_STATUS_OK</span> - If placed in the background or if the data has been downloaded</li>
				<li><span class="returnValue">GJ_STATUS_INVALID_CALL</span> - If used incorrectly</li>
				<li><span class="returnValue">GJ_STATUS_NETWORK_ERROR</span> - If there was an issue with a connection</li>
				<li><span class="returnValue">GJ_STATUS_REQUEST_FAILED</span> - If there was an issue with your input parameters.</li>
			</ul></td>
		</tr>
		<tr>
			<th id="GJ_fetch_scores"><span id="GJ_fetch_tableScores">GJ_fetch_tableScores(now?, tableID, count)</span><br /><span id="GJ_fetch_tableScores_user">GJ_fetch_tableScores_user(now?, tableID, count)</span></th>
			<td>Fetch the amount of scores defined by count, from the table defined by the table ID. You can find your tables' ID's by going to the Achievements tab of your game editing page, and then clicking the High Scores tab. Also, an important note is that using <em>0</em> as the tableID assumes you are attempting to use the <em>Primary Table</em> of your game. You can assign a primary table on the same page.<br/>Using the _user function only downloads the data of the currently logged in user.<br /><br />
			<strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">GJ_STATUS_OK</span> - If placed in the background or if the data has been downloaded</li>
				<li><span class="returnValue">GJ_STATUS_INVALID_CALL</span> - If used incorrectly</li>
				<li><span class="returnValue">GJ_STATUS_NETWORK_ERROR</span> - If there was an issue with a connection</li>
				<li><span class="returnValue">GJ_STATUS_REQUEST_FAILED</span> - If there was an issue with your input parameters.</li>
			</ul></td>
		</tr>
			
		<!-- STORAGE FUNCTIONS -->
		<tr class="header"><th id="StoreFunctions" colspan="2">Storage Functions</th></tr>
			<tr><td colspan="2">Store functions (and all functions that actually connect to Game Jolt) have a special argument called <span class="returnValue">now?</span><br />When set to true, this argument will allow you to stop the game from continuing until all of the requested data has been downloaded. When false, it will be worked on in the background and the game will continue normally.</td></tr>
		<tr>
			<th id="GJ_store_data"><span id="GJ_store_userData">GJ_store_userData(now?, key)</span><br /><span id="GJ_store_globalData">GJ_store_globalData(now?, key)</span><br /><span id="GJ_store_userFile">GJ_store_userFile(now?, key, file)</span><br /><span id="GJ_store_globalFile">GJ_store_globalFile(now?, key, file)</span></th>
			<td>These functions store your local data storage onto the Game Jolt servers. It's important to note that you don't feed the new value into these functions. Rather, you use the <a href="#GJ_data_" class="functionName">Data Storage Functions</a> to modify them first before storing them. User storage requires a logged-in user and stores the data into their personal storage, whereas global storage stores the data into the global storage. Using the File version of these scripts will upload the contents of the given file to the Game Jolt server instead of the local data storage.<br /><br />
			<strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">GJ_STATUS_OK</span> - If placed in the background or if the data has been successfully uploaded</li>
				<li><span class="returnValue">GJ_STATUS_INVALID_CALL</span> - If used incorrectly</li>
				<li><span class="returnValue">GJ_STATUS_NETWORK_ERROR</span> - If there was an issue with a connection</li>
				<li><span class="returnValue">GJ_STATUS_REQUEST_FAILED</span> - If there was an issue with your input parameters.</li>
			</ul></td>
		</tr>
		<tr>
			<th id="GJ_store_trophyEarned">GJ_store_trophyEarned(now?, trophyID)</th>
			<td>When a user is logged in, this will set the given trophy ID as "earned" on the Game Jolt server.<br /><br />
			<strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">GJ_STATUS_OK</span> - If placed in the background or if the trophy has been earned</li>
				<li><span class="returnValue">GJ_STATUS_INVALID_CALL</span> - If used incorrectly</li>
				<li><span class="returnValue">GJ_STATUS_NETWORK_ERROR</span> - If there was an issue with a connection</li>
				<li><span class="returnValue">GJ_STATUS_REQUEST_FAILED</span> - If there was an issue with your input parameters.</li>
			</ul></td>
		</tr>
		<tr>
			<th id="GJ_store_score">GJ_store_score(now?, tableID, score, sort, extra data)</th>
			<td>When a user is logged in, this will store a new score for that user with the given information. Score is the string that will be displayed to viewers, while sort is the real value that the Game Jolt servers will use to give it a rank. Extra data is an optional string value where you can enter whatever you'd like.<br /><br />
			<strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">GJ_STATUS_OK</span> - If placed in the background or if the score was stored successfully</li>
				<li><span class="returnValue">GJ_STATUS_INVALID_CALL</span> - If used incorrectly</li>
				<li><span class="returnValue">GJ_STATUS_NETWORK_ERROR</span> - If there was an issue with a connection</li>
				<li><span class="returnValue">GJ_STATUS_REQUEST_FAILED</span> - If there was an issue with your input parameters.</li>
			</ul></td>
		</tr>
		<tr>
			<th id="GJ_store_guestScore">GJ_store_guestScore(now?, tableID, name, score, sort, extra data)</th>
			<td>This will work for anyone who doesn't have a Game Jolt account. You simply give them a name and that will be displayed on the high score table in place of a user. All the other parameters are equivalent to <a class="functionName" href="#GJ_store_score">GJ_store_score()</a>.<br /><br />
			<strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">GJ_STATUS_OK</span> - If placed in the background or if the score was stored successfully</li>
				<li><span class="returnValue">GJ_STATUS_INVALID_CALL</span> - If used incorrectly</li>
				<li><span class="returnValue">GJ_STATUS_NETWORK_ERROR</span> - If there was an issue with a connection</li>
				<li><span class="returnValue">GJ_STATUS_REQUEST_FAILED</span> - If there was an issue with your input parameters.</li>
			</ul></td>
		</tr>
			
		<!-- USER FUNCTIONS -->
		<tr class="header"><th id="UserFunctions" colspan="2">User Functions</th></tr>
		<tr><td colspan="2">The userID argument requested in the following functions are the userIDs found on the Game Jolt website. For example, my user ID is 1165 (as can be seen by visiting my <a target="_blank" href="http://gamejolt.com/profile/thatbrod/1165/">Game Jolt profile</a> (Look at the url!)) and so I'd access my data by using <span class="functionName">GJ_user_*(1165)</span><br /><br />
		Using <em>0</em> as the userID is a special case. It is recognized as the currently logged in user. Also, if <a class="functionName" href="#GJ_login">GJ_login()</a> is successful, <a class="functionName" href="#GJ_user_isReady">GJ_user_isReady(0)</a> will always return true until that user logs out.</td></tr>
		<tr>
			<th id="GJ_user_isReady">GJ_user_isReady(userID)</th>
			<td><strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">true</span> - If the corresponding user has been downloaded</li>
				<li><span class="returnValue">false</span> - Otherwise</li>
			</ul></td>
		</tr>
		<tr>
			<th id="GJ_user_getLoggedID">GJ_user_getLoggedID()</th>
			<td><strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">userID</span> - user ID of currently logged in user</li>
				<li><span class="returnValue">-1</span> - If no user is currently logged in</li>
			</ul>
			</td>
		</tr>
		<tr>
			<th id="userGetters">
				<span id="GJ_user_getAvatar">GJ_user_getAvatar(userID)</span><br />
				<span id="GJ_user_getDevDescription">GJ_user_getDevDescription(userID)</span><br />
				<span id="GJ_user_getDevName">GJ_user_getDevName(userID)</span><br />
				<span id="GJ_user_getDevWebsite">GJ_user_getDevWebsite(userID)</span><br />
				<span id="GJ_user_getLastLogIn">GJ_user_getLastLogIn(userID)</span><br />
				<span id="GJ_user_getMemberAge">GJ_user_getMemberAge(userID)</span><br />
				<span id="GJ_user_getName">GJ_user_getName(userID)</span><br />
				<span id="GJ_user_isAdministrator">GJ_user_isAdministrator(userID)</span><br />
				<span id="GJ_user_isBanned">GJ_user_isBanned(userID)</span><br />
				<span id="GJ_user_isDeveloper">GJ_user_isDeveloper(userID)</span><br />
				<span id="GJ_user_isGamer">GJ_user_isGamer(userID)</span><br />
				<span id="GJ_user_isModerator">GJ_user_isModerator(userID)</span>
			</th>
			<td>
				These are the "getter" functions associated with users.<br /><br />
				
				<strong>Returns: </strong>
				<ul>
					<li><span class="returnValue">Corresponding value</span> - If the user is ready</li>
					<li><span class="returnValue">N/A (string)</span> - If the user is not ready</li>
					<li><span class="returnValue">false (real)</span> - If the user is not ready and using an <span class="functionName">is*</span> getter</li>
					<li><span class="returnValue">noone (-4)</span> - For getAvatar, when either the avatar has not finished downloading, does not exist at all, or if user isn't ready.</li>
				</ul>
			</td>
		</tr>
		
		<!-- TROPHY FUNCTIONS -->
		<tr class="header"><th id="TrophyFunctions" colspan="2">Trophy Functions</th></tr>
		<tr><td colspan="2">The trophyID argument used in the following functions are the same found on the Game Jolt achievements tab of your game. Each trophy you add will have its own ID, which you use here to access the trophy data. Of course, you could always just  set the trophy data in your game yourself, but the option is available to you.<br/><br/>To have users achieve trophies, see <a href="#GJ_store_trophyEarned" class="functionName">GJ_store_trophyEarned()</a>.</td></tr>
		<tr>
			<th id="GJ_trophiesReady">GJ_trophiesReady()</th>
			<td><strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">true</span> - If trophies have been downloaded</li>
				<li><span class="returnValue">false</span> - Otherwise</li>
			</ul></td>
		</tr>
		<tr>
			<th id="trophyGetters">
			<span id="GJ_trophy_getImage">GJ_trophy_getImage(trophyID)</span><br />
			<span id="GJ_trophy_getTimeAchieved">GJ_trophy_getTimeAchieved(trophyID)</span><br />
			<span id="GJ_trophy_getDescription">GJ_trophy_getDescription(trophyID)</span><br />
			<span id="GJ_trophy_getTitle">GJ_trophy_getTitle(trophyID)</span><br />
			<span id="GJ_trophy_isAchieved">GJ_trophy_isAchieved(trophyID)</span><br />
			<span id="GJ_trophy_isBronze">GJ_trophy_isBronze(trophyID)</span><br />
			<span id="GJ_trophy_isGold">GJ_trophy_isGold(trophyID)</span><br />
			<span id="GJ_trophy_isSilver">GJ_trophy_isSilver(trophyID)</span><br />
			<span id="GJ_trophy_isPlatinum">GJ_trophy_isPlatinum(trophyID)</span></th>
			<td>These are the "getter" functions associated with trophies.<br /><br />
			
			<strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">Corresponding value</span> - If the trophies are ready</li>
				<li><span class="returnValue">N/A (string)</span> - If the trophies aren't ready</li>
				<li><span class="returnValue">false (real)</span> - If the trophies aren't ready and using an <span class="functionName">is*</span> getter</li>
				<li><span class="returnValue">noone (-4)</span> - For getImage, when either the image has not finished downloading, does not exist at all, or if the trophies aren't ready.</li>
			</ul></td>
		</tr>
		
		<!-- DATA STORAGE FUNCTIONS -->
		<tr class="header"><th id="GJ_data_" colspan="2">Data Storage Functions</th></tr>
		<tr>
			<th id="GJ_data_user_getters"><span id="GJ_data_user_getReal">GJ_data_user_getReal(key)</span><br /><span id="GJ_data_global_getReal">GJ_data_global_getReal(key)</span></th>
			<td><strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">Value</span> - the <em>local</em> data (as a real number) associated with the given key and data type.</li>
				<li><span class="returnValue">Empty String</span> - When the data has not been downloaded or allocated yet.</li>
			</ul></td>
		</tr>
		<tr>
			<th id="GJ_data_global_getters"><span id="GJ_data_user_getString">GJ_data_user_getString(key)</span><br /><span id="GJ_data_global_getString">GJ_data_global_getString(key)</span></th>
			<td><strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">Value</span> - the <em>local</em> data (as a string) associated with the given key and data type.</li>
				<li><span class="returnValue">Empty String</span> - When the data has not been downloaded or allocated yet.</li>
			</ul></td>
		</tr>
		<tr>
			<th id="GJ_data_exists"><span id="GJ_data_user_exists">GJ_data_user_exists(key)</span><br /><span id="GJ_data_global_exists">GJ_data_global_exists(key)</span></th>
			<td><strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">true</span> - The <em>local</em> data has been downloaded or allocated.</li>
				<li><span class="returnValue">false</span> - Otherwise.</li>
			</ul></td>
		</tr>
		<tr>
			<th id="GJ_data_set"><span id="GJ_data_user_set">GJ_data_user_set(key, value)</span><br /><span id="GJ_data_global_set">GJ_data_global_set(key, value)</span></th>
			<td>Inserts the information into the <em>local</em> data storage, linking it to the given key. You need to change your information through these functions <em>before</em> uploading the key onto the servers.<br/><br/>
			<strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">GJ_STATUS_INVALID_CALL</span> - If you try to set user data while not logged in.</li>
			</ul></td>
		</tr>
		<tr>
			<th id="GJ_data_key_get"><span id="GJ_data_user_key_get">GJ_data_user_key_get(n)</span><br /><span id="GJ_data_global_key_get">GJ_data_global_key_get(n)</span></th>
			<td><strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">key name</span> - Returns the nth key that you have used in your <em>local</em> storage.</li>
			</ul></td>
		</tr>
		<tr>
			<th id="GJ_data_key_count"><span id="GJ_data_user_key_count">GJ_data_user_key_count()</span><br /><span id="GJ_data_global_key_count">GJ_data_global_key_count()</span></th>
			<td><strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">count</span> - The number of keys you have stored in your <em>local</em> storage.</li>
			</ul></td>
		</tr>
		<tr>
			<th id="GJ_data_key_find"><span id="GJ_data_user_key_find">GJ_data_user_key_find(key)</span><br /><span id="GJ_data_global_key_find">GJ_data_global_key_find(key)</span></th>
			<td><strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">n</span> - ID value associated with the given key.</li>
				<li><span class="returnValue">-1</span> - If the given key does not exist <em>locally</em>.</li>
			</ul></td>
		</tr>
		
		
		<!-- TABLE FUNCTIONS -->
		<tr class="header"><th id="GJ_table_" colspan="2">Highscore Table Functions</th></tr>
		<tr>
			<th id="GJ_table_get_size"><span id="GJ_table_size">GJ_table_size(tableID)</span><br />
			<span id="GJ_table_user_size">GJ_table_user_size(tableID)</span><br /></th>
			<td><strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">size</span> - The number of scores stored in the given tables.</li>
			</ul></td>
		</tr>
		<tr>
			<th id="GJ_tablesReady">GJ_tablesReady()<br /></th>
			<td><strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">true</span> - The information of the tables have been downloaded, and score-related-functions can now be run.</li>
				<li><span class="returnValue">false</span> - The information is still being downloaded.
			</ul></td>
		</tr>
		<tr>
			<th id="GJ_score_getters">
			<span id="GJ_tableScore_getExtraData">GJ_tableScore_getExtraData(tableID, rank)</span><br />
			<span id="GJ_tableScore_getGuestName">GJ_tableScore_getGuestName(tableID, rank)</span><br />
			<span id="GJ_tableScore_getScoreName">GJ_tableScore_getScoreName(tableID, rank)</span><br />
			<span id="GJ_tableScore_getSortValue">GJ_tableScore_getSortValue(tableID, rank)</span><br />
			<span id="GJ_tableScore_getUserID">GJ_tableScore_getUserID(tableID, rank)</span><br />
			<span id="GJ_tableScore_getUserName">GJ_tableScore_getUserName(tableID, rank)</span><br />
			<span id="GJ_tableScore_isGuestScore">GJ_tableScore_isGuestScore(tableID, rank)</span><br />
			<span id="GJ_tableScore_isUserScore">GJ_tableScore_isUserScore(tableID, rank)</span><br />
			<span id="GJ_tableScore_user_getExtraData">GJ_tableScore_user_getExtraData(tableID, rank)</span><br />
			<span id="GJ_tableScore_user_getScoreName">GJ_tableScore_user_getScoreName(tableID, rank)</span><br />
			<span id="GJ_tableScore_user_getSortValue">GJ_tableScore_user_getSortValue(tableID, rank)</span></th>
			<td>The Getter functions associated with table scores. In order to use these, you need to fetch the scores and then access their data through these functions. Rank is a value from (0 to GJ_table_size(tableID) - 1), with 0 is the best score.<br/><br/>
			<strong>Returns: </strong>
			<ul>
				<li><span class="returnValue">Value</span> - The value you requested based on the tableID, rank, and _user vs global scores.</li>
				<li><span class="returnValue">N/A</span> - When using the get* functions for string values and the rank and/or tableID can not be found.</li>
				<li><span class="returnValue">0</span> - When using the get* functions for real values and the rank and/or tableID can not be found.</li>
				<li><span class="returnValue">-1</span> - When using getUserID function and the rank and/or tableID can not be found.</li>
				<li><span class="returnValue">false</span> - When using the is* functions and the rank and/or tableID can not be found.</li>
			</ul></td>
		</tr>
	</table>
</div>
</body>
</html>
