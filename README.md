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
```

```
% git diff --cached skills-introduction-to-github
diff --git a/skills-introduction-to-github b/skills-introduction-to-github
new file mode 160000
index 0000000..85167ce
--- /dev/null
+++ b/skills-introduction-to-github
@@ -0,0 +1 @@
+Subproject commit 85167ced12b4ad9aac66320cc6e65093022b4870
```

```
% git diff --cached --subm
odule 
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
```