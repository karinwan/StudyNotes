# Git
## Upgrade Git in Ubuntu

Run the following commands: 
Do this only if your system doesn't have `add-apt-repository: `
```
sudo apt-get install python-software-properties
sudo apt-get install software-properties-common
```
Try from here first. 
```
sudo add-apt-repository ppa:git-core/ppa -y
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install git -y
git --version
```
