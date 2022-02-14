# learn-rebase

## General outline

The goal of this repository is to help you re-create the need to rebase a branch. Then perform an interactive rebase and push your results. The set of steps will generally look like:

1. Create `a-branch` that adds `a.txt` with one text line to the repo. Push `a-branch` to GitHub and open a pull request from `a-branch` to `master`
2. Create `b-branch` that comes out of `a-branch`. Make and commit an adjustment to `a.txt`. Then push `b-branch` to GitHub and open a pull request from `b-branch` into `a-branch`.
3. Squash merge `a-branch` into `master`. See that on the PR of `b-branch`, it is now pointing to `master` AND it has a conflict with master.
4. `git pull origin master` on `master` locally
5. `git checkout b-branch` 
6. `git rebase -i master` (This is **THE MAIN SHOW**)
7. `git push --force-with-lease`

## Walkthrough

You can clone this repo by running the following command. It will create a `learn-rebase` folder where you run it. After creating it, `cd` into it
```
gh repo clone agam-armis/learn-rebase
cd learn-rebase
```

Just so that you aren't conflicting with other people learning `rebase` with this repo, create a directory with your name, and add files inside that directory:
```
mkdir your_name
cd your_name
```

Add `a.txt` with one line of text, commit it to `your-name-a-branch` and push `your-name-a-branch` to GitHub:
```
echo "Hello" > a.txt
git add a.txt
git checkout -b your-name-a-branch
git commit -m "adding a.txt"
git push origin your-name-a-branch
```

Make a change to `a.txt`, commit it to `your-name-b-branch` and push `your-name-b-branch` to GitHub:

```
git checkout -b your-name-b-branch
echo "Goodbye" >> a.txt
git add a.txt
git commit -m "Concat goodbye to a.txt"
git push origin your-name-b-branch
```

Now is the time to cause a conflict between `your-name-b-branch` and `master`:
* Go to GitHub
* Squash merge `your-name-a-branch`
* Check the PR for `your-name-b-branch` and see that it has a conflict with master


## It's rebase time!

Very important to first pull master (or fetch, but that's for a different session). So we'll pull `master` and checkout `your-name-b-branch` again:
```
git checkout master
git pull
git checkout your-name-b-branch
```

And now, for the crucial moment:
```
git rebase -i 
``` 

After running this, you will get somehing like the following in your text editor of choice:

```
pick 2fd8ac1 adding a.txt
pick 308cc30 Concat goodbye to a.txt

# Rebase bb4765b..308cc30 onto bb4765b (1 command)
#
....
```

What we want here is to only keep the `Concat goodbye to a.txt`. To get this result we need to remove the other commit's line and save the file. You should get a success message. Now push the new `your-name-b-branch` but you should use the `--force-with-lease` flag as a good practice:

```
git push --force-with-lease
```
