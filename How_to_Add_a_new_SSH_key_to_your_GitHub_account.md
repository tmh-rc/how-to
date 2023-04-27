# How to Add a new SSH key to your GitHub account

Run following command in server's terminal to generate SSH key:
```
ssh-keygen -t rsa -b 4096 -C "username@servername"
```

Copy the SSH public key to your clipboard.

In the github, click your profile photo, then click Settings.

![](https://docs.github.com/assets/cb-139735/mw-1000/images/help/settings/userbar-account-settings.webp)

- In the "Access" section of the sidebar, click `SSH and GPG keys`.
- Click `New SSH key` or `Add SSH key`.
- In the "Title" field, add a descriptive label for the new key. For example, if you're using a personal laptop, you might call this key "Personal laptop".
- In the "Key" field, paste your public key.
- Click `Add SSH key`.


Enter following command to test SSH connection:
```
ssh -T git@github.com
```

You may see following success message:

```
Hi USERNAME! You've successfully authenticated, but GitHub does not
provide shell access.
```

Done!

Ref:
- https://docs.github.com/en/github-ae@latest/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection
