# Gerrit Installation

## Prerequisites
To run the Gerrit service, the following requirement must be met on the host:

* JRE, versions 1.8 or 11 [Download](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Gerrit is not yet compatible with Java 13 or newer at this time.

Installation link - [https://gerrit-documentation.storage.googleapis.com/Documentation/3.1.4/install.html](https://gerrit-documentation.storage.googleapis.com/Documentation/3.1.4/install.html)

## Download Gerrit
Current and past binary releases of Gerrit can be obtained from the [Gerrit Releases site](https://gerrit-releases.storage.googleapis.com/index.html).

Download any current `*.war` package. The war will be referred to as gerrit.war from this point forward, so you may find it easier to rename the downloaded file.

## Initialize the Site

Gerrit stores configuration files, the server’s SSH keys, and the managed Git repositories under a local directory, typically referred to as `'$site_path'`.

You also have to decide where to store your server side git repositories. This can either be a relative path under '$site_path' or an absolute path anywhere on your server system. You have to choose a place before commencing your init phase.

Initialize a new site directory by running the init command, passing the path of the site directory to be created as an argument to the '-d' option. Its recommended that Gerrit Code Review be given its own user account on the host system:

```bash
  sudo adduser gerrit
  sudo su gerrit

  java -jar gerrit.war init -d /path/to/your/gerrit_application_directory
```

>NOTE
If you choose a location where your new user doesn’t have any privileges, you may have to manually create the directory first and then give ownership of that location to the `'gerrit'` user.

If run from an interactive terminal, the init command will prompt through a series of configuration questions. If the terminal is not interactive, running the init command will choose some reasonable default selections. Once the init phase is complete, you can review your settings in the file `'$site_path/etc/gerrit.config'`.

When running the init command, additional JARs might be downloaded to support optional selected functionality. If a download fails a URL will be displayed and init will wait for the user to manually download the JAR and store it in the target location.

When the init phase is complete, the daemon will be automatically started in the background and your web browser will open to the site:

```bash
  Initialized /home/gerrit/review_site
  Executing /home/gerrit/review_site/bin/gerrit.sh start
  Starting Gerrit Code Review: OK
  Waiting for server to start ... OK
  Opening browser ...
```
When the browser opens, sign in to Gerrit through the web interface. The first user to sign-in and register an account will be automatically placed into the fully privileged Administrators group, permitting server management over the web and over SSH. Subsequent users will be automatically registered as unprivileged users.

## Installation Complete

Your base Gerrit server is now installed and running. You’re now ready to either set up more projects or start working with the projects you’ve already imported.

> Gerrit available at http://192.168.2.10:8080

## Start/Stop Daemon
To control the Gerrit Code Review daemon that is running in the background, use the rc.d style start script created by 'init':

```bash
  review_site/bin/gerrit.sh start
  review_site/bin/gerrit.sh stop
  review_site/bin/gerrit.sh restart
  ```
  
Uncomment the following 3 lines in the `'$site_path/bin/gerrit.sh'` script:

```bash
 chkconfig: 3 99 99
 description: Gerrit Code Review
 processname: gerrit
 ```

Then link the `gerrit.sh` script into `rc3.d`:

```bash
  sudo ln -snf `pwd`/review_site/bin/gerrit.sh /etc/init.d/gerrit
  sudo ln -snf /etc/init.d/gerrit /etc/rc3.d/S90gerrit
```

## SSH configuration

```bash
$ cd ~/.ssh
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
$ vi ./config
```

Add below lines to `config`

```bash
Host    192.168.2.10
    Hostname    192.168.2.10
    IdentityFile    ~/.ssh/<name-of-private-key-file>
```

#### Testing SSH connecion

* sshusername : `dhruvin` (in this example)
* hostname : ``192.168.2.10`

```bash
ssh -p 29418 sshusername@hostname
```

#### Resources
* [Install Gerrit](https://gerrit-documentation.storage.googleapis.com/Documentation/3.1.4/install.html)