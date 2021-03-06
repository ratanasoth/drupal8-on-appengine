# Deploying Drupal 8 on Google App Engine Flex and Cloud SQL

Deploying Drupal 8 on Google App Engine Flex and Cloud SQL using composer package manager and homebrew on Mac OS X.

Introduction and summary information should be included here. More description about this project should be inserted here. THIS README IS NOT COMPLETE.

## Assumptions

```sh
This project assumes you are using a Mac OS X system with root/admin privileges.
```

### Note

**Uploading files:**
> Google App Engine does NOT allow writes to the local filesystem. You will need to configure Cloud Storage or other storage option to be able to upload files on your app.

**Using Cloud SQL:**
> Use your Cloud SQL instance settings during the installation. That way, when you deploy your app it is already up and running!

**Extending Drupal:**
> It is highly recommended that you use composer to extend Drupal (with modules, themes, libraries etc.) Follow the syntax patterns below to install, uninstall, or update extensions for your new d8 site:

```sh
composer require drupal/<module-name>
composer remove drupal/<module-name>

composer require drupal/bootstrap
composer remove drupal/bootstrap

composer update -v
```

---

## Quick start

The fastest way to get started is to clone this repository and use composer to build your app. Let's clone!

```sh
git clone git@github.com:webdora/drupal8-on-appengine.git

```

Next you will initialize and install the application using composer. Run the steps below to create your project files and make sure everything is synced and linked.

```sh
composer create-project
composer install -v
composer update -v
```

If everything is configured properly, you should see the Drupal 8 installation page on your local web server. Follow on screen instructions to finish installing Drupal:

> [http://localhost/](http://localhost/ "localhost")

With Drupal 8 up and running on your localdev/web server, you are finally ready to deploy the app. Check the `app.yaml` file to see the Google App Engine settings. Then use the `gcloud app deploy` command to deploy your app. Adding `--verbosity=info` allows you to see the deployment process details.

```sh
gcloud app deploy --verbosity=info
```

After deployment, browse your shiny new Drupal App Engine app at:

> [https://your-project-name.appspot.com/](https://your-project-name.appspot.com/ "your-project-name.appspot.com")

## Prerequisites

You should have a web server (probably apache) running php 7.2 that has been configured to use this project's public folder as the document root. I used `/<my-file-path>/drupal8-on-appengine/web` for this project. If you do not already have necessary components installed use the steps below or follow the detailed instructions on [Andy Miller's blog](https://getgrav.org/blog/macos-sierra-apache-multiple-php-versions "Andy Miller's blog post").

You should also have a database server running MySQL or MariaDB on a network your web server can access. If you are using Google Cloud SQL for your database, make sure you have configured your service accounts with proper permissions, roles, and keys.

You can temporarily add your IP to the `Authorized networks` by going to `Cloud SQL > Instance Details > Edit` on [Google Cloud Console](https://console.cloud.google.com/ "Google Cloud Console"). You can find the official Google documentation [here](https://cloud.google.com/sql/docs/mysql/connect-external-app/ "Google Cloud SQL documentation").

### Install XCode CLI tools

```sh
xcode-select --install
```

### Install Homebrew

```sh
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

To make sure brew was installed use `brew --version`. To be safe, let's run `brew doctor`.

### Install Apache

```sh
brew install httpd
```

#### To automatically load apache on boot use

```sh
sudo brew services start httpd
```

#### Use the following commands as needed

```sh
sudo apachectl start
sudo apachectl stop
sudo apachectl -k restart
```

### Install PHP

```sh
brew install php@7.2
```

### Configure Apache for PHP

Edit the apache config file at `/usr/local/etc/httpd/httpd.conf` with your settings. See the guide linked on the top of this section for detailed setup guide. You also need to enable some modules. Uncomment the line `LoadModule rewrite_module lib/httpd/modules/mod_rewrite.so` to enable `mod_rewrite`.

### Install Google Cloud SDK

Make sure you have python 2.7 installed. Use `python -V` to check.

We will use homebrew to install GCP SDK:

```sh
brew cask install google-cloud-sdk
```

### Brew update and clean up

```sh
brew update
brew upgrade
```

THIS SECTION IS NOT COMPLETE.

## How I did it

Below I will outline how I created this project. I will cite sources and document my process for anyone that might be interested.

THIS SECTION IS NOT COMPLETE.

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment notes on how to deploy the project on a live system.

Go to [https://github.com/drupal-composer/drupal-project](https://github.com/drupal-composer/drupal-project "Drupal composer on github") and download the default `composer.json` file. In a separate location download this project's `composer.json` [file](https://github.com/webdora/drupal8-on-appengine/blob/master/composer.json).

We will study this file to create our app and deploy it to Google App Engine. In your project's root folder place the `composer.json`. We will be initializing our project here.

```sh
git init
```

Edit the default `composer.json` file to make it suitable for our use. We are not going to need multiple sites or local `.env` file configuration for this tutorial. So those parts should be removed.

Notice the changes made on the second `composer.json`. We added `php7.2.x` and `ext-gd`. The `gd` module is still finicky on the app engine, so instead of using a set version, we will allow `gcloud` to figure it out by using wildcard `“*”` for the version.

**Then you do:**

```sh
composer create-project
```

**Then do:**

```sh
composer install -v
```

**Then:**

```sh
composer update -v
```

Now you should have a Drupal install showing up on [http://localhost/](http://localhost/ "localhost") (depending on your configuration).

**Deploy your app to Google App Engine:**

```sh
gcloud app deploy --verbosity=info
```

## Deployment notes

```sh
This project is NOT deployment ready. Currently under active development.
```

## Credits

I used `drupal-composer/drupal-project` as basis. Relevant files were modified to enable composer install and deployment to Google App Engine Flex.

**Some resources I used:**

* [https://github.com/prameya/drupal-project](https://github.com/prameya/drupal-project "https://github.com/prameya/drupal-project")
* [http://blog.boombatower.com/drupal-google-app-engine](http://blog.boombatower.com/drupal-google-app-engine "http://blog.boombatower.com/drupal-google-app-engine")
* [https://cloud.google.com/sql/docs/mysql/connect-external-app/](https://cloud.google.com/sql/docs/mysql/connect-external-app/ "https://cloud.google.com/sql/docs/mysql/connect-external-app/")
* [https://getgrav.org/blog/macos-sierra-apache-multiple-php-versions](https://getgrav.org/blog/macos-sierra-apache-multiple-php-versions "https://getgrav.org/blog/macos-sierra-apache-multiple-php-versions")
* [https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet "https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet")
* [https://cloud.google.com/appengine/docs/flexible/php/configuring-your-app-with-app-yaml](https://cloud.google.com/appengine/docs/flexible/php/configuring-your-app-with-app-yaml "https://cloud.google.com/appengine/docs/flexible/php/configuring-your-app-with-app-yaml")
* [https://github.com/GoogleCloudPlatform/php-docs-samples/tree/master/appengine/flexible/drupal8](https://github.com/GoogleCloudPlatform/php-docs-samples/tree/master/appengine/flexible/drupal8 "https://github.com/GoogleCloudPlatform/php-docs-samples/tree/master/appengine/flexible/drupal8")
* [https://www.rainbowbreeze.it/how-to-setup-a-google-app-engine-python-environment-on-mac-osx-using-homebrew/](https://www.rainbowbreeze.it/how-to-setup-a-google-app-engine-python-environment-on-mac-osx-using-homebrew/ "https://www.rainbowbreeze.it/how-to-setup-a-google-app-engine-python-environment-on-mac-osx-using-homebrew/")