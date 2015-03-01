#Tutorial Steps
##Prerequisites
1. Two virtual machines (or physical machines):
  - An "upstream" VM
  - A "fork" VM
2. Set both VMs up with Git.
3. Create upstream and fork GitHub accounts.
4. It is preferable to go ahead and cache the GitHub credentials on the respective VMs. On Windows, this can be done by using [git-credential-winstore](https://gitcredentialstore.codeplex.com/).

##Create upstream project##
1. On the upstream VM, go to https://github.com/new
2. Give the project the name great-project
3. Check the `Initialize this repository with a README` checkbox.
4. Click `Create repository`
5. Github will now take you to the repo's homepage.

##Clone the project to the upstream VM##
1. On the upstream VM click the copy button beside the `HTTPS clone URL`
2. Open Git Bash, go to the directory you would like to have the repo cloned inside.
3. Type `git clone ` then paste the URL you copied previously and execute the command.
4. Change to the `great-project` directory.

##Create an initial commit and push to GitHub##
Execute the following commands:

1. `echo "Edit 1 from upstream/master" > file1.txt`
2. `git add file1.txt`
3. `git commit -m "Commit 1 from upstream/master"`
4. `git push`

##Fork the repo from the fork VM##
1. On the fork VM go to the upstream repo URL. Copy this from the upstream VM.
2. In the upper right corner of this page, click `Fork`.
3. Now clone this repo to your fork VM.
4. Add the remote to the fork VM repo by executing the command `git remote add upstream $URL` where `$URL` is the HTTPS clone URL of the upstream repo.

##Create a feature branch on the fork VM and commit a change to `file1.txt`##
Execute the following commands:

1. `git checkout -b featurebranch`
2. `echo "Edit 1 from fork/featurebranch" >> file1.txt` (Notice the `>>`. This will append a line to `file1.txt`)
3. `git add file1.txt`
4. `git commit -m "Commit 1 from fork/featurebranch"`
5. `git push origin featurebranch`

##Create a pull request##
1. In your web browser window, the Github page for the fork should have alerted that a new branch has been pushed.
2. Click the `Compare & pull request` button
3. Notice that the pull request can be merged automatically. But not for long!
4. On the upstream VM GitHub repo homepage, click the `Pull Requests` link.
5. Click on the pull request that was just created. Leave this window visible, if possible.

##Make a change in the upstream repo##
We will now simulate another PR being merged before your's. Or simply the upstream repo owner making changes before reviewing your PR.

On the upstream VM, execute the following commands:

1. `echo "Edit 2 from upstream/master" >> file1.txt` (Notice the `>>`. This will append a line to `file1.txt`)
2. `git add file1.txt`
3. `git commit -m "Commit 2 from upstream/master"`
4. `git push`

Now, notice that both the upstream and fork views of the pull request state that the pull request cannot be automatically merged. Let's rebase the fork to pull in the changes and update the pull request.

##Rebase the fork and merge the changes##
On the fork VM, execute the following commands:

1. `git pull --rebase upstream master`
2. `git checkout featurebranch`
3. `git rebase master`

This will fail, and we will now need to merge the changes. Use your favorite text editor to merge the changes to be as follows:

```text
Edit 1 from upstream:master
Edit 2 from fork:featurebranch
Edit 3 from upstream:master
```
Now, let git know that you're done merging and continue the rebase.

1. `git add file1.txt`
2. `git rebase --continue`
3. `git push --force`

##Merge the pull request to the upstream repo##
GitHub automatically updates the pull request. Notice that the pull request pages say that the pull request can be automatically merged now. So, on the upstream VM, click `Merge pull request`
