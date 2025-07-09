### Generating a new SSH key and adding it to the ssh-agent

1. Generating a new SSH key
```bash
$ ssh-keygen -t rsa -b 4096 -C "your_git_hub_email@example.com"
```

2.  Adding your SSH key to the ssh-agent
```bash
 # start the ssh-agent in the background 
 
 $ eval "$(ssh-agent -s)"  
 > Agent pid 59566
  
```

3. Add your SSH private key to the ssh-agent
```bash
$ ssh-add ~/.ssh/id_rsa
```

4. Copy the SSH public key to your clipboard
```bash
# Copies the contents of the id_rsa.pub file to your clipboard

$ clip < ~/.ssh/id_rsa.pub
```

5. Adding a new SSH key to your GitHub account

6. Check out a new SSH connection
```bash
# Attempts to ssh to GitHub

$ ssh -T git@github.com
> Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```
___

### Switching remote URLs from HTTPS to SSH

1. Open Git Bash.
2. Change the current working directory to your local project.
3. List your existing remotes in order to get the name of the remote you want to change.
```bash
$ git remote -v
> origin  https://github.com/USERNAME/REPOSITORY.git (fetch)
> origin  https://github.com/USERNAME/REPOSITORY.git (push)
```

4. Change your remote's URL from HTTPS to SSH with the `git remote set-url` command.  
```bash
$ git remote set-url origin git@github.com:USERNAME/REPOSITORY.git
```

5. Verify that the remote URL has changed
```bash
$ git remote -v   
> origin  git@github.com:USERNAME/REPOSITORY.git (fetch)  
> origin  git@github.com:USERNAME/REPOSITORY.git (push)
```


### Error git@github.com: Permission denied (publickey) after `git push`

Чтобы разово решить проблему до следующего запуска IDE, нужно в терминале VSCode ввести следующие команды:

```bash
 $ eval "$(ssh-agent -s)"  
 $ ssh-add ~/.ssh/id_rsa
```

Чтобы решить проблему перманентно, нужно добавить в файл `config` в папке `%HOMEPATH%\.ssh`

```
host github.com
 HostName github.com
 IdentityFile ~/.ssh/id_rsa
```

