# Setup GitLab Runner

## MacBook

Install GitLab Runner:
```
brew install gitlab-runner
```

Register:
```
gitlab-runner register
```
- GitLab instance URL: Your gitlab server (e.g. git.uwaterloo.ca)
- Registration token: Found in **Settings -> CI/CD -> Runners** on GitLab
- Executor type: `shell`

Start the runner:
```
gitlab-runner run
```

## Azure VM - Ubuntu VM

### Create an Ubuntu VM
1. Go to Azure Portal. 

2. Create a Virtual Machine
   - Create a resource -> Ubuntu Server
   - Ubuntu 22.04 LTS (recommended)
   - Create

3. Configure basic settings
   - Select your Azure subscription
   - Resource Group: Create new, name it something like `GitLabRunner-RG`
   - Virtual Machine Name (e.g., `GitLabRunner-VM`)
   - Region: Choose a nearby Azure region
   - Image: Ubuntu 22.04 LTS
   - Size: Choose Standard B2s (2 vCPUs, 4 GB RAM) for cost efficiency

4. Set up authentication
   - Authentication type: SSH public key (recommended) or Password.
   - Username (e.g., azureuser).
   - If using SSH key:
       - Download your private key. It should be named something like `GitLabRunner-UbuntuVM_key.pem`
       - Move to the directory where your key is located and set the correct permissions:  `chmod 400 GitLabRunner-UbuntuVM_key.pem`
       - SSH into your VM by:  `ssh -i GitLabRunner-UbuntuVM_key.pem azureuser@your-vm-ip`

5. Networking
   - Allow SSH (port 22)

6. Review & Create
   - Review + Create
   - Create

7. Connect to your VM
   - In the VM Overview section, find Public IP
   - SSH into the VM `ssh azureuser@<VM_PUBLIC_IP>` (exception for using SSH key, see section 4)

### Install GitLab Runner

Download the GitLab Runner binary
```
sudo curl -L --output /usr/local/bin/gitlab-runner "https://s3.dualstack.us-east-1.amazonaws.com/gitlab-runner-downloads/latest/binaries/gitlab-runner-linux-amd64"

```

Give it execute permissions
```
sudo chmod +x /usr/local/bin/gitlab-runner
```

Create a GitLab Runner user
```
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
```

Install and start the runner as a service
```
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
sudo gitlab-runner start
```

### Register the Runner with GitLab

Find your GitLab Registration Token in **GitLab -> Your Project -> Settings -> CI/CD -> Runners**. 

Replace `<GITLAB_URL>` and `<REGISTRATION_TOKEN>` with your GitLab instance URL and token:
```
sudo gitlab-runner register \
  --url "https://gitlab.com/" \
  --registration-token "<REGISTRATION_TOKEN>" \
  --executor "shell" \
  --description "Ubuntu Runner" \
  --tag-list "ubuntu,linux" \
  --non-interactive
```



## Azure VM - Linux Virtual Machine

### Create a Linux VM
1. Go to Azure Portal. 

2. Create a Virtual Machine
   - Virtual Machines
   - Linux Virtual Machine
   - Create

3. Configure basic settings
   - Select your Azure subscription
   - Resource Group: Create new, name it something like `GitLabRunner-651`
   - Virtual Machine Name (e.g., `GitLabRunner-VM`)
   - Region: Choose a nearby Azure region
   - Image: Ubuntu 22.04 LTS
   - Size: Standard_B1s

4. Set up authentication
   - Authentication type: SSH public key
   - Username (e.g., azureuser).
   - If using SSH key:
       - Download your private key. It should be named something like `GitLabRunner-VM_key.pem`
       - Move to the directory where your key is located and set the correct permissions:  `chmod 400 GitLabRunner-VM_key.pem`
       - SSH into your VM by:  `ssh -i GitLabRunner-VM_key.pem azureuser@your-vm-ip`

5. Networking
   - Allow SSH (port 22)

6. Review & Create
   - Review + Create
   - Create

7. Connect to your VM
   - In the VM Overview section, find Public IP
   - SSH into the VM `ssh azureuser@<VM_PUBLIC_IP>` (exception for using SSH key, see section 4)

### Install GitLab Runner

Download the GitLab Runner binary
```
sudo curl -L --output /usr/local/bin/gitlab-runner "https://s3.dualstack.us-east-1.amazonaws.com/gitlab-runner-downloads/latest/binaries/gitlab-runner-linux-amd64"

```

Give it execute permissions
```
sudo chmod +x /usr/local/bin/gitlab-runner
```

Create a GitLab Runner user
```
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
```

Setup the password as `ece651`
```
sudo passwd gitlab-runner
```

Install dependencies
```
sudo apt update && sudo apt install -y curl git docker.io
```

Enable and start Docker
```
sudo systemctl enable docker
sudo systemctl start docker
```

### Register the Runner with GitLab

Find your GitLab Registration Token in **GitLab -> Your Project -> Settings -> CI/CD -> Runners**. 

Replace `<GITLAB_URL>` and `<REGISTRATION_TOKEN>` with your GitLab instance URL and token:
```
sudo gitlab-runner register \
  --url "https://git.uwaterloo.ca" \
  --registration-token "<REGISTRATION_TOKEN>" \
  --name "GitLabRunner-VM" \
  --executor "docker" \
  --default image "alpine" or "ubuntu:latest"
```

Configuration (with the authentication token) was saved in "/etc/gitlab-runner/config.toml"

Start and Enable the Runner
```
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
```
```
sudo gitlab-runner start
```
```
sudo systemctl enable gitlab-runner
```


