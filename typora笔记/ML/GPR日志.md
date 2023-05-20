### 23.5.12

zscore标准化：减去均值，除标准差

### 5.14

 ox/red=1表示完全充电后的容量。ox/red=0表示放电后的容量。 

关于论文` Identifying degradation patterns of lithium ion batteries from impedance spectroscopy using
machine learning `

单温度模型：

`Capacity estimation`

```c
1. train_data: 25C01~25C04;
2. test_data: 25C05~25C08;

    容量针对起始容量进行归一化处理
```

`RUL prediction`

```c

```

多温度模型

`Capacity estimation`

```c
1. train_data : 25C01~25C04,35C01,45C01;->EIS_data
    EIS_data:1358*120 :
		/*[)
			＊ 1~201是25C01
        	＊ 201~381是25C02 
        	
        	* 381~451？
        	
        	＊ 451~652 是25C03 
        	
        	* 652~680？
        	
        	* 680~714 是25C04 
        	
        	* 714~761？
        	
        	* 761~1060  是35C01 
        	* 1060~1358 是45C01 
		*/


```



| 电池  | EIS_data_V循环次数 | Capacity_data循环次数 |       循环次数交集       |
| :---: | :----------------: | :-------------------: | :----------------------: |
| 25C02 |        250         |          180          |           180            |
| 25C01 |        261         |          349          |           261            |
| 25C03 |        229         |          201          |           201            |
| 25C04 |         81         |          34           |            34            |
| 35C01 |        327         |          326          |           326            |
| 45C01 |        299         |          299          |           299            |
|       |        1447        |         1389          | 1301（**EIS_data_new**） |



### 5.19 周五 天气挺好

周二讲完组会 ， 周三周四 摆了两天



