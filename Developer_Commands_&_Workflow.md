### *List of commands to know when working in a production environment.*

1. When starting to work on repo, we usually clone a repo, by default the `main` brach gets cloned
   ```bash
   git clone <repo-link>
   ```

2. Sometimes, the main branch will not have any code or we will be asked to work on (usually) `developer` or `dev` branch.
   We do run the following command:
   ```bash
   # first to check if we are on main branch, we do :
   git branch

   # then we change our branch to dev, we do
   git checkout dev

   # you can optionally run the following command if you want to remove any local changes to dev
   # this bring your local dev fully up to date.
   git pull origin dev
   ```

3. Creating a new branch:
   - Now there will be many developers who will work on dev, & you will be working on some feature.
   - We now have to create a new branch & name it (usually) our JIRA task ID, say : ID-69420.
   ```bash
   git checkout -b ID-69420
   ```

4. Pushing changes to new branch :
   ```bash
   git add .

   # commit changes
   git commit -m "ID-69420 : added a new feature"

   # push branch to GitHub
   git push -u origin ID-694220
   ```

> [!TIP]
> If you already have local changes on `dev` branch, you don't have to run the commands in point 1 & 2   
> You can directly start from commands in point 3:   
> *`git checkout -b ID-69420    `* <br>
> *`git add .   `*<br>
> *`git commit -m "ID-69420 : added a new feature"   `*<br>
> *`git push -u origin xyz123 # this is done only first time push, later you can only use push, pull etc.   `*<br>

5. Why we use `-u` for first push:
   - The `-u` => --ser-upstream
   - It tell Git :
     > From now on, this local brach (ID-69420) should track `origin/ID-69420`
   - So after the first push, git will remember the following relationsip:
     > local xyz123  ↔  remote origin/xyz123
   - to check if a upstream is already set, we run
     ```bash
     git branch -vv
     ```
   - If we see something like :
     ```
     * ID-69420  text [origin/ID-69420] ID-69420 : added a new feature
     ```
   - The `[origin/ID-69420]` part confirms tracking is set.
  
6. Now, if we want to merge the changes to `dev` branch, we do the follwoing :
   - Go to GitHub & then to our repo
   - Clikc compare & pull request
   - Base branch : dev
   - Compare branch : ID-69420
   - Get Review Done
   - Click Merge
  
7. If you wanna merge locally (Not recommended), you can do:
   - switch to dev
     ```bash
     git checkout dev
     ```
   - update dev branch:
     ```bash
     git pull origin dev
     ```
   - Merge our feature branch:
     ```bash
     git merge ID-69420
     ```
   - Push updated `dev` to GitHub:
     ```bash
     git push origin dev
     ```
   - Optional *_clean up_* (after merge)
     ```bash
     # Delete local branch
     git branch -d ID-69420

     # Delete remote branch
     git push origin --delete ID-69420
     ```
  




