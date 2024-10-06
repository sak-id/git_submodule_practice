## git submodule practice


git submoduleの理解のためのリポジトリ

## 行った変更
#### 初期設定

```
git init
git add .
git commit -m "first commit"
git push -u origin main
```

#### git submodule
```
# submoduleの追加。.gitmodulesが作成される
git submodule add git@github.com:sak-id/skills-introduction-to-github.git

% git diff --cached skills-introduction-to-github
diff --git a/skills-introduction-to-github b/skills-introduction-to-github
new file mode 160000
index 0000000..85167ce
--- /dev/null
+++ b/skills-introduction-to-github
@@ -0,0 +1 @@
+Subproject commit 85167ced12b4ad9aac66320cc6e65093022b4870

% git diff --cached --submodule 
diff --git a/.gitmodules b/.gitmodules
new file mode 100644
index 0000000..74bb0c9
--- /dev/null
+++ b/.gitmodules
@@ -0,0 +1,3 @@
+[submodule "skills-introduction-to-github"]
+       path = skills-introduction-to-github
+       url = git@github.com:sak-id/skills-introduction-to-github.git
Submodule skills-introduction-to-github 0000000...85167ce (new submodule)

% git commit -am "add submodule normally"
[main 52874a3] add submodule normally
 3 files changed, 45 insertions(+), 1 deletion(-)
 create mode 100644 .gitmodules
 create mode 160000 skills-introduction-to-github


git push
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 8 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 908 bytes | 908.00 KiB/s, done.
Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:sak-id/git_submodule_practice.git
   0aa15cd..52874a3  main -> main
```

#### git subtree
```
% git remote add audio git@github.com:sak-id/audio-fromexamples.git
% git remote -v
audio   git@github.com:sak-id/audio-fromexamples.git (fetch)
audio   git@github.com:sak-id/audio-fromexamples.git (push)
origin  git@github.com:sak-id/git_submodule_practice.git (fetch)
origin  git@github.com:sak-id/git_submodule_practice.git (push)
% git remote
audio
origin

% git subtree add --prefix=subtree_test --squash audio main
# 1回目はコミットしていない変化があったため失敗
fatal: working tree has modifications.  Cannot add.
# add/commitしてから再実行
git fetch audio main
remote: Enumerating objects: 23, done.
remote: Counting objects: 100% (23/23), done.
remote: Compressing objects: 100% (13/13), done.
remote: Total 23 (delta 8), reused 21 (delta 7), pack-reused 0 (from 0)
Unpacking objects: 100% (23/23), 8.69 KiB | 404.00 KiB/s, done.
From github.com:sak-id/audio-fromexamples
 * branch            main       -> FETCH_HEAD
 * [new branch]      main       -> audio/main
Added dir 'subtree_test'
```

#### git subtreeの中身を変更
subtree内の[README.md](subtree_test/README.md)に変更を加え、全体のリポジトリでpush
```
% git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   subtree_test/README.md

no changes added to commit (use "git add" and/or "git commit -a")
% git commit -am "modification inside subtree"
[main 24327e0] modification inside subtree
 1 file changed, 3 insertions(+), 1 deletion(-)
% git push
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 8 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 374 bytes | 374.00 KiB/s, done.
Total 4 (delta 3), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (3/3), completed with 3 local objects.
To github.com:sak-id/git_submodule_practice.git
   633296f..24327e0  main -> main
```
このリポジトリ内は変更され、元のリポジトリは変更されないことを確認

#### git submoduleの削除
https://zenn.dev/mtmatma/articles/6c2c8be8f8a051 を参考にした
```
% git submodule
 85167ced12b4ad9aac66320cc6e65093022b4870 skills-introduction-to-github (heads/main)

% git submodule deinit -f 
skills-introduction-to-github 
Cleared directory 'skills-introduction-to-github'
Submodule 'skills-introduction-to-github' (git@github.com:sak-id/skills-introduction-to-github.git) unregistered for path 'skills-introduction-to-github'

% git rm skills-introduction-to-github
rm 'skills-introduction-to-github'
```

これで完全にsubmoduleが消える

#### git submoduleの中身を別ディレクトリに移す
submodule内で変更してしまった際、deinitだと変更ごと消えてしまい困った
```
git submodule add git@github.com:sak-id/skills-introduction-to-github.git
fatal: A git directory for 'skills-introduction-to-github' is found locally with remote(s):
  origin        git@github.com:sak-id/skills-introduction-to-github.git
If you want to reuse this local git directory instead of cloning again from
  git@github.com:sak-id/skills-introduction-to-github.git
use the '--force' option. If the local git directory is not the correct repo
or you are unsure what this means choose another name with the '--name' option.

% git submodule add git@github.com:sak-id/skills-introduction-to-github.git submodule
Cloning into '/Users/sakiieda/Documents/progs/git_submodule_practice/submodule'...
remote: Enumerating objects: 84, done.
remote: Counting objects: 100% (84/84), done.
remote: Compressing objects: 100% (66/66), done.
remote: Total 84 (delta 25), reused 53 (delta 10), pack-reused 0 (from 0)
Receiving objects: 100% (84/84), 831.15 KiB | 1.20 MiB/s, done.
Resolving deltas: 100% (25/25), done.

## 新たなファイルをsubmodule内で新規作成した

cp -r submodule/* submodule_deplicate

% git add .
% git commit -m "copy files inside subm
odule"
[main ddc6fd5] copy files inside submodule
 21 files changed, 101 insertions(+), 2 deletions(-)
 create mode 160000 submodule
 create mode 100644 submodule_deplicate/LICENSE
 create mode 100644 submodule_deplicate/Profile.md
 create mode 100644 submodule_deplicate/README.md
 create mode 100644 submodule_deplicate/add_extra_file.txt
 create mode 100644 submodule_deplicate/images/Actions-to-step-4.png
 create mode 100644 submodule_deplicate/images/Green-merge-pull-request.png
 create mode 100644 submodule_deplicate/images/Pull-request-description.png
 create mode 100644 submodule_deplicate/images/code-tab.png
 create mode 100644 submodule_deplicate/images/commit-full-screen.png
 create mode 100644 submodule_deplicate/images/compare-and-pull-request.png
 create mode 100644 submodule_deplicate/images/create-branch-button.png
 create mode 100644 submodule_deplicate/images/create-new-file.png
 create mode 100644 submodule_deplicate/images/create-new-repository.png
 create mode 100644 submodule_deplicate/images/delete-branch.png
 create mode 100644 submodule_deplicate/images/main-branch-dropdown.png
 create mode 100644 submodule_deplicate/images/my-profile-file.png
 create mode 100644 submodule_deplicate/images/profile-readme-example.png
 create mode 100644 submodule_deplicate/images/pull-request-branches.png

% git submodule deinit -f submodule
Cleared directory 'submodule'
Submodule 'submodule' (git@github.com:sak-id/skills-introduction-to-github.git) unregistered for path 'submodule'

% git rm submodule
rm 'submodule'

% git commit -am "submodule deinit"
[main 860e43d] submodule deinit
 2 files changed, 4 deletions(-)
 delete mode 160000 submodule
```
こうすることで、誤ってsubmodule内で変更してしまったファイルをそのまま持ってくることができる