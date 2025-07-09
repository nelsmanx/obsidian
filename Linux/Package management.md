-  ? flatpack
-  ? AppImage

### What is the difference between `apt` and `apt-get`

The `apt` command is meant to be pleasant for end users and does not need to be backward compatible like `apt-get`.

While `apt` is a command-line tool, it is intended to be used interactively, and not to be called from non-interactive scripts. The `apt-get` command should be used in scripts (perhaps with the `--quiet` flag). For basic commands the syntax of the two tools is identical.
___

### Updating the package index

The APT package index is a database of available packages from the repositories defined in the `/etc/apt/sources.list` file and in the `/etc/apt/sources.list.d` directory. To update the local package index with the latest changes made in the repositories, type the following:

```bash
sudo apt update
```
___

### Upgrading packages

Installed packages on your computer may periodically have upgrades available from the package repositories (e.g., security updates). To upgrade your system, first update your package index and then perform the upgrade

```bash
sudo apt upgrade 
```

There is another way to provide a complete upgrade by using the command below:

```bash
sudo apt full-upgrade
```

`full-upgrade` works the same as `upgrade` except that if system upgrade needs the removal of a package already installed on the system, it will do that. Whereas, the normal upgrade command won’t do this.

The fastest and the most convenient way to update Ubuntu system by using this command:
```bash
sudo apt update && sudo apt upgrade -y
```
___

### Installing packages

```bash
sudo apt install nmap
```

>**Tip**:  
  You can install or remove multiple packages at once by separating them with spaces.
___

### Installing `.deb` packages

```bash
cd ~/Downloads/
sudo dpkg -i vivaldi-stable_3.8.2259.42-1_amd64.deb
```
___

### Removing packages

To remove the package installed in the previous example, run the following:

```bash
sudo apt remove nmap 
```

Another way of uninstalling packages is to use purge. The command is used in the following manner:

```bash
sudo apt purge nmap 
```

- `apt remove` just removes the binaries of a package. It leaves residue configuration files.
- `apt purge` removes everything related to a package including the configuration files.

If you used `apt remove` to a get rid of a particular software and then install it again, your software will have the same configuration files. Of course, you will be asked to override the existing configuration files when you install it again.

Purge is useful when you have messed up with the configuration of a program. You want to completely erase its traces from the system and perhaps start afresh. And yes, you can use `apt purge` on an already removed package.

Usually, `apt remove` is more than enough for uninstalling a package.
___

### How to clean your system with apt

You can still use the autoremove option and free up some diskspace:

```bash
sudo apt autoremove
```

This command removes libs and packages that were installed automatically to satisfy the dependencies of an installed package. If the package is removed, these automatically installed packages, though useless, remains in the system.
___

### Searching packages

Search can be used to search for the given regex term(s) in the list of available packages and display matches. This can e.g. be useful if you are looking for packages having a specific feature.

```bash
apt search <package>
```
___

### Edit sources list

 `edit-sources` lets you edit your `sources.list` files in your preferred text editor while also providing basic sanity checks.

```bash
sudo apt edit-sources
```
___

### Display a list of installed packages

Если вы установили программу через `apt`, то список установленных пакетов можно посмотерть через команду:

```bash
sudo apt list --installed 
sudo apt list --installed | grep wget
```
___
Если вы установили `.deb`-пакет вручную через `dpkg -i`, и этот пакет не был добавлен в репозитории, которые отслеживает `apt`, то команда `apt list --installed` может не показать его, так как `apt` работает с репозиториями, а не с локально установленными `.deb`-пакетами.

Чтобы проверить, установлен ли пакет через `dpkg`, используйте команду:

```bash
dpkg -l | grep code
```

Это покажет все пакеты, установленные через `dpkg`, включая те, которые были установлены вручную.
___
`snap` — это отдельная система управления пакетами, которая работает независимо от `apt` и `dpkg`. Когда вы устанавливаете программу через `snap`, она не регистрируется в системе `apt` или `dpkg`.

Поэтому команда `snap list` показывает установленные `snap`-пакеты, включая `code`, если он был установлен через `snap`.
___
Чтобы понять, как именно установлен `code`, выполните команду:

```bash
which code
```

Это покажет путь к исполняемому файлу. Если путь содержит `/snap/`, значит, программа установлена через `snap`. Если путь указывает на `/usr/bin/` или `/usr/local/bin/`, то, скорее всего, она установлена через `.deb`-пакет или вручную.
___
### Рекомендации

- Используйте `apt` для управления пакетами, если это возможно, так как он автоматически разрешает зависимости.
- Для настольных приложений можно использовать `snap` или `flatpak`, так как они предоставляют изолированные среды.
- Если вы устанавливаете пакеты вручную через `.deb`, убедитесь, что они из надежных источников.