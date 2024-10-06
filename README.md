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
git@github.com:sak-id/skills-introduction-to-github.git

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