
### `sudo`

`sudo` (Super User DO) allows you to execute commands as the superuser (root) or another user without fully switching to that user.  

```bash
sudo <command>               # as a root
sudo -u username <command>   # as a username
```

This command ==requires the password of the current user== (if the user is listed in the `/etc/sudoers` file).
The `/etc/sudoers` file defines who can use `sudo` and which commands are allowed.  

> It can be edited using the `visudo` command, which ensures syntax checking to avoid errors.  Do not use a text editor! 
> If you make a syntax error while using the `visudo` command, the program will prompt you to correct it: 

```bash
>>> /etc/sudoers: syntax error
What now? 

# x - exit without any changes
# e - edit file
# q - NEVER USE - save changes and quit
```

Example entry in `/etc/sudoers`. This allows `username` to run any command as any user on any host.

```bash
username ALL=(ALL:ALL) ALL
```

You can also restrict commands. This allows `username` to run only `apt` and `systemctl` commands as root.

```bash
username ALL=(ALL) /usr/bin/apt, /usr/bin/systemctl
```

Another option to use `sudo` - add the current user to the `wheel` or `sudoers` group.
```bash
sudo usermod -aG wheel losst  # For CentOS/RHEL
sudo usermod -aG sudo losst   # For Ubuntu/Debian

cat /etc/group | grep sudo    # Check members of the `sudo` group
groups                        # Check current user's groups

exit                          # Takes effect after next login
```
#### Common options

`-i`: Start a login shell as the target user (similar to `su -`).

```bash
sudo -i
```

`-l`: List the allowed (and forbidden) commands for the user.

```bash
sudo -l
```


> You can configure `sudo` to not require a password for specific users or commands.  Example in `/etc/sudoers`:

```bash
username ALL=(ALL) NOPASSWD: ALL
```

> By default, `sudo` remembers your password for 5 minutes. You can change this by adding the following to `/etc/sudoers`. This sets the timeout to 10 minutes. Set it to `0` to always require a password.

```bash 
> Defaults timestamp_timeout=10 
```

> [!NOTE]
> The `sudo !!` command is a quick way to **re-run the last executed command with root privileges**. 
> ```bash
> apt update
> > E: Could not open lock file /var/lib/apt/lists/lock - open (13: Permission denied)
> 
> sudo !!
> # executes sudo apt update
> ```

___

### `su`

`su` (Switch User) allows you to switch to another user, including the root user. By default, it switches to the root user if no username is specified.  ==Requires the password of the target user.== However, if the `su` command is run by the root user, no password is required.

```bash
# switch to root
su

# switch to a username
su username
```

#### Common options

`-` or `-l`: Start a login shell (loads the target user's environment variables).

```bash
su - username
```

The difference between `su - username` and `su username` lies in how the **user environment** is set up when switching to the target user. 

`su username`:
Switches to the specified user (`username`) but **preserves the current environment** (e.g., environment variables, working directory, shell settings).

Behavior:
- The shell remains the same as the original user's shell.
- The working directory does not change.
- Environment variables (e.g., `HOME`, `PATH`, `USER`) are inherited from the original user.

`su - username`:
Switches to the specified user (`username`) and **simulates a full login shell** for that user. This means it loads the target user's environment.

Behavior:
- The shell changes to the target user's default shell (as defined in `/etc/passwd`).
- The working directory changes to the target user's home directory.
- Environment variables (e.g., `HOME`, `PATH`, `USER`) are set according to the target user's profile files (e.g., `~/.bashrc`, `~/.profile`).
___
### Key differences between `sudo` and `su`

| Feature      | `sudo`                                | `su`                                 |
| ------------ | ------------------------------------- | ------------------------------------ |
| **Purpose**  | Execute commands as another user.     | Switch to another user's session.    |
| **Password** | Requires the current user's password. | Requires the target user's password. |
| **Session**  | Temporary command execution.          | Full session switch.                 |
| **Logging**  | Actions are logged.                   | No logging by default.               |
| **Security** | More secure (limited permissions).    | Less secure (full access).           |
___

### Why use `sudo -i` instead of `su` when root has no password?

In some Linux distributions (e.g., Ubuntu), the **root account is disabled by default**, meaning it does not have a password. This is a security measure to prevent unauthorized access to the root account. Here’s why and how this works:

#### Why Root Has No Password

- **Security Best Practice:**  
    Disabling the root password ensures that no one can directly log in as root or use `su` to switch to the root account. This reduces the risk of brute-force attacks or accidental misuse of the root account.
- **Encourages `sudo` Usage:**  
    By disabling the root password, users are forced to use `sudo` for administrative tasks. This provides better logging, accountability, and control over privileged actions.


####  Why `su` Doesn’t Work Without a Root Password

Command `su` or `su root` will prompt you for the root password. If the root account has no password, this will fail.


#### Why `sudo -i` works

The `sudo` command allows authorized users to execute commands as another user (including root) **without needing the target user’s password**. Instead, it requires the **current user’s password** (if they are in the `sudoers` file).

The `-i` option in `sudo` starts a **login shell** for the target user (root, by default). It simulates a full login environment, loading the root user’s profile and environment variables.

#### How to Enable Root Password (Not Recommended)

If you really want to enable the root password (e.g., to use `su`), you can do so by setting a password for the root account:

```bash
sudo passwd root
```

After this, you can use `su` to switch to the root account. However, this is **not recommended** for most users, as it reduces security.

To disable the root password and lock the root account, use the `passwd` command with the `-l` (lock) option. This will prevent anyone from logging in as root or using `su` to switch to the root account.

```bash
sudo passwd -l root
```