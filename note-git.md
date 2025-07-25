# Note git command 😁

### 🍗 Các lệnh cơ bản
* `git status` Kiểm tra trạng thái các thay đổi
* `git fetch` và `git pull` Cập nhật dữ liệu từ remote 
* `git add .` hoặc `git add <tên_tập_tin>` Thêm vào chỉ mục index hoặc có thể hiểu là báo cho git biết là sẽ quan tâm đến tệp này
* `git commit -m '<message>'` commit file thay đổi đến HEAD lúc này file vẫn còn đang ở local chưa được đưa lên remote
* `git push` hoặc `git push origin <tên_nhánh>` Đưa các thay đổi lên remote
* `git push --force` Đưa các thay đổi lên remote và đè lịch sửa commit trên remote thành lịch sửa commit dưới local, vì đôi lúc commit trên remote không giống với dưới local, nhưng đây chỉ nên dùng trên nhánh cá nhân, nếu ở các nhánh chung như `master` `release` thì không nên vì có thể làm mất commit của các thành viên khác, lúc đó cần các lệnh khác để xử lý.
*   git restore .` hoặc `git restore <tên_tập_tin>` Trở về các thay đổi trước đó, tức là khi code mà không cần thay đổi vừa thêm nữa thì dùng lệnh này để bỏ các thay đổi đó.
* `git reset --soft HEAD@{1}` để quay trở lại trước khi commit --amend 

### 🍔 Các lệnh với nhánh
* `git branch <branch_name>` Tạo nhánh
* `git checkout <branch_name>` Chuyển đổi giữa các nhánh
* `git checkout -B <branch_name>` Lệnh tạo nhánh đồng thời chuyển sang nhánh vừa tạo
EX: `git checkout -B release` sau đó hãy dùng lệnh ```git push``` thì sẽ nhận 1 thông báo lệnh như này `git push --set-upstream origin release` thì copy và chạy để đưa nhánh này lên remote
* `git branch -D <branch_name>` Xóa nhánh dưới local
* `git push origin -delete <remote_branch_name>` Xóa nhánh trên remote `origin` tên của remote
##### Git merge
* `git merge <branch_name>` hợp nhất `<branch_name>` vào nhánh đang đứng hiện tại
* `git diff` xem các nơi bị lỗi conflict
* `--allow-unrelated-histories` kẹp đoạn này sau merge khi có trường hợp không đồng nhất lịch sử commit. <p style="color: red">**Chú ý chỉ dùng trong trường hợp có nhánh lịch sử quá cũ so với nhánh chính và nên tạo 1 nhánh phụ từ nhánh chính rồi merge vào nhánh phụ để hạn chế việc bị mất code**</p> 

Sẽ có trường hợp xảy ra confict thì cần fix conflict và commit lại

##### Git rebase
Ngoài `git merge` ta có thể dùng thêm `git rebase`
Trong lúc đang làm mà nhánh nào đó đang có code mới mà ta cần lấy code đó thì có thể dùng rebase để đồng nhất về commit, lưu lịch sử commit cũng tường minh hơn.
* `git rebase <branch_name>` câu lệnh này sẽ bê các commit ở nhánh `<branch_name>` vào nhánh hện tại. Nó sẽ đi từng commit không giống như merge, merge thì sẽ lấy nhánh mới nhất
* `git rebase --continue` đôi lúc sẽ conflict ở 1 vài nhánh thì khi đó sẽ dừng lại và sửa conflict, sửa xong dùng lệnh `git add` sau đó sẽ dùng lệnh này để tiếp tục rebase
* `git rebase --abort` để hủy bỏ rebase

Sau khi hoàn thành thi dùng push để đưa lên remote

### 🍟 Reset commit hoặc xóa các commit
* `git log` để kiểm tra lịch sử commit ở branch hiện tại
* `git reset HEAD~1` Trở về trước đó 1 branch và các code thay đổi trước đó sẽ quay trở về thành những thay đổi mới `1` có thể thay số vd:`2` thì tức là về trước đó 2 commit
* `git push --force` sẽ có trường hợp khi push nhận thông báo cần pull code lại thì lúc này có thể dùng đến lệnh này để đè lại các commit có trên remote

##### EX: Hiện tại có 3 commit
```
commit 3
commit 2
commit 1
```
Muốn trở về `commit 1` dùng `git reset HEAD~2` sau khi chỉnh sửa thì dùng `git commit -m 'commit 4`, sau đó `git push` thì sẽ nhận lỗi yêu cầu pull, lúc này ta cần dùng `git push --force` để đè lại các commit trên remote
Khi đó commit trên remote sẽ là
```
commit 4
commit 1
```

### 🌭 Chery-pick
Đôi lúc đang làm mà ở nhánh nào đó mà cần release chức năng đó cho nhánh chính mà chức năng đó đang nằm ở 1 commit trong nhánh đang làm, hoặc khi merge mà lịch sử commit không đồng nhất với lịch sử của nhánh cần merge thì ta có thể dùng chery-pick
Lấy 1 hoặc nhiều commit từ nhánh khác vào nhánh hiện tại
```
git chery-pick <id_commit_1> <id_commit_2>
```
Lệnh tiếp tục chery-pick sau khi fix conflict 
```
git chery-pick --continue
``` 
Lệnh hủy bỏ hoặc thoát chery-pick
```
git chery-pick --abort
git chery-pick --quit
```

Cũng tương tự rebase đôi lúc sẽ có conflict thì ta sẽ fix conflict rồi add, sau đó dùng `git chery-pick --continue` xong thì commit và push code lên remote

### 🍘 Git clean
Đôi lúc sẽ có mấy file chạy chạy server sẽ tự sinh ra và mình không muốn commit file đó để tránh conflict khi merge
Trường hợp thực tế là khi mình làm với dự án dùng jquery hoặc js, css thuần, vì khi chạy dự án thì có tools chuyển các file js css thành file min mà khi chuyển file min đổi lúc sinh ra file <tên_file>.min.min tức là nó bị lặp lại cái file min, mà mấy file đó là lỗi tự sinh ra nên đi xóa tay thì lâu vì vậy mình thường dùng câu lệnh dưới
* `git clean -n` Kiểm tra các file sẽ bị xóa
* `git clean -f` Để xóa các file mà câu lệnh vừa báo
* `git clean -d <path_file_name>` Xóa các file được chỉ định
*Nếu trong xóa có file bạn không muốn xóa thì chỉ cần `git add <file>` đó là được

### 🍿 Khi chuyển branch mà không muốn commit
Trong lúc làm việc nhiều lúc cần chuyển sang 1 nhánh khác để fix một lỗi nào đó, nhưng ở nhánh hiện tại đang có thay đổi code, nếu commit rồi checkout thì sẽ sinh ra nhiều commit nên có thể dùng cách dưới.
```
git stash 
```
Thêm stash với message
```
git stash save "message"
```
*Khi dùng lệnh này thì các thay đổi ở branch hiện tại sẽ được đưa vào tạm, lúc này có thể chuyển sang các branch khác, sau khi làm ở các branch khác và trở lại branch cũ để làm tiếp thì dùng lệnh dưới để lấy code trong tạm trước đó ra*
```
git stash pop
```
Có thể dùng lệnh dưới để lấy stash cần chọn
```
git stash pop stash@{1}
```
Để kiểm tra các danh sách tạm
```
git stash list
```
### 🧂 Git remote
Trường hợp cần thay đổi nơi chứa source 
Kiểm tra danh sách remote
```
git remote -v
```
Thay đổi địa chỉ của remote
```
git remote set-url <name> <url>
```
* `<name>` Tên remote hiện tại, thường là origin hoặc dùng lệnh ```git remote -v``` điển kiểm tra
* `<url>` Đường dẫn của remote mới

Thay đổi tên của remote
```
git remote rename <old_name> <new-name>
``` 
### 🍖 Xóa tất cả branch dưới local
Khi đổi remote thì cần xóa hết nhánh local của remote của rồi fetch lại nhánh của remote mới
```
git branch | grep -v "master" | xargs git branch -D
```
* ```git branch``` Lấy tất cả branch
* ```grep -v "master"``` Chọn tất cả branch trừ nhánh master
* ```xargs git branch -D``` Xóa các branch đã chọn
### 🥩 Config git
#### Config ssl
Mở file config trong src
```
 vi .git/config
```
muốn thoát thì dùng 
* `:wq`thoát và lưu
* `:q` thoát không lưu
Tắt ssl cho git, khi nào ssh lỗi hoặc không có https, nên cài cho 1 project nhất định không nên cài global
```
git config http.sslVerify false
git config --global http.sslVerify false
```

### 🍙 Fix merge mà không muốn thêm commit
```
reset --soft HEAD~1
git commit -c ORIG_HEAD
git push --force-with-lease
```
(Chỉ dùng trên nhánh riêng, không dùng ở nhánh chung)

### ✨ Git ignore không hoạt động
```
git rm -rf --cached .
git add .
git rm --cache <ten_file>
```
### 🎃 Tạo file lưu các thay đổi commit
Lệnh tạo file
```
git diff > <ten_file>
```
Khi đưa cho người khác thì copy file đó vào root project rồi chạy lệnh
```
git apply <ten_file>
```
Drop commit
```
git rebase -i HEAD~N (n: commit length)
```
### Multiple ssh key
[https://www.freecodecamp.org/news/how-to-manage-multiple-ssh-keys/]
