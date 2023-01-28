```sh
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

```
Host github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa
```