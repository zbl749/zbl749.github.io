# Git 使用手册

## 一、基础概念

- **工作区(Workspace)**: 你本地看到的项目文件
- **暂存区(Index/Stage)**: 临时存放改动,`.git/index`文件
- **仓库(Repository)**: 提交历史,`.git`目录
- **HEAD**: 指向当前分支的最新提交

## 二、初始配置

```bash
# 设置用户信息(全局)
git config --global user.name "你的名字"
git config --global user.email "your.email@example.com"

# 设置文本编辑器
git config --global core.editor "vim"  # 或 "code --wait"

# 查看配置
git config --list

# 创建别名
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
```

## 三、基础操作

### 1. 初始化与克隆

```bash
# 初始化新仓库
git init

# 克隆远程仓库
git clone https://github.com/user/repo.git
git clone https://github.com/user/repo.git my-folder  # 指定目录名
```

### 2. 基本工作流程

```bash
# 查看状态
git status
git status -s  # 简洁模式

# 添加文件到暂存区
git add file.txt           # 指定文件
git add .                  # 当前目录所有改动
git add -A                 # 所有改动(包括删除)

# 提交改动
git commit -m "提交信息"
git commit -m "标题" -m "详细描述"  # 多行信息
git commit -a -m "信息"   # 跳过add,自动添加已跟踪文件的改动

# 查看提交历史
git log
git log --oneline         # 简洁一行
git log --graph --oneline --all --decorate  # 图形化显示
```

### 3. 查看差异

```bash
git diff                  # 工作区 vs 暂存区
git diff --cached         # 暂存区 vs 最新提交
git diff HEAD             # 工作区 vs 最新提交
git diff branch1 branch2  # 比较两个分支
```

## 四、分支管理

### 1. 基本操作

```bash
# 查看分支
git branch                  # 本地分支
git branch -a               # 所有分支(包括远程)
git branch -r               # 仅远程分支

# 创建与切换
git branch feature          # 创建分支
git checkout feature        # 切换分支
git checkout -b feature     # 创建并切换

# 重命名分支
git branch -m old-name new-name

# 删除分支
git branch -d feature       # 安全删除(已合并)
git branch -D feature       # 强制删除
```

### 2. 合并与变基

```bash
# 合并分支到当前分支
git merge feature           # 会保留合并历史
git merge --no-ff feature   # 强制创建合并提交

# 变基(保持线性历史)
git rebase main             # 将当前分支变基到main

# 解决冲突
# 1. 手动编辑冲突文件
# 2. git add .
# 3. git commit 或 git rebase --continue
```

### 3. 远程分支

```bash
# 推送分支
git push origin feature
git push -u origin feature  # 设置上游分支,后续可直接git push

# 拉取远程分支
git fetch origin
git checkout -b feature origin/feature

# 删除远程分支
git push origin --delete feature
git push origin :feature    # 旧语法
```

## 五、远程操作

```bash
# 查看远程仓库
git remote -v

# 添加远程仓库
git remote add upstream https://github.com/original/repo.git

# 获取远程更新
git fetch origin          # 获取但不合并
git pull origin main      # 获取并合并(= fetch + merge)
git pull --rebase origin main  # 获取并变基(推荐)

# 推送更新
git push origin main
git push -f origin main   # 强制推送(谨慎使用!)
```

## 六、撤销与回退

```bash
# 撤销工作区改动
git checkout -- file.txt  # 恢复文件到暂存区状态
git restore file.txt      # Git 2.23+ 新命令

# 撤销暂存区改动
git reset HEAD file.txt   # 取消add
git restore --staged file.txt  # Git 2.23+

# 修改最后一次提交
git commit --amend -m "新信息"
git commit --amend --no-edit  # 不改信息,只添加改动

# 回退到指定提交
git reset --soft HEAD~1   # 只移动HEAD,保留改动在暂存区
git reset --mixed HEAD~1  # 移动HEAD,改动在工作区(默认)
git reset --hard HEAD~1   # 彻底回退,丢弃改动

# 查看操作记录
git reflog                # 查看所有HEAD变动
```

## 七、标签管理

```bash
# 创建标签
git tag v1.0.0            # 轻量标签
git tag -a v1.0.0 -m "版本说明"  # 附注标签
git tag v1.0.0 commit-id  # 对指定提交打标签

# 查看标签
git tag
git show v1.0.0

# 推送标签
git push origin v1.0.0
git push origin --tags    # 推送所有标签

# 删除标签
git tag -d v1.0.0
git push origin --delete v1.0.0  # 删除远程标签
```

## 八、储藏(Stash)

```bash
# 保存当前改动
git stash
git stash save "说明"

# 查看储藏列表
git stash list

# 应用储藏
git stash apply           # 应用最新储藏
git stash apply stash@{1} # 应用指定储藏
git stash pop             # 应用并删除最新储藏

# 删除储藏
git stash drop stash@{1}
git stash clear           # 清空所有储藏
```

## 九、实用技巧

### 1. 交互式操作

```bash
git add -i              # 交互式添加
git rebase -i HEAD~3    # 交互式变基(合并、修改、删除提交)
```

### 2. 搜索与查找

```bash
git blame file.txt      # 查看每行代码最后修改者
git grep "关键字"        # 在版本库中搜索
git log -S "function"   # 搜索提交中新增/删除某字符串的历史
```

### 3. 子模块

```bash
git submodule add https://github.com/user/repo.git path/to/submodule
git submodule update --init --recursive
```

### 4. 大文件管理(LFS)

```bash
git lfs install
git lfs track "*.psd"
git add .gitattributes
```

## 十、推荐工作流

### 1. 功能分支工作流

```bash
# 开始新功能
git checkout -b feature/user-auth

# 开发完成后
git add .
git commit -m "feat: 用户认证功能"
git push origin feature/user-auth

# 创建Pull Request合并到main
```

### 2. Git Flow工作流

```
main (保护分支)  ────────────────────────────────────►
                \                                 /
develop  ────────►───►───────────────────────────►
          \      /    \                         /
feature-a  ─►───►      \                       /
                        \                     /
feature-b                ─►───►──────────────►
```

### 3. Fork工作流(开源贡献)

```bash
# 1. Fork仓库到个人账号
# 2. 克隆个人仓库
git clone https://github.com/your/repo.git

# 3. 添加上游仓库
git remote add upstream https://github.com/original/repo.git

# 4. 创建功能分支开发
git checkout -b fix/bug-123

# 5. 定期同步上游
git fetch upstream
git rebase upstream/main

# 6. 推送到个人仓库
git push origin fix/bug-123

# 7. 创建Pull Request
```

## 十一、常见问题解决

### 1. 误提交大文件

```bash
git reset --soft HEAD~1
echo "large-file.zip" >> .gitignore
git rm --cached large-file.zip
git commit -m "重新提交"
```

### 2. 合并冲突

```bash
# 查看冲突文件
git status

# 手动解决冲突后
git add .
git commit  # 或使用git rebase --continue
```

### 3. 修改历史提交

```bash
# 修改最近n个提交
git rebase -i HEAD~n

# 停止在需要修改的提交(将pick改为edit)
# 修改后
git commit --amend
git rebase --continue
```

### 4. 恢复被删除的分支

```bash
git reflog
git checkout -b branch-name commit-id
```

## 十二、最佳实践

1. **提交信息规范**: 使用清晰、简洁的提交信息,推荐[Conventional Commits](https://www.conventionalcommits.org/)
   - `feat:` 新功能
   - `fix:` 修复bug
   - `docs:` 文档
   - `style:` 格式
   - `refactor:` 重构
   - `test:` 测试
   - `chore:` 杂项

2. **频繁提交**: 小而频繁的提交比大提交更容易管理

3. **及时推送**: 定期推送本地提交到远程

4. **分支保护**: 对main/master分支设置保护规则,禁止直接push

5. **代码审查**: 使用Pull Request进行代码审查

6. **保持同步**: 定期pull或rebase上游更新,减少冲突

7. **清理历史**: 合并功能分支前使用rebase整理提交历史

---

**提示**: 记不住命令时,使用 `git help <command>` 或 `git <command> --help` 查看详细文档。例如:`git help rebase`。
