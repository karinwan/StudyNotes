# Github

## Initialize a new repository in github
#### 1. On the website: 
- Click on the '+' botton on the right-top of the webpage to create a new repository;
- Set repo name, description, and create README.md by default. 

#### 2. On Git Bash:
- Write your description into a new file called <README.md>:
```
echo "# StudyNotes" >> README.md
```
- Initialize Git:
```
git init
```
- 'git add <filename>' adds the file you are editing to the staging area:
```
git add README.md
```
- A commit permanently stores changes from the staging area inside the repository. "message" tells what has been changed. 
```
git commit -m "first commit"
```
- Create the main branch:
```
git branch -M main
```

```
git remote add origin https://github.com/karinwan/<repository_name>.git
```
- Push the changes to main branch:
```
git push -u origin main
```

## Create a new branch
- Update master
```
git pull
```
- Create the branch on your local machine and switch in this branch
```
git git checkout -b [branch_name]
```
- Push the new branch on GitHUb:
```
git push origin [branch_name]
```
- Add a new remote to the branch
```
git remote add [remote_name] [branch_name]
```
- Push changes from your commit into your branch
```
git push [remote_name] [url]
```
- Update branch with the original branch from official repository
```
git fetch [remote_name]
```
- Merge changes
```
git merge [remote_name]/develop
```


## Other git commands
- Copy all files from github repository to local: 
```
git clone <url>
```
- Check the status of your changes to the working directory:
```
git status
```
- List all commits made: 
```
git log
```
- List all branches created:
```
git branch -a
```
- Delete a branch on GitHub:
```
git push origin :[branch_name]
```


