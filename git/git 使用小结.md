先配置好SSH秘钥，不会的看文末链接 1
# 一、下载仓库
以我的仓库为例
```
git clone https://github.com/JavanCheng/Learning-Summary.git
```

# 二、提交修改

```
# 链接到远程仓库
git remote add origin git@github.com:JavanCheng/Learning-Summary.git
# 将修改的内容放入缓存
git add .
git commit -m "first commit"
# 推送修改（目前所有的主分支默认命名是 main ，使用 master 会报错）
git push origin main
```
# 三、更新远程仓库的更改

```
git pull origin master
```

# 四、bug小结
### 1. Failed to connect to github.com port 443: Timed out
参考链接：[https://blog.csdn.net/qq_42102911/article/details/121485672](https://blog.csdn.net/qq_42102911/article/details/121485672)
==将github地址加入hosts的方法==
### 2.error: src refspec xxx does not match any / error: failed to push some refs to
参考链接：[https://blog.csdn.net/u014361280/article/details/109703556](https://blog.csdn.net/u014361280/article/details/109703556)
==重新绑定远程仓库==
### 3.执行git push出现"Everything up-to-date"
参考链接：[https://cloud.tencent.com/developer/article/1027035](https://cloud.tencent.com/developer/article/1027035)
==add 和 commit 失败，重新 add 和 commit==
### 4.执行 git push 时出现 Connection reset by 140.82.112.4 port 22 fatal: Could not read from remote repository
参考链接：[https://blog.csdn.net/qq_33521184/article/details/103767426](https://blog.csdn.net/qq_33521184/article/details/103767426)
==大概率网不好，我换了手机热点就好了，== 也可以先 ping 一下试试（我试了没啥用）
### 5.git下载出现 Failed to connect to 127.0.0.1 port 1080: Connection refused
参考链接：[https://blog.csdn.net/weixin_41010198/article/details/87929622](https://blog.csdn.net/weixin_41010198/article/details/87929622)
之前看教程用了奇怪的端口，下载一直报错，用这个教程取消之前设置的端口（貌似需要把端口设置成 VPN 的端口才行）
教程链接如下：[https://www.cnblogs.com/etangyushan/p/8992926.html](https://www.cnblogs.com/etangyushan/p/8992926.html)
# 五、学习推荐
# 1.[如何将本地仓库推送到 github](https://www.cnblogs.com/xiaogongjin/p/11878073.html)
# 2.[菜鸟教程 git](https://www.runoob.com/git/git-tutorial.html)
# 3.[git 中文文档](https://www.git-scm.com/book/zh/v2)
