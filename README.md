# Electron Release Server (customized for Merck)

## Installation
### Database (MySQL)
1. Create databases
``` sql
CREATE DATABASE electron_release_server ;
```
2. Create 'electron_release_server_user' user
3. Grant 'OWNER' level permissions for the user for the database above (all permission except GRANT OPTION)

    **A database schema will be created automatically once the application starts.*

### Application server
1. Install GIT
``` bash
sudo apt-get install git
```
2. Create a directory for an application
``` bash
mkdir electron-release-server
```
3. Clone a repository
``` bash
GIT_SSL_NO_VERIFY=1
git clone https://<user_name>@git.epam.com/mrc-assy/electron-release-server.git
```
4. Install latest version of NodeJS
``` bash
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install nodejs -y

nodejs -v # should be 8.3.0 or higher
npm -v #should be 5.3.0 or higher
```
5. Install Required NPM modules
``` bash
npm install
```
6. Define environment variables
``` bash
NODE_ENV=production
```
7. Update configuration files
``` js
// config/connection.js

mysqlServer: {
    adapter: 'sails-mysql',
    host: 'localhost',
    user: 'electron_release_server_user',
    password: '<password>',
    database: 'electron_release_server'
},
```
``` js
// config/files.js

dirname: '~/electron-release-server/assets'
```
``` js
// config/env/production.js

module.exports = {
  models: {
    connection: 'mysqlServer',
    migrate: 'safe'
  },

  port: 80,

  log: {
    level: "silent"
  },

  appUrl: 'http://<your host name>',

  auth: {
    static: {
      username: 'admin',
      password: '<your password>'
    },

    secret: '2xSqEsM4dtYQcNd5V6FBJHsqzMMOHkftiCviyzUlvtZ2kX88FB7kfhZTYJo2daj'
  },

  jwt: {
    token_secret: 'pCGoKop8bsyp9avlngPSPVw2hitfi0VGgVjjAygYLKTonXsYq0xa4uGKbSt6JFP'
  }
};
```
8. Start an application
```bash
sudo npm start --prod
```

## Documentation
Original documentation for the project could be found [here](https://github.com/ArekSredzki/electron-release-server/tree/master/docs).

## Maintenance
You should keep your fork up to date with the electron-release-server master.

Doing so is simple, rebase your repo using the commands below.
```bash
git remote add upstream https://github.com/ArekSredzki/electron-release-server.git
git fetch upstream
git rebase upstream/master
```

## License
[MIT License](LICENSE.md)
