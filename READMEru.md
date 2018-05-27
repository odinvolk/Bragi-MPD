# Bragi-MPD

Bragi-MPD a fully featured, HTML5, mobile friendly MPD web client made using [MPD.js](https://github.com/bobboau/MPD.js). Bragi-MPD is intended to allow you to play your music the way you want, wherever you are, and with any device capable of loading a web page. You can manage playlists, search your music database, or manually navigate your music files. Inherently multi-user friendly it is like its namesake, great at parties.

Bragi поддерживает управление несколькими выходами на экземпляр MPD и может быть настроен для управления несколькими экземплярами (на нескольких хостах), что идеально подходит для управления музыкой в зонированных звуковых системах.

Функции
--------

Bragi стремится стать полнофункциональным MPD-клиентом, хотя он работает, и новые функции всегда добавляются, цель состоит в том, чтобы не оставлять лишнюю функциональную возможность.

 * Responsive design
  * fits mobile just as well as traditional desktop platforms
  * works on any modern web browser from Chrome to (recent versions of) IE to Safari (iOS and OSX) to Firefox
 * Playback control (available in all interfaces)
  * play/pause
  * volume control
  * seek
  * previous/next
  * display of metadata of currently playing song
 * Queue management
  * play a song on the queue
  * add songs by various means (mentioned elsewhere, i.e. search, playlist, manual file lookup)
  * remove a song from the queue
  * reorder songs on the queue
  * show currently playing song on the queue
  * remove currently playing song from the queue  
  * save queue as playlist
  * clear queue
 * Playlists
  * add song(s) in a playlist to the queue
  * add song from a playlist to the queue and play
  * remove song from playlist
  * reorder songs in playlist
  * delete playlist
  * rename playlist
 * File exploration
  * view and explore the MPD file database directly
  * add individual songs from a directory to queue / playlist
  * add all songs from a directory to queue / playlist
 * Search
  * search based on progressively filtered tags
  * free form search on all tags
  * add results to queue or playlist
 * Settings
  * change MPD settings, such as random, single play, crossfade, and consume
 * Outputs
  * enable/disable outputs manually or switch cleanly
 * Instances
  * control multiple MPD instances (requires configuration)
 * Configurable UI
  * mark certain UI elements as disabled via hierarchical CSS selectors in the disabled_features section
  * load custom CSS overriding the default to make your installation your own (BTW I'm looking for additional themes to bundle if you happen to come up with any)
 * Streaming

Screenshots
  -----------
![queue](https://raw.githubusercontent.com/wiki/bobboau/Bragi-MPD/img/screenshots/Bragi-queue.png)
Possibly the most important UI screen in the application, this is where you control and organize the list of music currently playing.

![playlist](https://raw.githubusercontent.com/wiki/bobboau/Bragi-MPD/img/screenshots/Bragi-playlist.png)
Here you can select from a list of saved playlists. You can edit them in the same way as the queue, overwrite/rename/delete them load into the queue by replacing the queue or appending to it.

![search](https://raw.githubusercontent.com/wiki/bobboau/Bragi-MPD/img/screenshots/Bragi-search.png)
Explore your music library by a tag based drill down. Adding additional tags to the search criteria will have their possible values filtered by preceding tags, so if you specify an artist and then add an album criteria only albums from that artist will be shown. Additional criteria will have only the values that have matches on that artist and album

![files](https://raw.githubusercontent.com/wiki/bobboau/Bragi-MPD/img/screenshots/Bragi-files.png)
If you have your database organized in a way you like you can still get into the low level file structure. You are not penalized if you don't want to use the fancy pants search feature and just want to deal with the files directly. You can drill down into a directory and you are given a breadcrumb trail that both describes where you are in the directory tree and gives you a shortcut back to a higher level.

![ouputs](https://raw.githubusercontent.com/wiki/bobboau/Bragi-MPD/img/screenshots/Bragi-outputs.png)
If you have more than one output defined for an instance the outputs tab will be present. Here you can turn them on and off. Someday if MPD supports it it would be nice to also set relative volume here, alas that has not yet been implemented in MPD yet.

![instances](https://raw.githubusercontent.com/wiki/bobboau/Bragi-MPD/img/screenshots/Bragi-instances.png)
If you have more than one instance configured, here you can switch between them. Password management is also handled in this screen if you have more than one instance (if you just have one instance you will be prompted)

![settings](https://raw.githubusercontent.com/wiki/bobboau/Bragi-MPD/img/screenshots/Bragi-settings.png)
Basic MPD play settings can be controlled by Bragi-MPD too.

Installation and Setup
----------------------

У вас должна быть [рабочая установка MPD](http://www.musicpd.org/doc/user/) для многих дистрибутивов Linux, это немного больше, чем 'apt-get install mpd'.

После того, как вы работаете с MPD, вам нужно будет развернуть экземпляр [Websockify](https://github.com/kanaka/websockify). После того, как вы загрузили Websockify, и вы перешли к его каталогу, и если вы используете стандартный порт для MPD, вы можете запустить его экземпляр, который позволит вам начать работу с MPD.js с помощью следующей команды:

    ./run 8800 localhost:6600

Версия всех зависимостей на стороне клиента Bragi-MPD была построена с включенными в репо. Вам просто нужно клонировать репо или загружать заархивированную копию и извлекать ее в веб-сервер веб-сервера. Apache или lighttpd были бы хорошим выбором для этого. Если вам не хочется настраивать полный веб-сервер в средах Linux и просто хотите быстро протестировать его, вы можете обслуживать файлы с помощью этой команды из корня папки repo.

    python -m SimpleHTTPServer

После этого вы сможете начать играть свою музыку, перейдя на ваш сервер в веб-браузере.

Configuration
-------------

Configuration is done in the config.js file. The default config file (default_config.js) has one instance on one host setup. If you wish to configure Bragi-MPD differently copy this file and rename it to just 'config.js' and it will override the default. The config file is basically a json object with a few sections.

The first section is defined by the key 'clients'. This is the list of MPD instances that Bragi-MPD knows about and can control. If only one client is set up (as it is in the default config) the instances tab will not be shown in the interface, otherwise you will be able to switch which instance you are controlling in the instance tab. You can have as many instances as you want. Each instance allows you to specify a name (only used in the UI), port number (default 8800), and host name (if not specified it will use the root of the host name you loaded the page from). Each client instance can optionally specify stream_port (for listening to an MPD HTTP stream), local_volume (to use client-side volume), and disabled_features, which is a listing of CSS selectors describing UI items you want to hide. This can be useful if you want to have a public client that allows some limited subset of functionality. Like maybe you don't want everyone at your party to be able to set the volume or delete your playlists (Keep in mind disabling UI elements is just a usability tweak and if your MPD instance still allows it an enterprising party-goer could easily install their own separate MPD client and bypass this so, it's advisable to set permissions appropriately for MPD if this is a concern of yours). On the subject of permissions, you can also force requesting a password by specifying needs_auth:true for a client, default behavior will be to allow all actions and if a authorization failure happens to request a password (which is cached in browser local storage).

There is also a 'theme' section, here you can specify an array of additional CSS files to load. If the theme key is present it is expected to be an array full of strings which are URLs of css files (relative to the page's url). These additional files will be loaded following the normal files and will override them assuming they have equal or greater [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity).

For more config details see the [wiki page](https://github.com/bobboau/Bragi-MPD/wiki/Configuration)

MPD configuration for streaming
-------------------------------

Your mpd.conf needs at least one httpd output section for streaming to work. In most cases, the encoder should be LAME (MP3). If you're running MPD on a low power device, such as a router, shine is a usable alternative, but audio quality will suffer noticeably for any given bitrate.

    audio_output {
    	type            "httpd"           #mandatory for streaming
    	name            "HTTP MP3 stream" #name it anything you like
    	encoder         "lame"            #or "shine" if lame uses too much cpu time
    	bitrate         "160"             #160kbps seems like a good balance of quality and low mobile data usage
    	port            "58000"           #port where MPD listens for streaming clients, this is what you'll put in stream_port in config.js
    	bind_to_address	"0.0.0.0"         #listen on all network interfaces
    	format          "44100:16:2"      #be sure to use a fixed stream format, like this one (44KHz, 16 bit, stereo)
    	always_on       "no"              #not needed for Bragi, enable only if necessary for other clients
    	max_clients     "4"               #0 for no limit
    	tags            "yes"             #for more track info, especially in other clients
    }
