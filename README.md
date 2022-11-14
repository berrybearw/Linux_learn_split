# Linux_learn_split
split 切割檔案介紹

* 參考

1. https://blog.gtwang.org/linux/tar-command-examples-in-linux-1/

2. https://blog.gtwang.org/linux/split-large-tar-into-multiple-files-of-certain-size/

3. https://www.jinnsblog.com/2018/03/linux-tar-and-split-cat-example.html

---
分割檔案 (範例:檔案分成最大 200M)
```
split -b 200M ubuntu.iso "ubuntu.iso.part"
```

像是 1 G 大小的檔案會產生 5 個 檔案 ( 原檔案不刪除 )

這樣就會占用 2 G ( 原檔案 + 分割後的檔案 )

依照大小分割
---
    
    ## **使用 split 分割檔案**
    
    如果要將一個大檔案分割成許多個小檔案，可以使用 `split` 配合 `-b` 參數指定每個小檔案的大小，並指定輸出檔名的開頭名稱：
    
```
split -b 200M ubuntu.iso "ubuntu.iso.part"
```
    
    預設的輸出檔案名稱會自動加上英文字母來區隔順序：
    
   ![image](https://user-images.githubusercontent.com/96226780/201688662-22f584f9-593b-4f95-b241-37605a5f9998.png)

    
    亦可以使用管線（pipe）結合其它的 Linux 指令，將資料直接分割後再儲存：
    
```
tar zcf - datafolder | split -b 200M - "datafolder.part"
```

依照檔案個數分割
---

如果想要將檔案依據大小均分為固定個數的檔案，可以使用 `-n` 參數，並指定要分成擠的小檔案，例如若要將 `ubuntu.iso` 這個檔案均分為 `4` 個小檔案，則執行：

```
split -n 4 ubuntu.iso "ubuntu.iso.part"
```

這樣每個分割出來的檔案其大小都會是相同的：

![image](https://user-images.githubusercontent.com/96226780/201688865-1af4e779-01f5-44b1-9b37-a4c50ce0dc82.png)

依照文字檔行數均等分割
---
    
## **以行數分割檔案**
    
`split` 除了以固定的檔案大小切割檔案之外，對於文字檔也可以使用固定行數的方式來分割檔案，這裡我產生一個文字檔，然後將這個文字檔每三行儲存為一個小檔案：
    
```
ls -l / > mydata.txt
split -l 3 mydata.txt mydata.part
```
    
分割出來的檔案中，每個檔案都只有三行文字：

![image](https://user-images.githubusercontent.com/96226780/201689027-82b4ef94-9c8a-4de2-94e5-cca64a8c9c85.png)

**`split` 以固定行數分割檔案**

如果要將文字檔案均分為大小相同的小檔案，但不想要把完整的行切斷，可以使用 `-n l/N` 參數，其中 `N` 是分割檔案數，例如：

```
ls -l / > mydata.txt
split -n l/3 mydata.txt mydata.part
```

這樣分割出來的檔案就不會有將一行資料切成兩行的問題：

![image](https://user-images.githubusercontent.com/96226780/201689091-602d85ff-c8c5-4800-b135-1bc73a5a2232.png)

合併
---

## **使用 cat 合併檔案**

使用 `split` 分割之後的檔案，可以使用 `cat` 來合併，例如：

```
cat datafolder.part* > datafolder.tar.gz
```

或是直接配合管線解壓縮：

```
cat datafolder.part* | tar zxvf -
```
