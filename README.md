# wordpress-webex
![CI](https://github.com//warpdev-bywarpcom/wordpress-webex/workflows/CI/badge.svg)
[![Codacy Badge](https://app.codacy.com/project/badge/Grade/1c42f5e2cdce4d28a4bda7b524e0b6b5)](https://www.codacy.com/gh/warpdev-bywarpcom/wordpress-webex/dashboard?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=warpdev-bywarpcom/wordpress-webex&amp;utm_campaign=Badge_Grade)
[![Codacy Badge](https://app.codacy.com/project/badge/Coverage/1c42f5e2cdce4d28a4bda7b524e0b6b5)](https://www.codacy.com/gh/warpdev-bywarpcom/wordpress-webex/dashboard?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=warpdev-bywarpcom/wordpress-webex&amp;utm_campaign=Badge_Coverage)

An open source [Cisco Webex](https://www.webex.com/) (teams) plugin for WordPress.

ToDo

## Contribute

### Setting the dev environment

Note: Instructions for Ubuntu 20.04


#### Install Nginx, PHP and WordPress

We follow [Digital Ocean guide](https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-lemp-on-ubuntu-20-04) to install the wordpress. It is installed on ``/var/www/wordpress/``.

#### Install GIT

Replace ``https://github.com/warpdev-bywarpcom/wordpress-webex.git`` with your fork URL.

    cd /var/www/wordpress/wp-content/plugins/
    git clone https://github.com/warpdev-bywarpcom/wordpress-webex.git
    cd wordpress-webex

Verify git is ok:

    git status

You should get:

    On branch main
    Your branch is up to date with 'origin/main'.

    nothing to commit, working tree clean

#### Install PHPunit and PHP_CodeSniffer

To run tests you need the following:

    sudo apt-get install subversion

Add a folder for PHPunit

    mkdir ~/bin

Add this folder to the path (you can add this line to the end of the ``~/.bashrc`` file so this is added every time you login)

    export PATH="$HOME/bin:$PATH"
    export PATH="$HOME/.config/composer/vendor/bin:$PATH"

[Add PHPUnit version 7](https://phpunit.de/getting-started/phpunit-7.html) ([requirement for wordpress](https://make.wordpress.org/core/handbook/references/phpunit-compatibility-and-wordpress-versions/))

    cd ~/bin
    wget -O phpunit https://phar.phpunit.de/phpunit-7.phar
    chmod +x phpunit

Verify the install
    phpunit --version

Should print:

    PHPUnit 7.5.20 by Sebastian Bergmann and contributors.

[Install composer](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-composer-on-ubuntu-20-04-pt):

    curl -sS https://getcomposer.org/installer -o composer-setup.php
    sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer

Add packages used to lint:

    composer global require wp-coding-standards/wpcs
    composer global require phpcompatibility/phpcompatibility-wp
    composer global require dealerdirect/phpcodesniffer-composer-installer

#### Run tests

Replace ``root`` with a mysql user with permission to create DBs, and ``password`` with the password, no pun intended.

    sudo bin/install-wp-tests.sh wordpress-test root password 127.0.0.1 latest
    phpunit

Run lint tests (use ``phpcs`` if you don't want to fix automatically):

    phpcbf --standard=WordPress .


#### Remove test

Replace ``root`` with the user name, and ``password`` with the user password.

    rm -rf /tmp/wordpress
    sudo rm -rf /tmp/wordpress-tests-lib/
    sudo rm -rf /tmp/wp-latest.json
    mysqladmin drop wordpress-test --user="root" --password="password"