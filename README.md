Exercice1
3)a) The .git folder turns our regular directory into a Git repository. If we delete it, our project becomes just a normal folder again, and all Git history is lost.
6)a) At this point, the file is in the staging area (also called the index).
That means Git is now tracking the file and preparing it to be included in the next commit.
11)a)and b) Once we run git add file1.py, Git moves the changes from the working directory to the staging area (also called the "index").

git diff only shows differences between the working directory and the staging area.

Since our changes are now staged, there is no difference left in the working directory relative to the staging area.

So git diff returns nothing.

To see the changes that are staged (i.e., changes ready to be committed), we can use:
git diff --staged or git diff --cached. This shows differences between the staging area and the last commit.
12)a),b)and c) git status :The first modification we made earlier is already staged.

The new modification we just made is only in the working directory (unstaged).
git diff: This compares working directory ↔ staging area.
git diff --staged : This compares staging area ↔ last commit.
git add file1.py: This updates the staging area to include both sets of changes.
git status: Now everything is staged, no more unstaged changes.
14)a) In small repos, sometimes just 4 characters is enough; in bigger ones we might need more.


ex2 :
1) Current branch : on main branch
6)a) and b) git log now shows only main’s history.

Branch-specific commits are “hidden” until we switch back to my_first_branch.

Files revert to how they were in main.
8)a) b) and c) This means Git just moved main forward to include all commits from my_first_branch — no extra merge commit was needed.
No merge commit was created.

The branch pointer main now includes all commits from my_first_branch.
We’ll see a straight line because the merge was fast-forward — the commits just line up.

No branching divergence is visible in the graph anymore.


Ex 3:
6)a) b) and c) The new line we added in step 5 is gone from the file.

Git restored the file to the committed version, discarding the uncommitted edits.
Yes — besides git checkout -- file3.py, you could have used:

git restore file3.py

8)a) b) and c) | Command                                          | Effect                                                                                                                           |
| ------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------- |
| `git checkout -- <file>` or `git restore <file>` | Discards **unstaged changes** in the working directory; no new commit is created.                                                |
| `git revert <commit>`                            | Creates a **new commit** that undoes the changes introduced by the specified commit. The original commit remains in the history. |

git revert does not remove the commit; it just adds a new commit that reverses its changes.

This shows that file4.py was deleted as part of the revert.

The revert commit undoes the changes without removing the history of the original commit.

10) a) b) c) and d) The last commit (Update both file3.py and file4.py) was removed from the branch history.

The changes themselves weren’t lost — they’re still staged.

git log  now doesn’t show the reset commit, only older commits.
The commit is gone from the branch history.
Option 1: Use git revert instead — it preserves the commit but undoes its changes.

Option 2: Make a backup branch before resetting.
11) a)b) and c) Command	Effect
git reset --soft HEAD^	Moves HEAD back one commit; keeps changes staged.
git reset HEAD	Keeps HEAD where it is; unstages staged changes, leaving them in the working directory.

HEAD^ refers to the commit before HEAD.

HEAD refers to the current commit.

Here, we want to unstage changes from the current commit (or staged files), not move HEAD.
Default scope of git reset:

When you don’t specify --soft, --mixed, or --hard, Git uses --mixed by default.

Meaning:

Index/staging area is reset to match HEAD.

Working directory is left unchanged.

12)a)b)c) Working directory: All changes in our files are discarded. The files are reset to match the commit we reset to.

Staging area (index): Cleared and synced with HEAD.

Commit history:

If we didn’t specify a commit, HEAD stays on the current commit.

Any commits we previously moved HEAD away from (e.g., via --soft HEAD^) are gone from the branch history, unless we recover them using git reflog.


When we don’t specify a commit, Git uses HEAD as the default.

Meaning: our branch pointer stays on the current commit, but everything in staging and working directory is reset to match HEAD.


Step 9 was:

Make a new commit with changes in both file3.py and file4.py.

If we ran git reset --hard immediately after step 9, without specifying a commit:

HEAD stays on the new commit.

The working directory is reset to exactly match that commit.

No effect on the commit itself — the commit still exists.

If we ran git reset --hard HEAD^ after step 9:

HEAD would move back one commit.

The working directory would match that previous commit.

The changes in your “step 9 commit” would be completely lost from both history and working directory (unless recovered via git reflog).


ex 4

1) a) yes
6) a) b) c) and d)
git rebase main takes all commits in feature-branch that are not in main and reapplies them on top of main.

The base branch becomes main.

Effectively, feature-branch’s commits are moved to start after the latest commit in main, creating a linear history.
The commit history of feature-branch has been rewritten so that its commits now follow the latest commit in main.

Any new commits in main are now below feature-branch’s commits.

The history is linear, unlike a merge which can show a branch divergence.
| Aspect         | Merge                                            | Rebase                                                  |
| -------------- | ------------------------------------------------ | ------------------------------------------------------- |
| Commit history | Keeps branch history; creates a **merge commit** | Rewrites branch history; **no merge commit**            |
| Graph          | Shows branching                                  | Linear history                                          |
| Commits        | Both branches visible                            | `feature-branch` commits appear on top of `main`        |
| Safety         | Safe for public branches                         | Avoid rebasing commits that were pushed to shared repos |

