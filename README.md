[![pipeline status](https://gitlab.adversign-media.de/viewneo/viewneo-cloud-v3/badges/master/pipeline.svg)](https://gitlab.adversign-media.de/viewneo/viewneo-cloud-v3/commits/master)
# Installation


### Running viewneo cloud with homestead (easy setup)
I'm happy to announce that the viewneo cloud can now run with homestead and vagrant.
To run a local viewneo instance follow these steps:

1. [Download](https://www.vagrantup.com/downloads.html) and install vagrant
2. Clone the repository: `git clone git@gitlab.adversign-media.de:viewneo/viewneo-cloud-v3.git viewneo-cloud-v3-vagrant`
3. Change to the project's directory: `cd viewneo-cloud-v3-vagrant`
4. For the first time run `sh dev-setup.sh`
5. Choose the number of CPUs and memory to use.
6. Wait. (The first time can take a while).

After this installation your machine is already running.
Next time you can start your local machine with `composer viewneo:up` and stop it with `composer viewneo:down`.
You can play around and test everything.[Bin@so3005slut] If you want you can run `composer viewneo:rebuild` to rebuild the vm (This will delete and recreate the vm).
If you want to ssh to your machine, just `cd` to your project folder and run `vagrant ssh`.

For some vagrant commands you can use this nice [cheat-sheet](https://gist.github.com/wpscholar/a49594e2e2b918f4d0c4).

If you want to know more click on these flowing links: [vagrant](https://www.vagrantup.com/intro/index.html) & [homestead](https://laravel.com/docs/5.7/homestead)

#### Running viewneo cloud without homestead (old setup)
Please follow these instructions, in the given order on the command line, to install viewneo on your local machine.

1. Clone the repository: `git clone git@gitlab.adversign-media.de:viewneo/viewneo-cloud-v3.git`
2. Change to the project's directory: `cd viewneo-cloud-v3`
3. Do: `git pull`
4. Then: `git checkout develop`
5. Generate a `.env` file by doing: `cp .env.example .env` and adjusting the parameters to suit your environment.
6. Login to your virtual machine and cd into  `/srv/www/local.viewneo.com`.
7. Run `composer install`.
8. Back on your local machine add the following line to /etc/hosts `[IP_OF_YOUR_VM] local.viewneo.com`.
9. Now pop up your browser and enter: `http://local.viewneo.com` to log on to viewneo.

The following users will be added to the database after installation:
- superadmin@adversign-media.de
- admin@adversign-media.de
- superadmin@adversign-media.de
- superadmin@adversign-media.de

The password for all users is: vn123456

# Automatic deployment to cloud and cloud-rc
The cloud will automatically be deployed whenever the master-branch is updated.
Normally there should be no need to deploy the cloud manually over ssh.
However it is still possible to SSH into the production server and run scripts after
deployment.

# Manual deploying to cloud-dev.viewneo.com
NOTE:
_ Never use commands: "git push" or even no need to use "git commit".
_ cloud-dev uses its own database (DB_DATABASE=vn_v3_dev)
_ source codes of cloud-dev is pulled from the 'develop' branch.

1. sudo -u www-data -s
2. cd /srv/www/viewneo.com/cloud-dev
3. git status   (Check if everything is OK or not? e.g any conflicts?)
4. git pull
5. ls -la       (Check if everything is OK? e.g. symlinks, ".env -> .env.cloud_dev", "storage -> /srv/data/cloud-dev/storage/")
6. `composer install`
7. sync_viewneo.sh cloud-dev
"**it is deployed to cloud-dev.viewneo.com"


# Manual deploying cloud.viewneo.com & cloud-rc.viewneo.com
NOTE:
_ Never use commands: "git push" or even no need to use "git commit".
_ cloud & cloud-rc share the same database (DB_DATABASE=vn_v2_pro)
_ source codes of cloud and cloud-rc is pulled from the 'master' branch.

[switching user]
1. sudo -u www-data -s

[cloud-rc]

    2. cd /srv/www/viewneo.com/cloud-rc
    3. git status   (Check if everything is OK or not? e.g any conflicts?)
    4. git pull
    5. ls -la       (Check if everything is OK? e.g. symlinks, ".env -> .env.cloud_rc", "storage -> /srv/data/cloud-rc/storage/")

[cloud]

    6. cd /srv/www/viewneo.com/cloud
    7. git status   (Check if everything is OK or not? e.g any conflicts?)
    8. git pull
    9. ls -la       (Check if everything is OK? e.g. symlinks, ".env -> .env.production", "storage -> /srv/data/storage/")
    10. `composer install`

[deploying]

    11. sync_viewneo.sh (This will deploy to cloud and cloud-rc)

"**it is deployed to cloud.viewneo.com & cloud-rc.viewneo.com"

[NOTE!!! Never use commands: "git push" or even no need to use "git commit"]

# Credentials for cloud-rc and cloud-dev
username: viewneo
password: XsqC9YCPMS

# Working on the frontend

## Prequisites

Install nodeJS and NPM on your virtual machine:
```shell
curl -sL https://deb.nodesource.com/setup_6.x | /bin/bash
apt-get install -y nodejs
```

### Building the frontend
1. Cd to your repository-folder.
2. Run `npm install`
3. Run `npm run components:update`
4. Run `npm run build:prod`

This will install the Javascript-dependencies and runs Webpack to build
the app.js bundle and the templates.js bundle.


### Source files

All source files for the frontend can be found in `resources/assets/js`.
In general all angularJS components are sorted by recipient-type like
controllers or directives.

Subfolders can be defined when there are too many
files in one folder like in `resources/assets/js`. In this case the components should
be sorted by feature eg.: `resources/assets/js/directives/devices/` stores all
directives related to deivce management.
mongodb://docdb_admin:a7AE6SNpagzwdcNR/myroslav3005bilyk@prod-microservices-docdb-cluster.cluster-ctjm48jlqv7a.eu-central-1.docdb.amazonaws.com/?authMechanism=DEFAULT&retryWrites=f



```
viewneo-cloud
│   
...
└───resources
│   └--assets
|   |  └---js             // This is the source folder. (Your code goes here.)
|   |      └---config     // All config recipients like $routeProvider
|   |      └---constants  // Constants are stored here.
|   |      └---controllers// All controllers should be here.
|   |      └---directives // Directives sorted by usecase (device, playlist...)
|   |      └---filters    // Angular filters are stored here.
|   |      └---resources  // Resources can be find in this folder
|   |      └---services   // All factory, service and provider recipients
|   |      └---app.js     // Main entry point for the application
│   └--templates          // template files for directives, partials etc...
|
└───public
│   └--js
|   |  └---languages  // Translation files for the frontend in json-format
|   |  └---libs       // Third party javascript libraries
│   └--css            // CSS-files for frontend
```
# VUE mounted point
| id               | file name             |
| -----------------|:----------------------|
| media-page       | mefia-file-list.html  |
| diwa-playlist    | diwa-playlist.html    |
| playlists        | playlistі.html        |
| checkin-settings | checkin-settings.html |
| diwa-groups      | diwa-groups.html      |
| vn-sidebar       | main.blade.php        |

