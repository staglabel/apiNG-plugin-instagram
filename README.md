[logo]: http://aping.io/logo/320/aping-plugin.png "apiNG Plugin"
![apiNG][logo]

**_apiNG-plugin-instagram_** is a [Instagram REST API](https://www.instagram.com/developer/endpoints/media/) plugin for [**apiNG**](https://github.com/JohnnyTheTank/apiNG).

# Information
* **Supported apiNG models: `social`, `video`, `image`**
* Used promise library: [angular-instagram-api-factory](https://github.com/JohnnyTheTank/angular-instagram-api-factory) _(included in minified distribution file)_

# Documentation
    I.   INSTALLATION
    II.  ACCESS TOKEN
    III. USAGE

## I. INSTALLATION
    a) Get files
    b) Include files
    c) Add dependencies
    d) Add the plugin

### a) Get files
You can choose your preferred method of installation:

* Via bower: `bower install apiNG-plugin-instagram --save`
* Download from github: [apiNG-plugin-instagram.zip](https://github.com/JohnnyTheTank/apiNG-plugin-instagram/zipball/master)

### b) Include files
Include `aping-plugin-instagram.min.js` in your apiNG application
```html
<script src="bower_components/apiNG-plugin-instagram/dist/aping-plugin-instagram.min.js"></script>
```

### c) Add dependencies
Add the module `jtt_aping_instagram` as a dependency to your app module:
```js
var app = angular.module('app', ['jtt_aping', 'jtt_aping_instagram']);
```

### d) Add the plugin
Add the plugin's directive `aping-instagram="[]"` to your apiNG directive and configure your requests (_**III. USAGE**_)
```html
<aping
    template-url="templates/social.html"
    model="social"
    items="20"
    aping-instagram="[{'tag':'camping'}]">
</aping>
```

## II. ACCESS TOKEN
    a) Generate your `access_token`
    b) Insert your `access_token` into `aping-config.js`

### a) Generate your `access_token`
1. Login on [instagram.com/developer](https://www.instagram.com/developer/)
2. Navigate to [instagram.com/developer/clients/manage](https://www.instagram.com/developer/clients/manage/)
3. Register a New Client
4. Generate access_token
    * Open `https://api.instagram.com/oauth/authorize/?client_id=CLIENT-ID&redirect_uri=REDIRECT-URI&response_type=code` with information from http://instagram.com/developer/clients/manage/
    * OR via curl:
```
curl \-F 'client_id=CLIENT-ID' \
    -F 'client_secret=CLIENT-SECRET' \
    -F 'grant_type=authorization_code' \
    -F 'redirect_uri=YOUR-REDIRECT-URI' \
    -F 'code=CODE' \
    https://api.instagram.com/oauth/access_token
```


### b) Insert your `access_token` into `aping-config.js`
Open `js/apiNG/aping-config.js` in your application folder. It should be look like this snippet:
```js
apingApp.config(['$provide', function ($provide) {
    $provide.constant("apingApiKeys", {
        //...
        instagram : [
            {'access_token':'<YOUR_INSTAGRAM_ACCESS_TOKEN>'},
        ]
        //...
    });

    $provide.constant("apingDefaultSettings", {
        //...
    });
}]);
```

:warning: Replace `<YOUR_INSTAGRAM_ACCESS_TOKEN>` with your instagram `access_token`

## III. USAGE
    a) Models
    b) Requests
    c) Rate limit

### a) Models
Supported apiNG models

|  model   | content | support | max items<br>per request | (native) default items<br>per request |
|----------|---------|---------|--------|---------|
| `social` | **images**, **videos** | full    | `33`   | `20`   |
| `image`  | **images** | partly    | `33`   | `20`   |
| `video`  | **videos** | partly    | `33`   | `20`   |

**support:**
* full: _the source platform provides a full list with usable results_ <br>
* partly: _the source platfrom provides just partly usable results_


### b) Requests
Every **apiNG plugin** expects an array of **requests** as html attribute.

#### Requests by Tag
|  parameter  | sample | default | description | optional |
|----------|---------|---------|---------|---------|
| **`tag`** | `camping` |  | Search Tag (without #) | no |
| **`items`**  | `10` | `20` | Items per request (`0`-`33`) |  yes  |

Sample requests:
* `[{'tag':'soccer'}, {'tag':'nofilter'}]`
* `[{'tag':'eagle', 'items':10}]`

#### Requests by User
|  parameter  | sample | default | description | optional |
|----------|---------|---------|---------|---------|
| **`userId`** | `416104304` |  | The `userId` of an instagram user<br>[Username to userId converter](http://jelled.com/instagram/lookup-user-id) | no |
| **`items`**  | `10` | `20` | Items per request (`0`-`33`) |  yes  |

Sample request:
* `[{'userId':'416104304', 'items':10}]`

#### Requests by Coordinates
|  parameter  | sample | default | description | optional |
|----------|---------|---------|---------|---------|
| **`lat`** | `-13.163333` |  | Latitude of the center search coordinate. If used, lng is required | yes |
| **`lng`** | `-72.545556` |  | Longitude of the center search coordinate. If used, lat is required | yes |
| **`distance`** | `2500` | `1000` | Radius of the center search coordinates in meters. Max distance is `5000` meters | yes |
| **`items`**  | `10` | `20` | Items per request (`0`-`33`) |  yes  |

Sample request:
* `[{'lat':'-13.163333', 'lng':'-72.545556', 'distance':'2000'}]`

#### Requests by Location
|  parameter  | sample | default | description | optional |
|----------|---------|---------|---------|---------|
| **`locationId`** | `24245` |  | The Instagram `locationId` of a location.<br>Find Instagram locations [here](http://worldc.am/) | no |
| **`items`**  | `10` | `20` | Items per request (`0`-`33`) |  yes  |

Sample requests:
* `[{'locationId':'24245', 'items':30}]`


### c) Rate limit
Visit the official [Instagram API documentation](https://www.instagram.com/developer/limits/)

**The live Rate Limit is 5000 / hour.** Global rate limits are applied inclusive of all API calls made by an app per access token over the 1-hour sliding window, regardless of the particular endpoint. Rate limits also apply to invalid or malformed requests.


# Licence
MIT

