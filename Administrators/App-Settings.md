The app settings can be found in the administrative area of Nextcloud.

## Legacy Api Support
The Legacy API is the API which was originally provided by the Passwords App in versions prior to 2018.1.
This API is used by many clients for passwords and therefore still available.
However the API does not support client side encryption or safe server side encryption.
It is also makes the application slower and does not strictly enforce HTTPS.

#### Enable Legacy API
This option enables or disables the API completely.
If the API is disabled it will no longer be possible to access it in any way as the app will no longer register the necessary components in Nextcloud.

**Note:** The browser extension does not support the new api in versions prior to 2.0.0.

#### Legacy API was last used on
This setting is read only.
It should tell you when the legacy api was last used.
If you can see that the api is no longer in use on your server, you should disable it.



## Internal Data Processing
These settings influence how Passwords processes different types of data internally.

#### Image Rendering
You have two options for image rendering.
If Imagemagick or Graphicsmagick are installed on your system, they will automatically be chosen as default.
GDLib should only be chosen if Imagemagick is broken or not available.
GDLib usually provides lower image quality and might not work with all formats.

**Note:** If you have Imagemagick selected, make sure that svg is supported. Otherwise GDLib will be used in some cases.



## External Services
In this section you can configure all the third party services used by Passwords.


#### Password Security Checks
This service is used to check if a password is safe or not.

**Have I been pwned?** is the recommended service.
[haveibeenpwned.com](https://haveibeenpwned.com/) stores the SHA-1 hashes of billions of compromised passwords.
Their database receives regular updates with lists of passwords used by hackers to attempt to crack accounts.
The app uses their [k-anonymity api](https://www.troyhunt.com/ive-just-launched-pwned-passwords-version-2/#cloudflareprivacyandkanonymity) to download a subset of hashes and does the comparison locally.
The app never sends SHA-1 hashes to the api.

**10 Million Passwords** downloads a static password file and fills the local cache with it.
After this, all password checking can be done locally.
Unlike Hibp it does not need to make a request for each check, but the database is a lot smaller and not up to date.
Updating the database can take up to 1.75Gib of RAM or up to 125MiB on less powerful systems.
If you do not have that much RAM available, you should not use this service.
It also requires around 512Mib of disk space.

**1 Million Passwords** downloads a static file with the most common passwords.
It uses a lot less system resources than the 10Mio passwords and should run on any system.
The database is considerably smaller and not up to date.

**10 Mio Passwords + Have I been pwned?** fills the local cache with the most common passwords.
This service fills the local database with the 10 million passwords and therefore reduces the amount of requests to Hibp.


#### Password Generator Service
This service will be used to generate the basic words for a new password.

**Local Dictionary** detects and uses locally installed dictionaries for english, german, french, italian, spanish and portuguese..
Actually available options depend on which dictionaries are installed on the server.

**Leipzig Corpora Collection** supports english, german, french, italian, spanish, portuguese, dutch, dansk, czech and polish.
The service returns random words from a randomly selected corpora and has the largest language support.

**watchout4snakes.com** is only available in english.
It can provide words based on their rarity and type and is therefore a great service to generate easy to remember and secure passwords.

**Random Characters** generates sets of random letters for the service.
This service has no dependencies.


#### Favicon Service
This service delivers the website favicons .
The icons are only fetched once for a domain and then stored locally.

**Local analyzer** fetches the start page of the domain and searches for common icon tags.
This service usually finds the most icons, but also the most useless icons.

**Besticon** uses a [besticon](https://github.com/mat/besticon) third party service to find icons.
It usually returns the best icons and also good default icons if none is found.
If no api url is provided, our shared Besticon instance will be used.
The service can be self hosted by following our [tutorial](./Besticon-Self-Hosting).
The url for the service can be defined in the settings. Any compatible api is accepted.

**favicongrabber.com** is free, requires no software and delivers good icons.
There is an api request limit which means that it can be slower.

**DuckDuckGo** uses the icon service of the search engine.
All icons have a native resolution of 32 pixels.

**Google** uses googles icon service.
It finds the least icons and they usually have a crappy resolution.

**None** always returns a default icon.
It is the fastest and most privacy friendly service.


#### Favicon Service Api
If you use a service with an API, you can enter the url here.


#### Website Preview Service
This service is used to generate previews of websites.
Only the front page of the domain is called and at maximum twice for mobile and desktop views.
If you know a good program or service, feel free to open an issue to support it.
(Requirements: Offers a free plan and has an api)

**Pageres CLI** requires [pageres-cli](https://github.com/sindresorhus/pageres-cli) to installed locally.
Usually very reliable local and headless preview generator with a modern browser engine.
If the installation with NPM fails, try `sudo npm install --global pageres-cli --unsafe-perm`.
If you are using a docker container, add `--cap-add=SYS_ADMIN` to the docker command to enable the chrome sandbox.

**Browshot** offers 100 free screenshots per month.
The api offers HTTPS by default, you can view the screenshots in your account and you can buy additional screenshots as you need.
Passwords will check your account and use free screenshots if possible. 
(Instance 27 is used for desktop and instance 67 for mobile.)
If your account balance allows it, passwords will use premium instances if no free screenshots are left.
(Instance 58 is used for desktop and instance 275 for mobile.)
You can specify the premium instance to use with the config keys `service/preview/bws/mobile` and `service/preview/bws/desktop` manually.

**screeenly** offers unlimited free screenshots and [self hosting](https://github.com/stefanzweifel/screeenly/wiki/Requirements-and-Install)
It has HTTPS by default and usually creates proper screenshots.
You can either just enter an api key and use the hosted version at [screeenly.com](https://secure.screeenly.com/) or enter a full url like `https://secure.screeenly.com/api/v1?key=yourapikey` where everything before `?key=` is the api url and the key is your api key.

**screenshotlayer** offers 100 free screenshots per month.
If you need more, you have to buy a subscription.
Triggers the bot protection on more websites and HTTPS is not supported.

**screenshotmachine.com** offers 100 fresh screenshots for free per month (accumulative) and impressions are free.
You pay what you use, it is quite fast and supports different devices.
HTTPS is not supported.

**None** just delivers one of five default images.


#### Website Preview API Key
If you use "Browshot", "screenshotlayer" or "screenshotmachine.com", you will have to provide an api key here.
Otherwise these services will not work.



## Default Email Settings
These settings can be overwritten by the user.

#### Send emails for security events
Enable emails for security relevant events by default.
This will enable emails for bad passwords.

#### Send emails for sharing events
Send emails when a password was shared with an user.



## Backup Settings
Passwords makes regular backups of the raw password database.
These backups can be used to restore the entire database or the database of a specific user.

#### Backup Interval
Specifies the interval in which backups should be created automatically.
The default value is `Every Day`.
You can also create backups manually with the command line command.

**Note:** You can not disable automated backups since we _really_ can't help you when you loose your data.

#### Amount of backups to keep
Specifies the amount of backups to keep.
If the maximum is reached, the oldest backup will be deleted.
This setting also includes manually created backups.
The default value is `14`, setting the value to `0` will keep all backups.
The shorter your backup interval is, the higher this setting should be to cover at least two weeks.



## Other Settings

#### Remove deleted objects from database
Specifies the time after which passwords, folders and tags deleted by the user will be removed from the database permanently.
This setting does not affect the data of deleted users which will always be deleted permanently.

#### Show Nightly Updates in "Apps"
This setting will modify the Nextcloud core to enable the installation of nightly updates for the passwords app.

#### Server survey participation
The server survey will send us some anonymous data of your server once a week.
This helps us to plan the future development of the app.
You can either contribute basic data (Nextcloud, App and PHP version) or full data (App Settings, Encryption usage) or no data at all.
You can read more about this [here](./Server-Survey) or take a look at our [statistics](https://ncpw.mdns.eu/) generated from the data. 


## Caches
Caches are used to store temporary data. they are usually not emptied by the app.
If problems occur, the first tip is always to empty the related cache.

#### Default Cache
Usually not used. Contains general files.

#### Avatars Cache
Contains rendered images of user avatars.

#### Favicon Cache
Contains the raw and scaled favicons.
This cache can not be cleared if you are using the shared BestIcon instance.

#### Pageshot Cache
Contains the raw website screenshots and resized or cropped versions.

#### Passwords Cache
Contains lists with bad passwords.