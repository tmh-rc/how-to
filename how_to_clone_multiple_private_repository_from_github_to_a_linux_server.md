# How to clone multiple private repository from github to a linux server?

## Step 1

First, you need to generate SSH key by followning command:
```
ssh-keygen -t rsa -b 4096 -C "username@servername"
```
Generate anther SSH key with different path for anther repository
```
ssh-keygen -f /root/.ssh/new_key -t rsa -b 4096 -C "username@servername"
```

## Step 2

- Add first SSH key to first github repository
- Add second SSH key to second github repository

## Step 3

Check private key permission (read and write only for the owner). If not given permission:
```
chmod 600 /path/to/your/private_key
```

### Step 4

Use `ssh -T git@github.com` to check if your SSH connection to GitHub is working and you have proper authentication. Then clone the both repositories.


### Step 5

If cloning the second private repository is not working, 
Do following

- **Check Loaded SSH Keys**: Verify that the correct SSH key is loaded into the SSH agent. To list the keys loaded in the agent, use the following command:
```
ssh-add -l
```

Note: If `ssh-add` command not found. You need to start by following command:
```
eval "$(ssh-agent)"
```

- If the correct key is not listed, you can add it to the agent using:
```
ssh-add /path/to/your/private_key
```

- **Clear SSH Key Cache**: If you've recently added or updated the SSH key for the new repository, it's possible that the SSH agent is still using the cached old key. Clear the SSH key cache by running:
```
ssh-add -D
```

- Then clone the second private repository again. This time, it will work.
