# shell script(bash) cheat sheet

ref: [basic tutorial](https://www.runoob.com/linux/linux-filesystem.html)

## grep

可以從串流資料或檔案中，使用關鍵字或正規表示法（regular expression）篩選出想要尋找的資料，並且顯示出來
[ref]( https://blog.gtwang.org/linux/linux-grep-command-tutorial-examples/)

---

## check process

---

```bash
sudo lsof -i :port 			# 看port
top							# 看執行中的程式
pwd							# 看home address
nmap -p port localhost 		# check port			
lsof -i -n					# check all ports
lsof -t -i :1521			# check specfic port
kill -9 PID 				# kill process
whereis						#locates the source, binary, and manuals sections for specified files
```

### lsof (t:port, )

list open files - 在linux環境下，任何事物都以檔的形式存在，通過檔不僅僅可以訪問常規資料，還可以訪問網路連接和硬體(每個通訊埠都會與主機的IP位址及通訊協定关联。 通訊埠以16位元數字來表示，這被稱為通訊埠編號（port number = 16bit）。)

```bash

lsof -i -n					# check all ports
lsof -t -i :1521			# check specfic port
```

### ps

---

process status）命令用于显示当前进程的状态

```bash
ps -ef              #看所有運行中的processes
ps -A 				#看所有processes
ps -au 				#detail
ps ax | grep -i "node\|pm"      #抓出特定關鍵字的process
ps ax | grep -i "c2"
ps ax | grep -i "clipu"
ps ax | grep -i "chrome"
```

---

## ls

```bash
ls                  # list files in the foldfer
ls -lrt             # list file sorted by time
ls -l               # list files with sizes, owner, permission, last updated
```

---

## check content of a file

```bash
cat file            # 全部內容, 如果不指定file, 則會接收你接下來input的value line by line

tail -f file        # means following, 會一直更新最新內容
head file           # 從最上端開始讀
stat file           # 看file metadata
```

## chmod

change mode - 用於控制使用者對檔案的權限的命令和函式

check auth of all files in the folder
auth of file system format is 10 bit: trwxrwxrwx

	t代表file type = - or d
	rwx分別代表 read, write, execute. ex:-rwxrwx---

- 0bit: file type-
- 1~3bit: u (for owner)
- 4~6bit: g (for group)
- 7~9bit: o (for others)

```bash
chmod ugo+/-rwx file
chmod go-rwx file
chmod +x file  - is equal to chmod ugo+x (Based on umask value)
chmod 755 file  - is equal to chmod u=rwx,go=rx
chmod u=rx file        (Give the owner rx permissions, not w)
chmod go-rwx file      (Deny rwx permission for group, others)
chmod g+w file         (Give write permission to the group)
chmod a+x file1 file2  (Give execute permission to everybody)
chmod g+rx,o+x file    (OK to combine like this with a comma)
```

---

## check status of disk

常用三个命令 df、du 和 fdisk。

- df（英文全称：disk free）：列出文件系统的整体磁盘使用量
- du（英文全称：disk used）：检查磁盘空间使用量, 与 df 命令不同的是 Linux du 命令是对文件和目录磁盘使用的空间的查看
- fdisk：用于磁盘分区


```bash
df -H               # check disk usage 
```

---

## 紀錄查看

```bash
history	# 看command紀錄
last - number # 看login紀錄
lscpu
```

---

## ssh 相關

```bash
ssh url -l username 							# 登入 by username
scp account@url:datapath /Users/account/path 	# 從remote複製到本機
scp /pathaddress account@url:pathaddress 		# 本機複製到remote

ssh-keygen -t rsa -C your "email address".      # create key pairs with comment 

```

---

## network

	ifconfig 看自身網路的各種資訊(include mac address, ipv4 address,  ipv6 address, 還有其他pc的ip)

	ifconfig en0 up
	ifconfig en0 down
	traceroute url 檢視封包傳遞過程
	ping url 測試連線結果
	telnet <host> <port>    telnet就是檢視某個埠是否可訪問

---

## monitor

	uptime -  prints the current time, the length of time the system has been up, the number of users online, and the load average. The load average is the number of runnable processes over the preceding 1-, 5-, 15-minute intervals.

	top - displays the processor activity of your Linux box and also displays tasks managed by the kernel in real-time
	
## compression

	you need to download app firstly

	tar cvf FileName.tar DirName
	tar xvf FileName.tar
	gzip FileName
	gunzip FileName.gz
	tar zcvf FileName.tar.gz DirName
	tar zxvf FileName.tar.gz

---

## ab(Apache Benchmark) 

Apache HTTP Server的壓力測試工具

	-n [requests]  :request 數量
	-c [cuncurrency] :併發數(同時有多少request)
	-t [timelimit] :響應等待時間

example1: 對github發送 1000 requests 每次同時100個

	ab -n 1000 -c 100 https://github.com/

	Connection Times (ms)
				min  mean[+/-sd] median   max
	Connect:      156  761 417.4    639    3449
	Processing:    75 1159 1351.9    241    5988
	Waiting:       28  259 250.8    182    3023
	Total:        391 1920 1416.1   1369    6872

example2: 對gitlab發送 1000 requests 每次同時100個

	ab -n 1000 -c 100 https://about.gitlab.com/

	Connection Times (ms)
				min  mean[+/-sd] median   max
	Connect:      102 3018 2311.9   1951   19917
	Processing:   376 3865 3077.6   2968   31797
	Waiting:       14  790 661.1    596    6895
	Total:       1419 6884 4634.0   5299   39784

## network

- [tools for terminal to monitor network](https://askubuntu.com/questions/257263/how-to-display-network-traffic-in-the-terminal)