# Variable VVV - The Best VVV Site Wizard

	 ██    ██ ██    ██
	░██   ░██░██   ░██     Variable VVV 1.9.3
	░░██ ░██ ░░██ ░██
	 ░░████   ░░████       The easiest way to set up
	  ░░██     ░░██        WordPress sites with VVV!
	   ░░       ░░


`vv` makes it extremely easy to create a new WordPress site using [Varying Vagrant Vagrants](https://github.com/Varying-Vagrant-Vagrants/VVV). `vv` supports site creation with many different options; site blueprints to set up all your plugins, themes, and more; deployments; and lots more features.

[![Travis](https://img.shields.io/travis/bradp/vv.svg)]()

*Tired of the time it takes to do a `vagrant provision` or create new sites?* Check out [flip](https://github.com/bradp/vvv-provision-flipper), a simple utility I built to solve that issue.

## Installation

### OS X Installation

If you have [Homebrew](http://brew.sh/) installed, you run the following in your terminal application:

	$ brew install bradp/vv/vv

Otherwise, clone this repositoy and edit your `$PATH` to include the `vv` core file:

1. Clone this repo: `git clone https://github.com/bradp/vv.git`
1. Add the `vv` core script to your shell's `$PATH`:
    * If you're using `bash`: ``touch ~/.bash_profile && echo "export PATH=\$PATH:`pwd`/vv" >> ~/.bash_profile``

### Windows Installation

* Clone `vv` to a folder somewhere.

    `$ git clone https://github.com/bradp/vv.git`

* Add that folder to your system path. See [here](http://windowsitpro.com/systems-management/how-can-i-add-new-folder-my-system-path) if you need help.

* Open an explorer window and go to My Computer (or This PC).
* Right click and choose properties
* Choose Advanced System Settings
* Choose Environmental Variables form the Advanced Tab
* Choose the "Path" variable and edit it.
* Add a semicolon to end the previous path item and then add the `vv` folder path (Example: `;C:\Users\Name\Documents\vv`)
* Open Git Bash and run `vv`

Alternately, you can use cmd.exe with `bash vv`.

Props to [Vinsanity](https://github.com/Vinsanity) for these instructions. If you're having issues, please see [this issue](https://github.com/bradp/vv/issues/33).

### Linux Installation

* Clone vv into a folder.

    `$ git clone https://github.com/bradp/vv.git`

* Access the directory that you cloned vv into.

* Copy the vv executable to /usr/local/bin

    `$ sudo cp vv /usr/local/bin`

* You should now be able to easily run vv from anywhere in your system.
 
## Adding tab-completion to `vv`

Currently, `vv` supports tab-completion of arguments and options in both bash and ZSH. To enable this, you'll first want to make sure you're on the most current version of `vv`. Then simply add `source $( echo $(which vv)-completions)` to the end of your .bash_profile, .bashrc or .zshrc.

## Updating

vv is currently under development, and you'll probably want the latest and greatest version at all times.

You can run `vv --update` to update to the latest version. This will update via Homebrew if you've installed it that way, otherwise vv will bootstrap an update on where ever you've installed it.

vv will automatically check for updates and update itself once a week. You can disable this by adding `"auto_update_disable": false` to the JSON config in `~/.vv-config`.

If you have trouble updating, you may want to try some of the options below:

Homebrew sometimes caches a version of Variable VV causing you to receive a message saying you are out of date with the Github version, however running `vv --update` simply downloads a version you already have installed. In cases like this, there are two safe options you can try.

First, and simplest, run `vv --force-update`. Second, if that does not work you can safely uninstall Variable VV and re-install it via homebrew, you can do this with these commands: `brew remove vv` then `brew untap bradp/vv` and finally, run the install command `brew install bradp/vv/vv`as mentioned above. You will not lose any settings or sites.

## Usage

Once installed, you can run `vv` anywhere you'd like. If vv can't automatically find your VVV installation, you will be prompted for the path. It will also save this into a configuration file in `~/.vv-config`, so you won't be prompted again.

At any time, you can run `vv` or `vv --help` to see a list of all possible options and flags.

vv will prompt you for a value for any required flags that were not specified.

The main commands are `list`, `create`, `delete`. These will list your sites, create a site, and delete a site. These each have a few aliases, so for example, if you run `vv show`, vv will know you meant `vv list`.

To start creating a site, simply do `vv create` ( you can also do `vv --create`, or simply `vv -c`). You will then be prompted for all required options.

All options and flags are [listed below](#options).

## Site Creation

`vv create`

Creating a site does the following:

* Halts Vagrant (if running)
* Creates a web root for the site in the `www` folder containing three files: `vvv-init.sh`, `wp-cli.yml`, and `vvv-hosts`
	* `vvv-init.sh` tells Vagrant to create a database if one does not exist and install the latest version of WordPress (via WP-CLI) the next time Vagrant is provisioned
	* `wp-cli.yml` tells WP-CLI that WordPress is in the htdocs folder
	* `vvv-hosts` contains the hosts entry to give your site a nice custom domain (the domain is set in the wizard)
* Creates a file in the `nginx-config` folder to handle server settings for your site
* Restarts Vagrant with `vagrant up --provision`

Provisioning Vagrant takes a couple of minutes, but this is a crucial step as it downloads WordPress into your site's htdocs directory and runs the installation. If you want to skip provisioning and install WordPress manually, you can run the new site's `vvv-init.sh` file directly in the Vagrant shell.

### Subdomain Multisite Installation

If you are using a subdomain multisite, you must edit vvv-hosts file inside of that site's folder with each subdomain on a new line. For example:
    mysite.dev
    siteA.mysite.dev
    siteB.mysite.dev

After this, run `vagrant reload --provision` and your subdomains should resolve. *Please note*, any sites set up prior to version 1.7.3 will need more configuration for this, either delete and re-set up the site or [ping me on Twitter](http://twitter.com/bradparbs) for help.

## Site Deletion

`vv delete site_name`

You can also leave off site_name to be prompted for it.

Deleting a site does the following:

* Halts Vagrant (if running)
* Deletes the site's web root (which deletes the `vvv-init.sh`, `wp-cli.yml`, and `vvv-hosts` files as well)
* Deletes the file in the `nginx-config` folder pertaining to the site
* Deletes the database associated with the site

## Deployments

`vv deployment-create`, `vv deployment-remove`, `vv deployment-config`

vv supports setting up deployments that work with [Vagrant Push](https://docs.vagrantup.com/v2/push/index.html). You'll need to be on version 1.7.0 or later of Vagrant. Simply run `vv --deployment-create` and walk through the wizard.

To deploy a site, you can do `vv vagrant push <sitename>-<deployment_name>`.

When removing a deployment, your current Vagrantfile will be backed up as Vagrantfile-backup.


## Advanced Usage

Anything that vv prompts you for, you can pass in as an argument. Most of this is realized in the site creation. In fact, there are a few arguments you can pass in that aren't prompted. This gives you total control over creating a new site.

To create a new site named 'mysite' that has the domain 'mysite.dev' and is a multisite with subdomains, with `WP_Debug` turned on would be:

`vv create -d mysite.dev -n mysite -m subdomains -x`

Or, the more readable version with all expanded flags.

`vv create --domain mysite.dev --name mysite --multisite subdomains --debug`

To use a custom database prefix, simply use the `vv create --prefix myprefix` when creating a new site.

## Blueprints

Blueprints allow you to set up different plugins, themes, mu-plugins, options, or constants that will be installed to a new site you create. First, run `vv --blueprint-init` to have vv create a `vv-blueprints.json` file in your VVV directory. You can edit this file to create and set up different blueprints.

The blueprint should look like this:
```json
{
  "sample": {
    "themes": [
      {
        "location": "automattic/_s",
        "activate": true
      }
    ],
    "mu_plugins": [
      {
        "location": "https://github.com/WebDevStudios/WDS-Required-Plugins.git"
      }
    ],
    "plugins": [
      {
        "location": "https://github.com/clef/wordpress/archive/master.zip",
        "version": null,
        "force": false,
        "activate": true,
        "activate_network": false
      },
      {
        "location": "cmb2",
        "version": "2.0.5",
        "force": false,
        "activate": true,
        "activate_network": false
      },
    ],
    "options": [
      "current_theme::_s"
    ],
    "demo_content": [
      "link::https://raw.githubusercontent.com/manovotny/wptest/master/wptest.xml"
    ],
    "defines": [
      "WP_CACHE::false"
    ]
  }
}

```

For themes, plugins, and mu-plugins, you can use:

* Github username/repo
* Full git url
* Url to zip file
* WordPress.org slug

The options for plugins and themes correspond to the equivalent [WP CLI](http://wp-cli.org) option.

For options, demo content, and constants, please note the `::` as a separator between the key and value.

Custom demo content can be imported through the blueprint. Be sure to use a link that points to just the xml code, like [this](https://raw.githubusercontent.com/manovotny/wptest/master/wptest.xml). You can add as many demo content files as you'd like, just separate each line with a comma as usual.

You can create as many named blueprints in this file as you would like, all with as many different settings as you'd like.

When creating a site, the name you've specified (in this example, "sample") is what you'll need to specify to use this blueprint.

You can use 'SITENAME' or 'SITEDOMAIN' anywhere in the blueprint, and that will be replaced with the actual site name or local domain when installing.

## Vagrant Proxy

Because vv knows where you VVV installation is, you can run it from anywhere. vv will proxy any commands passed into `vv vagrant <command>` to your VVV location. So `vv vagrant halt` will halt your VVV vagrant, no matter where you run it.

## `vv` Options

|Option|Description|
|------|-----------|
|`--help`, `-h`| Show help and usage|
|`--version`|Show current vv version number.|
|`--about`|Show about screen.|
|`--update`|Updates vv to the latest stable version|
|`--debug-vv`|Outputs all debugging info needed for bug reporting.|
|`--path`,	`-p`|Path to VVV installation|
|`--force-path`, `-fp`|Override vv auto-VVV locating|
|`--force-sites-folder`,`-fsf`|Override sites folder directory locating|
|`--defaults`|Accept all default options and skip the wizard. You can also run `yes | vv create ... other flags ... -defaults` to skip the final confirmation|

### Commands

|Command|Description|
|-------|-----------|
|`list`, `--list`,  `-l`|List all VVV sites|
|`create`, `--create`, `-c`|Create a new site|
|`delete`, `--delete`, `-r`|Delete a site|
|`deployment-create`, `--deployment-create`|Set up deployment for a site|
|`deployment-remove`, `--deployment-remove`|Remove deployment for a site|
|`deployment-config`, `--deployment-config`|Manually edit deployment configuration|
|`blueprint-init`, `--blueprint-init`|Initalize blueprint file|
|`vagrant`, `v`, `--vagrant`, `-v`|Pass vagrant command through to VVV.|

### Options for Site Creation

|Option|Description|
|------|-----------|
|`--name`, `-n`|Desired name for the site directory (e.g. mysite)|
|`--domain, -d`|Domain of new site|
|`--webroot`, `-wr`|Subdirectory used for web server root|
|`--bedrock`, `-bed`|Creates Roots.io Bedrock install|
|`--blueprint`, `-b`|Name of blueprint to use|
|`--live-url`, `-u`|Live URL of site|
|`--files`, `-f`|Do not provision Vagrant, just create the site directory and files|
|`--images`, `-i`|Load images by proxy from the live site|
|`--wp-version`, `-wv`|Version of WordPress to install|
|`--debug`, `-x`|Turn on WP_DEBUG and WP_DEBUG_LOG|
|`--multisite`, `-m`|Install as a multisite. Can also pass in "subdomain" or "subdirectory"|
|`--sample-content`,`-sc`|Adds sample content to site.|
|`--username`|Admin username|
|`--password`|Admin password|
|`--email`|Admin email|
|`--prefix`|Database prefix to use|
|`--git-repo`,`-gr`|Git repo to clone as wp-content|
|`--path`,	`-p`|Path to VVV installation|
|`--force-path`, `-fp`|Override vv auto-VVV locating|
|`--blank`|Creates blank VVV site, with no WordPress|
|`--blank-with-db`|Creates a blank VVV site, with a database|
|`--wpskeleton`, `-skel`|Creates a new site with the structure of [WP Skeleton](https://github.com/markjaquith/WordPress-Skeleton)
|`--database`,`-db`|Imports a local database export|
|`--remove-defaults`,`-rd`|Removes default themes and plugins|
|`--language`,`--locale`|Install WP in another locale. Need to pass the locale afterwards, like so: `vv create --locale fr_FR`|

### Options for Site Removal
|Option|Description|
|------|-----------|
|`--name`, `-n`|Desired name for the site directory (e.g. mysite)|
|`--path`,	`-p`|Path to VVV installation|
|`--force_path`, `-fp`|Override vv auto-VVV locating|

### Options for Deployment Setup
|Option|Description|
|------|-----------|
|`--name`, `-n`|Desired name for the site directory (e.g. mysite)|
|`--deployment-name`|Name of deployment (production, staging, other, etc)|
|`--host`|Host (if SFTP, define port as host:port) |
|`--username`|FTP Username |
|`--password`|FTP Password  |
|`--passive`|Use Passive transfer mode? (y/n)  |
|`--secure`|Use SFTP? (y/n) |
|`--destination`|Destination path ( You probably want / or ~/public_html ) |
|`--confirm-removal`|Used when removing a deployment to skip the confirmation prompt |


## `.vv-config`

The first time you run `vv`, it will attempt to locate your VVV installation location. If it can't find it, you will be prompted for it. This will be written to a .vv-config file in your home directory. (`~/.vv-config`) You can also edit this file if you need to change your VVV path.

Also, if `vv` detects a `.vv-config` file in your current directory, this local file will override the one in your home directory. A use case would be to have several different `VVV` installations, that each contain their own local `.vv-config` file. Provided that you enter the appropriate directory before sending commands to `vv`, this effectively allows you to manage several different installations through one user account.

You can also add `"auto_update_disable": false` to this file to disable auto-update functionality.


## `vv` Hooks

`vv` has support for extensibility within the 'hooks' system present. This allows for quite a lot of extensibility and injection into the `vv` process. This system allows you to add your own code to run within almost any point with `vv`.

To get started with hooks, run any `vv` command with `--show-hooks` at the end. For example, `vv list --show-hooks` will run `vv list` as normal, but will also show all the hooks available.

To create the folder that your hook code should live in, simply make a 'vv' folder inside of your VVV folder.

To add code to run for a hook, make a file within your vv folder inside of VV named the hook that you want to add to. This file can be any command line runnable language, and will be executed inline.

For example, saving this file as the name of any hook will output 'Hello' when that hook gets called.

```bash
    #! /usr/bin/php
    echo 'Hello'
```

Another example would be running npm install inside of wp-content for all new sites.

Make a file named post_site_creation_finished. This file gets 4 variables passed in: the hook name, the name of the site folder, the site domain, and the VVV path.

```bash
    #!/bin/bash
    cd www/"$2"/htdocs/wp-content || exit
    npm install
```

## Questions?

Ping me on Twitter at [@bradparbs](http://twitter.com/bradparbs).

## Thanks

Forked and based off of [vvv-site-wizard from Alison Barrett](https://github.com/aliso/vvv-site-wizard).
Also thanks to [creativecoder](https://github.com/creativecoder), [jtsternberg](https://github.com/jtsternberg), [tnorthcutt](https://github.com/tnorthcutt), [joehills](http;//github.com/joehills), [gregrickaby](https://github.com/gregrickaby), [leogopal](https://github.com/leogopal), [Mte90](https://github.com/Mte90), [Octopixell](https://github.com/Octopixell), [wpsmith](https://github.com/wpsmith), [WPProdigy](https://github.com/WPprodigy), [caseypatrickdriscoll](https://github.com/caseypatrickdriscoll), [michaelbeil](https://github.com/michaelbeil), [wesbos](https://github.com/wesbos), [Ipstenu](https://github.com/Ipstenu) for awesome contributions.
