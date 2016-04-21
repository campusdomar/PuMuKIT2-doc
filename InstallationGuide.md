# PuMuKIT-2 Installation Guide

*This page is updated to the PuMuKIT 2.2.0 version*

## Index

1. [Requirements](#1-requirements)
2. [Single-node Installation on Linux Ubuntu 14.04](#2-single-node-installation-on-linux-ubuntu-1404)
3. [Multi-node Installation](#3-multi-node-installation)
4. [F.A.Q.](#4-faq)

## 1. Requirements

PuMuKIT-2 is a LEMP (Linux, nginx, MongoDB, PHP) application, created with the Symfony2 framework. It uses libav-tools (or ffmpeg) to analyze the audiovisual data, as well as to transcode the data.

We recommend to install PuMuKIT on **Ubuntu 14.04** Linux. PuMuKIT 2 has been developed and tested on that Linux distribution and version.

Although it is recommended to use Ubuntu 14.04 it's not mandatory. You can try other Linux distributions and versions at you own risk. The raw requirements for PuMuKIT installation at OS level are: Linux, nginx (1.4.6), libav-tools (9.18), php5 (5.5.9) and mongo (3.0). Libav-tools with h264 and aac support is needed. Also the following php5 modules are required: php5-json, php5-cli, php5-mongo, php5-ldap, php5-curl, php5-intl, php5-fpm and php5-xdebug. Make sure text search is enabled for your mongodb (version 3.0+).

Use [composer](https://getcomposer.org/) to check and install the dependencies

PuMuKIT-2 has been developed and is often installed on Linux Ubuntu but its use is not essential. It is known it works on Ubuntu 14.04. If it is installed on other Linux distributions, additional libraries may be required.

## 2. Single-node Installation on Linux Ubuntu 14.04

In this section we will describe how to install PuMuKIT in a single server (node) running Ubuntu 14.04. A single-node deployment of PuMuKIT is usually recommended for development, demonstration and small production environments.

Follow the below instructions. If any error is thrown check the [F.A.Q. section](#4-faq) or ask on PuMuKIT community list:

1. Update APT source list to install last version of MongoDB.

    ```
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
    echo "deb http://repo.mongodb.org/apt/ubuntu "$(lsb_release -sc)"/mongodb-org/3.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.0.list
    sudo apt-get update
    ```

2. Install dependencies of PuMuKIT-2 (see requirements):

    ```
    sudo apt-get install -y git curl nginx-full
    sudo apt-get install -y php5-fpm php5-cli php5-curl php5-intl php5-json
    sudo apt-get install -y php5-intl php5-xdebug php5-curl php5-ldap
    sudo apt-get install -y mongodb-org php5-mongo
    sudo apt-get install -y libav-tools libavcodec-extra
    ```

3. Create web directory and give the right permissions:

    ```
    sudo mkdir -p /var/www
    sudo adduser `whoami` www-data
    sudo chown `whoami`:www-data -R /var/www
    sudo chmod 0755 -R /var/www
    sudo chmod g+s -R /var/www
    ```

4. Download the last version of PuMuKIT-2:

    ```
    git clone https://github.com/campusdomar/PuMuKIT2.git /var/www/pumukit2
    ```

5. Activate the 2.2.0 tag:

    ```
    cd /var/www/pumukit2
    git checkout 2.2.0
    ```

6. Install [composer](https://getcomposer.org/).

    ```
    curl -sS https://getcomposer.org/installer | php
    ```

7. Install dependencies and provide PuMuKIT-2 parameters:

    ```
    php composer.phar install
    ```
    In case it gives a token error, check the [solution in the F.A.Q](#add-github-token)

8. Give cache and log directories the right permissions.

   * Follow the instructions at Symfony [documentation](http://symfony.com/doc/2.6/book/installation.html#checking-symfony-application-configuration-and-setup).

9. Set the `"date.timezone"` setting in all the `php.ini` files with your timezone (e.g. `Europe/Madrid`):

    ```
    sudo sed -i "s/;date.timezone =/date.timezone = Europe\/Madrid/g" /etc/php5/cli/php.ini
    sudo sed -i "s/;date.timezone =/date.timezone = Europe\/Madrid/g" /etc/php5/fpm/php.ini
    ```

10. Override the 'upload_max_filesize' and 'post_max_size' settings in all the php.ini files to upload bigger files

    ```
    sudo sed -i "/post_max_size =/c\post_max_size = 2000M" /etc/php5/cli/php.ini
    sudo sed -i "/upload_max_filesize =/c\upload_max_filesize = 2000M" /etc/php5/cli/php.ini
    sudo sed -i "/post_max_size =/c\post_max_size = 2000M" /etc/php5/fpm/php.ini
    sudo sed -i "/upload_max_filesize =/c\upload_max_filesize = 2000M" /etc/php5/fpm/php.ini
    ```
    *NOTE: We recommend a default value of 2G (or 2000M). If you are planning to upload bigger files, change the '2000M' values above to higher ones.*

11. Set "xdebug.max_nesting_level" to "1000" in PHP configuration to stop Xdebug's infinite recursion protection erroneously throwing a fatal error:

    ```
    echo "xdebug.max_nesting_level=1000" | sudo tee -a /etc/php5/fpm/conf.d/20-xdebug.ini
    ```

12. Add NGINX config file and restart php5-fpm and nginx

    ```
    sudo cp doc/conf_files/nginx/default /etc/nginx/sites-available/default
    sudo service php5-fpm restart
    sudo service nginx restart
    ```

13. Check environment requirements:

    * Go to `http://{PuMuKIT-2-HOST}/config.php` for checking requirements.
    * Fix errors if any and restart PHP5-FPM service. Fix warnings if necessary (PDO drivers are not necessary for PuMuKIT-2 to work).
      ```
      sudo service php5-fpm restart
      ```
    * Check requirements again
    * Repeat all steps until the MAJOR PROBLEMS list disappears.

14. Prepare environment (init mongo db, clear cache)

    ```
    php app/console doctrine:mongodb:schema:create
    php app/console cache:clear
    php app/console cache:clear --env=prod
    ```

15. Create the admin user

    ```
    php app/console fos:user:create admin --super-admin
    ```

16. Load default values (tags, broadcasts and roles).

    ```
    php app/console pumukit:init:repo all --force
    ```

17. [Optional] Load example data (series and multimedia objects)

    ```
    php app/console pumukit:init:example  --force
    ```

18. Restart server

    ```
    sudo service php5-fpm restart
    sudo service nginx restart
    ```

19. Connect and enjoy

    * Connect to the frontend here: `http://{PuMuKIT-2-HOST}/`
    * Connect to the back-office (Admin UI) with the user created on step 13 here: `http://{PuMuKIT-2-HOST}/admin`

[(back to index)](#index)

## 3. Multi-node Installation

In this section we will describe how to install PuMuKIT in a multi server (multi-node) environment. A multi-node deployment of PuMuKIT is usually recommended for real production environments.

On a default single-node installation, the transcoder, the storage of the multimedia files and the database are
installed in the same node as PuMuKIT. A different installation can be done. It is possible to set up external nodes
to install a transcoder or manage the storage of the multimedia files in a NAS (Network Attached Storage) server.

To perform a multi-node installation of PuMuKIT, first you need to install PuMuKIT in a node according to
[Single-node Installation on Linux Ubuntu 14.04](#2-single-node-installation-on-linux-ubuntu-1404).

After having a single-node deployment you can improve it with additional nodes to take care of storage and/or trancoding jobs:


#### 4.1. External PuMuKIT-Transcoders

On a default standalone installation, there is only one PuMuKIT transcoder installed in the same node as PuMuKIT core.

One or multiple transcoders can be installed in different nodes. An external transcoder node can be configured with the
following recommendations:

- Ubuntu 14.04 although another operating system that accomplish the following recommendations can be used.
- 8 CPUs
- 8 GB RAM
- 20 GB HDD
- libav / ffmpeg libraries: the ones you have configured into the `app/config/encoder.yml` file of your PuMuKIT node.
- Being in the same network as the PuMuKIT and NAS nodes.

Configure the transcoder node as a web server with nginx or Apache.

Copy the [webserver.php file](https://github.com/campusdomar/PuMuKIT2/blob/2.1.x/doc/conf_files/cpus/webserver.php) of the PuMuKIT node into the web folder of your transcoder node server directory.
For instance, in a default nginx server configuration on an Ubuntu 14.04 server, should be in `/usr/share/nginx/html/webserver.php`. Change the default user and password of this webserver configuration
by opening the file and changing these two lines:

```
define("USER", "pumukit");
define("PASSWORD", "PUMUKIT");
```

Give access to the storage system (On PuMuKIT server or External NAS).

Configure the `app/config/encoder.yml` file of your PuMuKIT node to add the transcoder server in the `cpus` section.
To see all the options to configure, type on your PuMuKIT node and see the `cpus` section:

```
cd /path/to/pumukit2
php app/console config:dump-reference pumukit_encoder
```

```
    cpus:

        # Prototype
        name:

            # Encoder Hostnames (or IPs)
            host:                 ~ # Required

            # Deprecated since version 2, to be removed in 2.2.
            number:               1

            # Top for the maximum number of concurrent encoding jobs
            max:                  1

            # Type of the encoder host (linux, windows or gstreamer)
            type:                 ~ # One of "linux"; "windows"; "gstreamer"

            # Specifies the user to log in as on the remote encoder host
            user:                 ~

            # Specifies the user to log in as on the remote encoder host
            password:             ~

            # Encoder host description
            description:          ''
```

* `name`: name of your transcoder server. E.g.: transcoder. Replace the word `name` with the name you give.
* `host`: IP or hostname of your transcoder server
* `max`: maximum number of concurrent jobs
* `type`: type of the transcoder host
* `user`: username defined in the webserver.php file of your transcoder server
* `password`: password defined in the webserver.php file of your transcoder server

You can add as many transcoders as you want to the configuration of the encoders of PuMuKIT.
Define each transcoder with a different `name` and add host, max, type, user and password configuration.


#### 4.2. External Storage or NAS (Network Attached Storage) Server

Configure your NAS server to give access to PuMuKIT and transcoder nodes according to:

- PuMuKIT uploads, inbox and tmp directories. Defaults:
    - pumukit2.uploads_dir: '%kernel.root_dir%/../web/uploads'
    - pumukit2.inbox: '%kernel.root_dir%/../web/storage/inbox'
    - pumukit2.tmp: '%kernel.root_dir%/../web/storage/tmp'

- Encoders configuration: `dir_out` parameter of each profile defined in your `app/config/encoder.yml` configuration file.

PuMuKIT node and transcoder node should mount the NAS server folder where all the multimedia files are shared.


[(back to index)](#index)

## 4. F.A.Q.

**Configure max upload filesize**

1.- If you get a 413 response status code (request entity too large) and get "client intended to send too large body" in NGINX log. Check the NGINX conf file (/etc/nginx/sites-available/default)

```
client_max_body_size 2000m;
client_body_buffer_size 128k;
```

2.- If you get a message like "The file XXXX.avi exceeds your upload_max_filesize ini directive". Check the php-fpm conf file (/etc/php5/fpm/php.ini)

```
upload_max_filesize = 2000M
post_max_size = 2000M
```

3.- If you get a 504 "Gateway Time-out", change the php5-fpm configuration. Chose the same value for `request_terminate_timeout` in `/etc/php5/fpm/pool.d/www.conf` and `max_execution_time` in `/etc/php5/fmp/php.ini`files. For example:

* /etc/php5/fpm/pool.d/www.conf

```
request_terminate_timeout = 30
```

* /etc/php5/fpm/php.ini

```
max_execution_time = 30
```

* Restart php5-fpm and NGINX:

```
sudo service php5-fpm restart
sudo service ningx restart
```


**Setting up Permissions?**

 * http://symfony.com/doc/2.6/book/installation.html#checking-symfony-application-configuration-and-setup
 * http://symfony.es/documentacion/como-solucionar-el-problema-de-los-permisos-de-symfony2/
 * Setting up ownership of upload directories

    ```
    sudo chown -R www-data:www-data web/storage/ web/uploads/
    ```


**Enable MongoDB text index**

Use MongoDB 2.6 or upper, the text search feature is enabled by default. In Ubuntu 14.04 you can install the last version of MongoDB using the next documentation:

 * http://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/


#####Add GitHub token

In case the `php composer.phar install` asks for a token with a  message like:

```
Could not fetch https://api.github.com/repos/VENDOR_NAME/NAME_Bundle/zipball/HASH_CODE, please create a GitHub OAuth token to go over the API rate limit
Head to https://github.com/settings/tokens/new?scopes=repo&description=Composer+on+HOST_ID+DATE+TIME
to retrieve a token. It will be stored in "/home/USER/.composer/auth.json" for future use by Composer.
Token (hidden):
```

There is a rate limit on GitHub's API for downloading repos. In case of reaching that limit, Composer prompts the above message asking for authentication to download a repo.

It is necessary to create a token and add it to composer.

1.- Create an OAuth token on GitHub. Follow the instructions on [GitHub Help page](https://help.github.com/articles/creating-an-access-token-for-command-line-use/).

2.- Add the token to the configuration running:

```
cd /var/www/pumukit2
php composer.phar config -g github-oauth.github.com <oauthtoken>
```

Now Composer should install/update without asking for authentication.


**Not allowed to access app_dev.php via web**

If you get this message when trying to access http://{PuMuKIT-2-HOST}/app_dev.php:
```
You are not allowed to access this file. Check app_dev.php for more information.
```

Comment the following code in `web/app_dev.php` file:

```php
/*
// This check prevents access to debug front controllers that are deployed by accident to production servers.
// Feel free to remove this, extend it, or make something more sophisticated.
if (isset($_SERVER['HTTP_CLIENT_IP']) || isset($_SERVER['HTTP_X_FORWARDED_FOR']) || !(in_array(@$_SERVER['REMOTE_ADDR'], array('127.0.0.1', 'fe80::1', '::1')) || php_sapi_name() === 'cli-server')
) {
header('HTTP/1.0 403 Forbidden');
exit('You are not allowed to access this file. Check '.basename(FILE).' for more information.');
}
*/
```


**Not allowed to access config.php via web**

If you get this message when trying to access http://{PuMuKIT-2-HOST}/config.php:
```
This script is only accessible from localhost.
```

Comment the following code in `web/config.php` file:

```php
/*
if (!in_array(@$_SERVER['REMOTE_ADDR'], array(
    '127.0.0.1',
    '::1',
))) {
    header('HTTP/1.0 403 Forbidden');
    exit('This script is only accessible from localhost.');
}
*/
```


**Internal Server Error (500) when search a series or a multimedia object**

The mongodb text index is needed for searching. And this error is produced because the text index is not created:

```
Cannot run command count(): error processing query: ns=pumukit___.MultimediaObject limit=0 skip=0
...
planner returned error: need exactly one text index for $text query
```

To generate the index execute the command:

```
php app/console doctrine:mongodb:schema:create --index
```

[(back to index)](#index)
