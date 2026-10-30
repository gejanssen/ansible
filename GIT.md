#Hints and tips regarding git

## initial setup
git clone git@github.com:/gejanssen/ansible

git config --global --edit

[user]
        name = Gé Janssen
        email = email-address

import current ssh key into github

## check files
git status


## edit files and comment the update
git add filename_of_file
git commit -m "changed files"

## upload files
git push origin main
