## Quick Note for Git SSH

Open terminal

```
ssh-keygen -t rsa -C "your_email@example.com"
```

Copy .pub file and add ssh key in GitHub

```
ssh-add ~/.ssh/id_rsa
```

Remote git repo setting

```
git remote set-url origin git@github.com:user_names/repo_name.git
```