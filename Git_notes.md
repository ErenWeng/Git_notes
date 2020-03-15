# Git_notes
>人生不能重來，但Git可以

## 安裝環境

傳送門：[git官網](https://git-scm.com/)

先到git的官方網站下載安裝，下載前會自動判斷電腦適合的版本，安裝完後開啟Git Bash應用程式（模擬Linux中的Bash）

如果喜歡圖形化介面工具（GUI），Git官網上會有一些推薦，本人則是使用SourceTree

## Vim 編輯器

由於 Vim 是 Git 的預設編輯器，不少人在使用 Git 的過程中時常意外進入 Vim 編輯器，以下使用方式：

1. 進入之後會直接到 Normal 模式，按下`:w`會進行存檔，按下`:q`會關閉這個檔案，而`:wq`則是存檔完成後直接關閉這個檔案。

2. 在 Normal 模式，又稱命令模式，在這個模式下無法輸入文字，僅能進行複製、貼上、存檔或離開。

3. 要開始輸入文字，需要先按下 `i`、`a` 或`o` 這三個鍵其中一個進入 Insert 模式，便能開始打字。其中，i 表示 insert，a 表示 append，而 o 則是表示會新增一行並開始輸入。

4. 在 Insert 模式下，按下`ESC`鍵或是`Ctrl + [`組合鍵，可退回至 Normal 模式。

## Git 常用指令

### 環境

- `which git` git當前位置
- `git --version` git的版本

### 設定

- `git config --global user.name "newbie"` 設定姓名
- `git config --global user.email "newbie@email.com"` 設定eamil
- `git config -l` 查看設定（list）
- `git config --global core.editor vim` 預設編輯器為vim

> --global參數可以讓所有的git project都採用這個預設值

### git 版控

- 版控流程
  1. `git init` 初始化現在位置的資料夾並且讓git來控制版本
  2. `git add`
      - `git add <file>` 將修改的檔案由工作目錄Working Directory/Tree推送至暫存區域Staging Area，向git說明即將要提交的檔案
      - `git add .` 所有檔案推送至暫存
      - `git rm <file>` 向git說明檔案將要移除，相當於`git add`
      - `git add -u` 加入所有被修改過的檔案（包括刪除）
  3. `git commit -m "<message>"` 將修改由暫存區愈推送至本機儲存庫，並且會新增一個commit將Head貼紙一併移動至此（沒加訊息會跑到vim編輯器，可輸入`:q`離開）
  4. `git push` 將修改送到遠端儲存庫

- 脫離版控
  - `git rm <file> --cached` 單個檔案脫離為Untracked
  - `rm -rf .git` 刪除所有git版控

### git status 檔案狀態建議

- Untracked files（未被追蹤的檔案）
  - 使用`git add`推進暫存

- Changes not staged for commit（被更動但尚未要提交的檔案）
  - 使用`git add`暫存
  - 使用`git checkout <file>`回到修改前的樣子

- Changes to be committed（將要提交的檔案，已add進暫存）
  - 使用`git reset Head <file>`將檔案還原為尚未add的狀態

### 狀態、檢查

- `git status` 查詢現在這個目錄的「狀態」
- `git show` 查詢最後一次commit的修改內容
- `git show <commit>` 查詢某次commit的修改內容
- `git log` 查詢commit的歷史紀錄(時間、作者、做了什麼、貼紙位置、commit)
- `git log <file>` 只顯示此檔案之紀錄
- `git log -p <file>` 只顯示此檔案的修改紀錄(+是新增 -是刪除)
- `git log --name-status` 顯示檔案新增、更動、刪除等資訊情形
- `git log --stat` 顯示更動檔案的統計及摘要
- `git log --oneline` 只保留(貼紙、commit、做了什麼)
- `git log --oneline --author="a"` 只找a作者的commit
- `git log --oneline --grep="WTF"` 只找commit訊息留有WTF的
- `git log --oneline --since="10am" --until="5pm" --after="2020-03"` 找到從2020年3月之後，每天早上10點到下午5點的 Commit
- `git log --oneline --graph` 可看到分支圖形
- `git log --oneline --graph --decorate --all` 可看到更美的分支圖形
- `git blame -L 1,10 <file>` 檢查此檔案中第1~10行的作者與時間
- `git diff <file>` 比較檔案最後一次commit的到現在已修改的工作目錄和索引
- `git diff <commit_or_branch> <commit_or_branch>` 比對兩版本
- `git diff -–name-only <commit_or_branch> <commit_or_branch>` 只想檢視此作者兩個版本的修改差別

### 還原

- 把刪掉的檔案救回 （可先用git status看一下那些被刪除，因為如果目錄也被刪掉就沒救了）
  - `git checkout .` 救回所有檔案
  - `git checkout <file>` 救回某檔案

>如果有做過修改沒commit提交，checkout會放棄此次工作目錄的修改並還原成上次提交版本，並從暫存區退回工作區域（此次修改會被覆蓋小心使用）

- git checkout的還原
  - `git checkout HEAD~2` 退回兩個版本前

- git reset的還原

  - `git reset head` 還原成前次版本
  - `git reset head <file>` 只還原某個檔案成前次版本
  - `git reset <commit>` 回到某次commit的版本
  - `git reset <commit_or_branch>^` 退回此commit/貼紙1次前的版本
  - `git reset <commit_or_branch>^^` 退回此commit/貼紙2次前的版本
  - `git reset <commit_or_branch>~x` 退回此commit/貼紙x次前的版本
  
  reset參數
  - `git reset --soft <commit_or_branch>` 只移動Head和branch，由儲存庫丟回暫存區，可用於更改commit訊息（重新commit不需要再add一次）
  - `git reset --mixed <commit_or_branch>` 預設，移動HEAD與其Branch，將暫存區的內容移出
  - `git reset --hard <commit_or_branch>` 直接移除暫存及工作目錄所有內容(但並沒有真的消失)

- git revert的還原
  - `git revert HEAD --no-edit` 新增一個commit來原地覆蓋上一個不想要的commit
  - `git revert <commit>` 新增一個和此commit相反的修改

- 看不見檔案且commit也忘了
  - `git reflog`來查看head的移動紀錄（每次head移動都會再reflog記上一筆commit，保留30天），就會看到commit，再用`git reset <commit> --hard`救回

  - 會使head發生改變的指令
    - `git commit` head移動到新的commit
    - `git reset --hard <commit>` head換到新的commit
    - `git checkout <branch_or_commit>` head切換到某貼紙或分支
    - `git merge/rebase` head合併分支
    - `git cherry-pick/revert` 挑入/挑出commit時

- log 和 reflog的差別

  - reflog可以看到所有分支的操作紀錄
  - log看不到已經刪除的commit

- reset 和 checkout的差別

  - checkout是退回某版本，只會移動head，分支不會跟著移動，常用於切換分支
  - reset：移動head和其branch，會移出commit，除了動到head還會移動branch，如果分支沒有人管理就會形同隱形

### 修改

- --amend

  - `git commit --amend -m "add cat"` 將最後一次commit訊息改為add cat

  - 將Untracked的檔案併入已發出的commit (檔案忘記一起commit又不想再commit一次)
  - `git add <file>` 先推至暫存，`git commit --amend --no-edit` 再推入最後一次的commit並且不加說明文字

>每次的--amend都會被視為更改，而產生新的commit，但實際上commit並沒有增加

- cherry-pick
  
  - 使用`git reset --hard <commit>`回到過去的某版本，再使用`git cherry-pick <commit>`把後面的版本依自己想要的順序一個一個加進來就可以調換順序，當然沒加的就不會顯示（形同刪除紀錄）

### 分支

- 查看、移動
  - `git branch` 查看分支及head的位置
  - `git checkout <branch_or_commit>` 將head切換到某分支/commit
  - `git branch -f <branch> <commit>` 移動某分支到某commit（如果此分支不存在就會建立新的）

- 新增分支
  - `git branch <branch>` 新增分支
  - `git branch <branch> <commit>` 在某commit加上分支
  - `git checkout -b <branch>` 新增分支並且直接切換到新分支
  - `git branch -m <old_branch_name> <new_branch_name>` 改變分支名稱

- 刪除分支
  - `git branch -d <branch>` 可能會顯示你還沒合併的訊息
  - `git branch -D <branch>` 強制刪除

- 合併分支 (被合併貼紙的都是在新commit的下面)
  - `git merge <branch>` 合併分支
  - `git merge <branch> --no-ff` 保留分支原樣(不使用快轉模式Fast Forward合併)
  - `git rebase <branch>` 將當前分支接到某分支之後，並且會被重新分配一個commit
  - `git rebase <branch_1> <branch_2>` 將分支2接到分之1支後
  
[Git遊戲：learngitbranching](https://learngitbranching.js.org/)
