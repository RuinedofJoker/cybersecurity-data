driftnet是一款简单而使用的图片捕获工具，可以很方便的在网络数据包中抓取图片，其使用语法



```bash
driftnet [options] [filter code]

参数说明：

　　　　　　-i　　设置要监听的接口
　　　　　　
　　　　　　-d　　指定保存图片的路径

　　　　　　-a　　附加模式：捕获图片并保存到临时目录；不显示捕获图片窗口（不会显示在屏幕上）

　　　　　　-b	捕获到新的图片时发出嘟嘟声
		  -i	interface 选择监听接口
		  -f	file 读取一个指定pcap数据包中的图片
		  -p	不让所监听的接口使用混杂模式
  		  -m	number 指定保存图片数的数目
		  -d	directory 指定保存图片的路径
		  -x	prefix 指定保存图片的前缀名
```

