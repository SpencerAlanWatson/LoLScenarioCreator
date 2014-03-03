#LoLScenarioCreator
A NodeJS server which creates and sends replays to the [League of Legends](http://leagueoflegends.com) clients, in which clients can create their own.

##Shoutouts & Stuff
First,  a must:  

League of Legends Scenario Creator isn’t endorsed by Riot Games and doesn’t reflect the views or opinions of Riot Games or anyone officially involved in producing or managing League of Legends. League of Legends and Riot Games are trademarks or registered trademarks of Riot Games, Inc. League of Legends © Riot Games, Inc.

Second, 
> ❤ [League of Legends](http://leagueoflegends.com)  
> ❤❤ [Riot Games](http://www.riotgames.com/)  
> ❤ [GitHub](http://www.github.com)  
> ❤ [Adobe Brackets](http://brackets.io/)  

I primarily use Adobe Brackets as my editor for this, which I linked to above.  

I just felt that it was about time that I reference some awesome people, and things I use for this project.  

##Librarys This Project Currently Uses
This project runs on [Nodejs](http://nodejs.org/), and uses [NPM](https://www.npmjs.org/‎).
This Project also uses several libraries/frameworks, including:  
*  [Express](http://expressjs.com/)   
    *  For general routing purposes, and other useful features.
*  [Request](https://github.com/mikeal/request)  
    *  To easily make http requests.
*  [Underscore](http://underscorejs.org/)
    *  For general features.
*  [Async](https://github.com/caolan/async)  
    *  Asyncronous things! =)  
*  [Simple Bufferstream](https://github.com/rvagg/node-simple-bufferstream)
    *  Easily make a buffer into a readable stream.
    

##Contributing
All contributions are welcomed, and I would love for some help!  IF you have comments, think you can  help improve this, or just want to want to give some information, please do!  I would love it if you would contribute!  
  
I try to follow [GitHub-flow](http://scottchacon.com/2011/08/31/github-flow.html)

##Current State
Currently, in an effort to learn about how the spectate client works, right now it records responses from Riot spectate servers if you tell league to connect to it.  It only connects to NA right now.  It saves responses in a .json file, be warned though, the files are big because of all the octets in there, but it is encoded in utf8.  I would recommend the fanc and awesome text editior [Sublime Text](http://www.sublimetext.com/) for these large files, because it handles large files like a champ.  It's also all around pretty sweet.  

##What I know right now
So, when I started looking into doing this, I quickly found that information like this is hard to find.  
I have been learning, and still am learning, about how the spectate client works.  

This is what I know on Friday Febuary 28th, 2014 at 4:23 PM for Patch 4.3
Note, any url path segment with a : in it is a variable and the name is what I think/know it is.  

*  To get your league client to connect to this app to record the data, you do
    *"C:\Riot Games\League of Legends\RADS\solutions\lol_game_client_sln\releases\0.0.1.12\deploy\League of Legends.exe" "8394" "LoLLauncher.exe" "" "spectator yourServerIP:3000 encryptionKey gameid region"
    *  NOTE:  IF you getting immediately bug splatted, it may be that you are getting access violation errors, you can fix this by cd'ing into the directory and just launching it "League of Legends.exe" then command line stuff.  At least that's what happend to me.
    *  Where yourServerIP is this apps address(if locally hosted then localhost)
    *  encryptionKey is the observer encryption key (more data need on this, as the client communicates on http, not https) ex: RWG1pPWbRSXgcSXJiej9moJuM05d2CJ9
    *  gameID is the id of the game ex: 1292132254
    *  region is the server your on ex: NA1
    *  I usually use [quickfind's beta site's](http://quickfind-beta.kassad.in/) current game specate feature to get this info, and then replace the server ip with my own.  
    *  I wonder what the reason for 8394 is, and the empty "", maybe it could be useful for us.
*  Spectate data is sent through http, all but one is GET (the odd-ball one is POST) and is either json, or octet-stream responses.
*  The requests the client makes are as follows 
    *  The first request is a GET request for the verison /observer-mode/rest/consumer/version
        *  My guess it is used for making sure the server's and clients verisons are matching.  
    *  The second request is a GET request for the meta data of the game /observer-mode/rest/consumer/getGameMetaData/:region/:gameid/:unknown/token 
        *  And this contains the game id, and info on pending/avaliable chunks.
    *  The third request (which is the first request that will be repeated multiple times) is a GET request for the last chunks info /observer-mode/rest/consumer/getLastChunkInfo/:region/:gameID/:delay/token
        *  This seems to be used to (obviously) get info on the last chunk to become avaliable,
        *  but less obviously is it also seems to be used to see if a new chunk is avaliable, which means that the client sends this request often enough.
    *  The fourth request is a GET request for game chunk data /observer-mode/rest/consumer/getGameDataChunk/:region/:gameID/:chunkID/token
        *  This is the meat of the data, it returns a octet-stream (read: binary) and is what we are trying to figure out how to manipulate so we can get the client to do our clients their bidding.  
    *  The fifth request is a GET request for key frame data /observer-mode/rest/consumer/getKeyFrame/:region/:gameID/:keyID/token
        *  Like the game data, this is the tender, succulent meat (or veggie burger if you are a vegitarian), and it also returns a octet-stream (read: binary) and is part of what we are trying to figure out.
    *  The sixth, and final (not always made) request is a POST request for end /observer-mode/rest/consumer/end/:region/:gameid/:timestamp/token
        *  This request seems to only be made when the client is ending the replay pre-maturely.  If the client watches the full game, then they don't send this request.
    *  The game's main loop of requests are 3, 4, 5, and unless there is an error (in which it will send a 6), that's pretty much it.  
*  NA's spectate server is 216.133.234.17:8088
    *  Note: the server may do more then just handle spectating.
    
##Who am I?
I am a 15 year old high school student who loves learning, technology, software creation, a avid fan of league of legends, and a huge fan of Riot Games.  I have done a fair bit in Javascript, but I am trying to learn more about everything I can in software, and technology in general, which is one of the reasons I started this project.  When I was 14 I was an web development intern for Assurity Life Insurance, which was awesome and showed to me that I really love development.  

The reason I even mention this is because I know this is my first major public project, and I really want contributers, critiques, and to learn.
