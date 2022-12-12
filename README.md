# Rapid Reponse Beta site (name TBD!)

# Installation

## Requirements
This project requires the following software on OS X:
- [Docker](https://hub.docker.com/editions/community/docker-ce-desktop-mac/)
- Git
- [DDEV](https://ddev.readthedocs.io/en/stable/)
- PHP 8.1 or higher
- Composer

See [DDEV Installation](#ddev-installation) for further information.

# Working on the Project

## Local Development

### Initial installation

These instructions assume that DDEV is installed and working.

* Checkout the git repository to your `Sites` directory.
  * `git clone git@github.com:tboggia/rapidresponse.git`
* Configure `nfs` for the project directory. See [Set up NFS](#set-up-nfs).
* `cd rapidresponse`
<!-- * Login to your Pantheon Dashboard, and [https://pantheon.io/docs/machine-tokens/](Generate a Machine Token) for ddev to use.
* Run `ddev auth pantheon <YOUR TOKEN>` (This is a one-time operation, and configures ddev to work with all the sites on your account.)
* Sync the LIVE Pantheon environment down to DEV.
* Make a backup of DEV on Pantheon.
* Run `ddev pull` to load data from Pantheon's DEV environment.
* Run `ddev start` if needed. -->

*Troubleshooting*

<!-- * There are some configurations for DDEV that can be edited at `.ddev/config.yaml` if needed, or let `ddev config pantheon` guide you through the steps. -->
<!-- * If styling doesn't come through and the console has this error: `Refused to apply style from 'https://berkeley-web-services.ddev.site/' because its MIME type ('text/html') is not a supported stylesheet MIME type, and strict MIME checking is enabled.`, run `drush -y config-set system.performance css.preprocess 0 && drush -y config-set system.performance js.preprocess 0` from inside the virtual machine. [Stackoverflow explanation](https://stackoverflow.com/a/49778131/7412129).  -->

<!-- ### Getting updates

Once Pantheon integration to DDEV is setup, you can refresh your local site by making a backup to the Pantheon DEV server and then running `ddev pull`.

That's it. -->

## Using Drush aliases

Drush aliases let you use `drush` command on remote machines. To use `drush`, run `ddev ssh`.

## Deploying
Still a ?, currently I use GatorHost terminal to git pull.

# DDEV Installation

[DDEV](https://www.ddev.com/) is a local development environment for use with Drupal and WordPress. DDEV is well-supported and provides community support.

This section will cover the basic installation of DDEV, with some recommendations. You can also follow instructions at https://ddev.readthedocs.io/en/stable/
You need to install Docker Desktop for Mac first. You may need to create an account.
https://hub.docker.com/editions/community/docker-ce-desktop-mac.

Install DDEV with Homebrew:
```
brew tap drud/ddev
brew install ddev
```
See [Recommended DDEV configuration](#recommended-ddev-configuration) to complete the  recommended configuration for DDEV.

## Recommended DDEV Configuration

### Set up NFS 
This will make file changes available to the ddev environment quickly and consistently. Without NFS, files may occasionally get "stuck" at an older version.
Either follow the official documentation here Using NFS to Mount the Project into the Web Container, or use these steps:

1. Add the project path to your `/etc/exports ` file, which will look something like this:
    ```
    # DDEV
    "/System/Volumes/Data/Users/[YOUR USERNAME HERE]/Sites/berkeley-web-services" --alldirs -mapall=501:20 localhost
    ```
      - You will need to provide the path to the project. This may be within /System/Volumes/... or it may be directly within your user directory like /Users/YOU/Sites/global.
      - The 501 here may need to be replaced with your user ID. You can use the script mentioned in the documentation above if you don’t want to manually create those.

    This action requires *administrator* permissions on your machine.

2. Edit your `/etc/nfs.conf`:
    ```
    # For DDEV
    nfs.server.mount.require_resv_port = 0
    ```
    This action requires *administrator* permissions on your machine.

3. Restart the NFS service:
    ```
    sudo nfsd restart
    ```
    This action requires *administrator* permissions on your machine.

4. Finally, verify that NFS is configured correctly for the project:
    ```
    ddev debug nfsmount
    ddev restart
    ```

### Set up a certificate
This makes HTTPS work without those “This is dangerous” notices.
- SSH into the ddev container `ddev ssh`
- From inside the container, run `mkcert -install`
