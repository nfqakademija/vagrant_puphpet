# Vagrant Installation

*Note:* Copy paste commands one at a time!

## Cygwin (Windows only)

Select http server. Mirror `http://cygwin.uib.no` will work just fine.

Mandatory packages:

- interpreters/ruby
- vim

## Vagrant Installation

Install VirtualBox (https://www.virtualbox.org/wiki/Downloads).

Install Vagrant (https://www.vagrantup.com/downloads.html).

Install plugins:

```sh
vagrant plugin install vagrant-winnfsd # Windows only
vagrant plugin install vagrant-hostmanager
vagrant plugin install vagrant-bindfs # MacOSX and Linux only
```


```sh
sudo apt-get install nfs-kernel-server # Linux only (Ubuntu / Debian)
```

Clone this repository.

Run `vagrant up`. This will take 5 - 20 minutes. Monitor and allow admin access multiple times during first few minutes.

Visit http://nfqakademija.dev and check that you see "everything is awesome". Also visit /info.php to see if php is working.

## Symfony

Install Symfony:

```sh
vagrant ssh
cd /tmp
composer create-project symfony/framework-standard-edition
cp -r -n /tmp/framework-standard-edition/* /var/www/nfqakademija/
cat /tmp/framework-standard-edition/.gitignore >> /var/www/nfqakademija/.gitignore
```

Update configuration so that `/app_dev.php/` URL part would not be required:

```sh
cd /var/www/nfqakademija
sed 's/app.php/app_dev.php/g' web/.htaccess > /tmp/foo_bar_htaccess && /bin/cp /tmp/foo_bar_htaccess web/.htaccess
sed "s/'::1'/'::1', '192.168.56.1'/g" web/app_dev.php > /tmp/foo_bar_app_dev && /bin/cp /tmp/foo_bar_app_dev web/app_dev.php
```

Visit: http://nfqakademija.dev/. It should output nicely formatted Symfony hello page.

## If NFS fails

Try `smb` instead of `nfs`. Continue this chapter only if that fails also.

Disable two-way file sync. Open locally `/puphpet/config.yml` file and find `synced_folder` section. Replace sub-tree with `synced_folder: []`.

In PHPStorm set up deployment server over SSH:

- Type: `SFTP`
- SFTP host: `nfqakademija`
- Root path: `/var/www/nfqakademija`
- User name: `vagrant`
- Use private key at `{YOUR_PATH}/puphpet/files/dot/ssh/id_rsa`.
- Test your connection: `Test SFTP connection...` button.
- Save and close.

Select root project directory `nfqakademija`, use **Ctrl + Alt + A** and type in `Sync with Deployed to ...` (autocomplete). Upload all.

Next time you can select only `app` or `src` folders to save sync time.

First time sync on my laptop: `Synchronization completed in 4 minutes: 30,465 files transferred (2.7 Mb/s)`
