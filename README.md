# Cool Kids Corporation

GIT training for the coolest kids from San Francisco, Dallas/Portland, Gangneung, Yerres and Lyon.

<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#how-to-get-started">How to get started</a>
      <ul>
        <li><a href="#cloning-the-repository">Cloning the repository</a></li>
      </ul>
    </li>
    <li>
      <a href="#how-to-survive">How to survive</a>
      <ul>
        <li><a href="#pushing-your-commits">Pushing your commits</a></li>
        <li><a href="#pulling-changes">Pulling changes</a></li>
        <li><a href="#working-with-branches">Working with branches</a></li>
        <li><a href="#correcting-your-mistakes">Correcting your mistakes</a></li>
      </ul>
    </li>
  </ol>
</details>

## How to get started

You have created your github account and desire to contribute to your new project but you don't know how to start. Don't worry, this guide was created by someone who ruined countless Gitlab/Github repositories.

### Cloning the repository

The first step is to clone a repository. You have two options that both work fine: HTTPS or SSH.

I personally prefer SSH like a lot of CS nerds, mainly because sometimes you might need to authenticate to push or pull with HTTPS. Already happened before but not sure exactly when it does. SSH is also a secure and encrypted protocol. ([read more](https://phoenixnap.com/kb/git-ssh-vs-https))

If you want to opt for SSH, follow these steps:

1. Open a git bash terminal

2. Generate an SSH key with the following command (press enter for everything):
   ```sh
   ssh-keygen -t ed25519 -C "yeri@redvelvet.com"
   ```
3. Copy the public SSH key to your clipboard
   ```sh
   cat ~/.ssh/id_rsa.pub | pbcopy
   ```
4.  Go to GitHub --> Settings --> SSH and GPG keys --> New SSH key

5. Paste the public key

6. Configure the name/email used for your git operations in a terminal:
   ```sh
   git config --global user.name "Yeri"
   git config --global user.email "yeri@redvelvet.com"
   ```
## How to survive

Now that you are all set, you have decided to code and now you have changes in your repository. How can you see that? 

Try to run this command:
   ```sh
   git status
   ```
You can see that you have "Untracked files" that correspond to the files that you created/modified.

### Pushing your commits

In order to push these changes to the remote repository on GitHub, you need to follow three steps.

#### 1. Stage the changes

The first one is to stage the changes with this command:
   ```sh
   git add <file>
   ```

or this one if you want to stage ALL files:
   ```sh
   git add -A
   ```

By doing so, you tell your git that you are about to commit this particular state of the file. You should not modify a file that is staged. You can always unstage a file with:
   ```sh
   git restore --staged <file>
   ```

#### 2. Commit the changes

Now that you have staged changes, the next step is to actually commit. A commit needs a commit message. You need to run this command:
   ```sh
   git commit -m "commit message"
   ```

#### 3. Push the commit

Lastly, you have to push your commit to the remote repository:
   ```sh
   git push
   ```
You may have commits multiple commits and then push all at once as well. This way in case you realize that you need to modify something, you can always reset a local commit with a command I will detail later.

### Pulling changes

Sometimes, changes have been made to the remote repository. This is not synced automatically with your local repository and you will need to pull these changes.

This command will do the trick and will pull all changes for all branches (je crois)
   ```sh
   git pull
   ```

### Working with branches

#### 1. Going to a branch

Branching is crucial and the most confusing part of git. Creating and navigating between branches is easy with this command:
   ```sh
   git checkout <branch_name>
   ```
You can always delete the branch:
   ```sh
   git branch -d <branch_name>
   ```

#### 2. Merging branches

Now that you are in your branch, you are now on a different path from the main branch you originated from. The tricky part is that if you modify some files and those files are also modified in the original branch, you will have conflicts that you will need to solve once you decide to merge.

To my knowledge, I have used two different techniques to merge branches: one involving "git rebase" and the other one that does not. I have lost knowledge of the former and is dangerous if not mastered. The other one is a bit easier and I will detail here.

Basically, you have made a bunch of commits in your branch so has your teammate on the main branch. This is an important step that you need to follow. You need to get all the changes from the main branch into your branch before being able to merge.
   ```sh
   git checkout main_branch
   git pull
   git checkout dev_branch
   git merge main_branch
   ```  
You might run into conflicts in this step.

#### 3. Managing conflicts

Conflicts occur when you have two different versions of a same file (or portion of a file) that collude and you will need to decide which version you want to keep. You are also free to modify as you see fit. What you want at the end is a version of the file that does not have this type of lines:
   ```sh
   <<<<<< HEAD
   ............
   ===
   >>>>>>
   ```  

Once you are done with the modification, you will need to stage the changes. I am used to VScode way of handling conflicts but it should be similar with other IDEs.

#### 4. Creating a pull request

Now that you have pulled all the changes from main branch to your current branch, you are now ready to create a pull request (merge request in GitLab).

Just go to GitHub --> Pull Requests --> Create pull request

Sometimes, your boss will ask you to make some changes, do not worry about making additional commits. You can at anytime squash these "useless" modification commits into a single one when you actually merge the branches.

Once your pull request is approved, the main branch will accept all the incoming commits and its history will reflect your changes made on your branch.

### Correcting your mistakes

Sometimes you screwed up badly. You are in a tough position, sprint meeting is 30 minutes away and you duplicated 12 commits in the dev branch history. You feel the HR call threatening you. Rest assured, you can always save yourself. (to some extent)

#### Case 1: Commits not yet pushed to the repository

You are lucky, you have not pushed yet but you still realize your commits are lacking something or you want to modify something. This is best practice to just modify the previous commit that involves a functionality rather than creating a new commit that changes something on top of it. This command is useful:
   ```sh
   git reset --safe HEAD~1
   ``` 
This will reset the last commit you made and unstage all the files that were made in it. The "safe" means that those changes will not be deleted. You can change "1" by any number you desire to reset as many commits as you want.

This (dangerous) command is equivalent but all the changes made in the commit(s) will be deleted:
   ```sh
   git reset --hard HEAD~1
   ```

#### Case 2: Commits pushed to the repository

##### Deleting commits

You have screwed up big time but you need to be saved. You want to delete those commits that mess up the history. Basically, you run the previous command (hard) to revert you back to the last commit that is good. Then you run this to push the deletion in the remote repository:

   ```sh
   git push origin main --force
   ```

##### Modifying a commit

Sometimes you just want to modify the commit that you have pushed. It is simple, just do your modifications, stage the changes and then run this command:
   ```sh
   git commit --amend --no-edit
   ```

If you want to change the commit message you can do this:
   ```sh
   git commit --amend -m "new commit message"
   ```

Then finally:
   ```sh
   git push --force
   ```

Beware that this will modify the history (date etc). It is still a useful way to modify a commit.

## Conclusion

This is most of what I know about git, there are some stuff I am too newbie to do (rebasing, changing n-th ago commit etc...) but I hope it will be enough to start.

Denis
