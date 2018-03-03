# Magic Mirror Module: Api


This module for the [Magic Mirror²](https://github.com/MichMich/MagicMirror) provides a somewhat RESTful(ish) API to control the behaviour of the mirror.

The code is based on the great [MMM-Remote-Control module](https://github.com/jopyth/MMM-Remote-Control/issues).



### Routes

#### Modules

To send a notification to a module, just send a `POST` request to

```bash
MAGIG_MIRROR_IP:PORT/api/v1/modules/MODULE_NAME/ACTION?payload1key=payload1value&payload2key=payload2value
```

Example:
```bash
curl -X POST 'http://localhost:3001/api/v1/modules/mmm-glance/glance_on?name=news&time=1000'
```

#### Global
To send a global notification, use ```bash
MAGIG_MIRROR_IP:PORT/api/v1/notify/ACTION?payload1key=payload1value&payload2key=payload2value
```.

Example:
```bash
curl -X POST 'http://localhost:3001/api/v1/notify/glance_on?name=news&time=1000'
```

#### Validation

The advantage of specifying a module is having some basic validation which helps with debugging and error handling.

Example:
```bash
curl -X POST 'http://localhost:3001/api/v1/modules/imaginary_module/do_stuff'
```
will return a 404 response with an error message:
```json
{"success":"false","error":"Module not found"}
```
or
```json
{"success":"false","error":"Action not found"}
```

Successful calls return
```json
{"success":"true"}
```

#### Payload
The payload is passed as URL parameters. These are neither validated nor do they trigger an error message for invalid or missing keys or values.

### Overview
#### api/v1

| Routes | request | description |
| ------------- | -------- | ------------- |
| modules | GET | get a list of all installed modules and actions:<br> ```{"success": "true", "modules": {"alert": ["SHOW_ALERT","HIDE_ALERT"]}``` |
| modules/moduleName | GET | get a list of available actions for a module:<br> ```{"success":"true","actions":["SHOW_ALERT","HIDE_ALERT"]}``` |
| modules/moduleName/action | POST | send a notification to a module  |
| nofify/action | POST | send a global notification  |


## Installation

### Quick install

If you followed the default installation instructions for the [Magic Mirror²](https://github.com/MichMich/MagicMirror) project, you should be able to use the automatic installer.
The following command will download the installer and execute it:
```bash
bash -c "$(curl -s https://raw.githubusercontent.com/juzim/MMM-Api/master/installer.sh)"
```

### Manual install

1. Clone this repository in your `modules` folder, and install dependencies:
```bash
cd ~/MagicMirror/modules # adapt directory if you are using a different one
git clone https://github.com/juzim/MMM-Api.git
cd MMM-Remote-Control
npm install
```

2. Add the module to your `config/config.js` file, if you add a `position`, it will display the URL to the remote on the mirror.
```js
{
    module: 'MMM-Api'
},
```

3. Add the IP addresses of devices you want to use to access the Remote Control to the `ipWhiteList` in your `config.js`.

4. Restart your Magic Mirror² (i.e. `pm2 restart mm`).

### Update

Update this module by navigating into its folder on the command line and using `git pull`:

```bash
cd ~/MagicMirror/modules/MMM-Api # adapt directory if you are using a different one
git pull
npm install # install (new) dependencies
```

Alternatively you can run the `installer.sh` script again:
```bash
~/MagicMirror/modules/MMM-Api/installer.sh
```


## Known limitations

* the list of actions is generated by pulling some assumed action names from the files and should not be relied on
* in addition the validation of modules and actions is purely cosmetic to help with debugging and error handling. Since every notification can be send with the notify command, it should not be seen as a security layer
