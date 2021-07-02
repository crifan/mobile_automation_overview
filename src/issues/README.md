# 常见问题

移动端自动化测试会遇到的一些问题，现整理如下。

比如 [Same test cases for Android and iOS automation — pros and cons | by Satyajit Malugu | Medium](https://medium.com/@satyajit/same-test-cases-for-android-and-ios-automation-pros-and-cons-e65cfb30e9ed)，其中就提到了：

* 返回按钮的处理等问题
* 界面的源码结构不同
  * iOS：会返回当前页面中所有的元素，包括不可见的（需要滚动后才可见的那些）元素
  * Android：只会当前页面中可见的元素

导致实现移动端的多平台统一测试用例，很不容易

## OCR识别复杂游戏界面中文字，偶尔会误判出错

比如之前折腾：

【已解决】安卓游戏暗黑觉醒自动化：稳定的检测出是首充豪礼首充6元首充98元的首充弹框

期间，对于 首充豪礼 弹框页面：

![img_orc_first_charge_1](../assets/img/img_orc_first_charge_1.jpg)

![img_orc_first_charge_2](../assets/img/img_orc_first_charge_2.jpg)

偶尔可以返回：

```json
{'chars': [{'char': '首', 'location': {'width': 36, 'top': 834, 'left': 938, 'height': 60}}, {'char': '充', 'location': {'width': 36, 'top': 834, 'left': 991, 'height': 60}}, {'char': '6', 'location': {'width': 30, 'top': 834, 'left': 1040, 'height': 60}}, {'char': '元', 'location': {'width': 36, 'top': 834, 'left': 1084, 'height': 60}}, {'char': '首', 'location': {'width': 36, 'top': 834, 'left': 1230, 'height': 60}}, {'char': '充', 'location': {'width': 36, 'top': 834, 'left': 1303, 'height': 60}}, {'char': '9', 'location': {'width': 29, 'top': 834, 'left': 1334, 'height': 60}}, {'char': '8', 'location': {'width': 30, 'top': 834, 'left': 1389, 'height': 60}}, {'char': '元', 'location': {'width': 57, 'top': 834, 'left': 1432, 'height': 60}}], 'location': {'width': 551, 'top': 834, 'left': 938, 'height': 60}, 'words': '首充6元首充98元'}
```

完美解析的效果，即：`首充6元首充98元`，是图片中准确的文字和充值金额。

由于游戏界面中文字比较复杂，尤其是：

* 特殊的字体
  * 首充豪礼 字体很特别
    * 估计是游戏常用字体
* 额外加了闪光等效果
  * 比如 首充98元
    * 中的 首充 或 98元 外圈闪光

等特殊情况，导致文字检测出来，常常会误判：

比如：

（1）首元首充8元

```json
{'chars': [{'char': '首', 'location': {'width': 36, 'top': 836, 'left': 939, 'height': 59}}, {'char': '元', 'location': {'width': 36, 'top': 836, 'left': 1085, 'height': 59}}, {'char': '首', 'location': {'width': 36, 'top': 836, 'left': 1231, 'height': 59}}, {'char': '充', 'location': {'width': 36, 'top': 836, 'left': 1286, 'height': 59}}, {'char': '8', 'location': {'width': 29, 'top': 836, 'left': 1390, 'height': 59}}, {'char': '元', 'location': {'width': 55, 'top': 836, 'left': 1432, 'height': 59}}], 'location': {'width': 549, 'top': 836, 'left': 939, 'height': 59}, 'words': '首元首充8元'}
```

（2）首元首8元

```json
{'chars': [{'char': '首', 'location': {'width': 36, 'top': 837, 'left': 938, 'height': 61}}, {'char': '元', 'location': {'width': 75, 'top': 837, 'left': 1087, 'height': 61}}, {'char': '首', 'location': {'width': 37, 'top': 837, 'left': 1236, 'height': 61}}, {'char': '8', 'location': {'width': 30, 'top': 837, 'left': 1381, 'height': 61}}, {'char': '元', 'location': {'width': 54, 'top': 837, 'left': 1424, 'height': 61}}], 'location': {'width': 547, 'top': 837, 'left': 938, 'height': 61}, 'words': '首元首8元'}
```

（3）首充元+首充98

```json
{'chars': [{'char': '首', 'location': {'width': 37, 'top': 832, 'left': 934, 'height': 62}}, {'char': '充', 'location': {'width': 38, 'top': 832, 'left': 990, 'height': 62}}, {'char': '元', 'location': {'width': 57, 'top': 832, 'left': 1087, 'height': 62}}, {'char': '+', 'location': {'width': 31, 'top': 832, 'left': 1136, 'height': 62}}, {'char': '首', 'location': {'width': 38, 'top': 832, 'left': 1239, 'height': 62}}, {'char': '充', 'location': {'width': 37, 'top': 832, 'left': 1297, 'height': 62}}, {'char': '9', 'location': {'width': 30, 'top': 832, 'left': 1348, 'height': 62}}, {'char': '8', 'location': {'width': 30, 'top': 832, 'left': 1388, 'height': 62}}], 'location': {'width': 553, 'top': 832, 'left': 934, 'height': 62}, 'words': '首充元+首充98'}
```

（4）首充5元充98元

```json
{'chars': [{'char': '首', 'location': {'width': 58, 'top': 832, 'left': 933, 'height': 62}}, {'char': '充', 'location': {'width': 38, 'top': 832, 'left': 990, 'height': 62}}, {'char': '5', 'location': {'width': 30, 'top': 832, 'left': 1043, 'height': 62}}, {'char': '元', 'location': {'width': 77, 'top': 832, 'left': 1087, 'height': 62}}, {'char': '充', 'location': {'width': 38, 'top': 832, 'left': 1300, 'height': 62}}, {'char': '9', 'location': {'width': 31, 'top': 832, 'left': 1332, 'height': 62}}, {'char': '8', 'location': {'width': 31, 'top': 832, 'left': 1390, 'height': 62}}, {'char': '元', 'location': {'width': 57, 'top': 832, 'left': 1435, 'height': 62}}], 'location': {'width': 558, 'top': 832, 'left': 933, 'height': 62}, 'words': '首充5元充98元'}
```

（5）首兄元首9元

```json
{'chars': [{'char': '首', 'location': {'width': 36, 'top': 836, 'left': 941, 'height': 61}}, {'char': '兄', 'location': {'width': 37, 'top': 836, 'left': 996, 'height': 61}}, {'char': '元', 'location': {'width': 37, 'top': 836, 'left': 1090, 'height': 61}}, {'char': '首', 'location': {'width': 36, 'top': 836, 'left': 1239, 'height': 61}}, {'char': '9', 'location': {'width': 30, 'top': 836, 'left': 1345, 'height': 61}}, {'char': '元', 'location': {'width': 59, 'top': 836, 'left': 1427, 'height': 61}}], 'location': {'width': 545, 'top': 836, 'left': 941, 'height': 61}, 'words': '首兄元首9元'}
```

（6）首元98元

```json
{'chars': [{'char': '首', 'location': {'width': 36, 'top': 837, 'left': 940, 'height': 61}}, {'char': '元', 'location': {'width': 37, 'top': 837, 'left': 1088, 'height': 61}}, {'char': '9', 'location': {'width': 29, 'top': 837, 'left': 1344, 'height': 61}}, {'char': '8', 'location': {'width': 30, 'top': 837, 'left': 1381, 'height': 61}}, {'char': '元', 'location': {'width': 61, 'top': 837, 'left': 1424, 'height': 61}}], 'location': {'width': 546, 'top': 837, 'left': 940, 'height': 61}, 'words': '首元98元'}
```

（7）首元首元

```json
{'chars': [{'char': '首', 'location': {'width': 34, 'top': 837, 'left': 938, 'height': 56}}, {'char': '元', 'location': {'width': 34, 'top': 837, 'left': 1092, 'height': 56}}, {'char': '首', 'location': {'width': 34, 'top': 837, 'left': 1246, 'height': 56}}, {'char': '元', 'location': {'width': 50, 'top': 837, 'left': 1420, 'height': 56}}], 'location': {'width': 547, 'top': 837, 'left': 938, 'height': 56}, 'words': '首元首元'}
```

（8）首元道充98元

```json
{'chars': [{'char': '首', 'location': {'width': 35, 'top': 835, 'left': 940, 'height': 60}}, {'char': '元', 'location': {'width': 35, 'top': 835, 'left': 1085, 'height': 60}}, {'char': '道', 'location': {'width': 35, 'top': 835, 'left': 1230, 'height': 60}}, {'char': '充', 'location': {'width': 36, 'top': 835, 'left': 1302, 'height': 60}}, {'char': '9', 'location': {'width': 30, 'top': 835, 'left': 1333, 'height': 60}}, {'char': '8', 'location': {'width': 30, 'top': 835, 'left': 1387, 'height': 60}}, {'char': '元', 'location': {'width': 35, 'top': 835, 'left': 1430, 'height': 60}}], 'location': {'width': 531, 'top': 835, 'left': 940, 'height': 60}, 'words': '首元道充98元'}
```

（9）首充元首充98元

```json
{'chars': [{'char': '首', 'location': {'width': 61, 'top': 828, 'left': 934, 'height': 67}}, {'char': '充', 'location': {'width': 41, 'top': 829, 'left': 995, 'height': 66}}, {'char': '元', 'location': {'width': 41, 'top': 830, 'left': 1095, 'height': 67}}, {'char': '首', 'location': {'width': 61, 'top': 833, 'left': 1237, 'height': 67}}, {'char': '充', 'location': {'width': 39, 'top': 833, 'left': 1297, 'height': 67}}, {'char': '9', 'location': {'width': 33, 'top': 834, 'left': 1331, 'height': 66}}, {'char': '8', 'location': {'width': 34, 'top': 834, 'left': 1391, 'height': 67}}, {'char': '元', 'location': {'width': 39, 'top': 835, 'left': 1439, 'height': 67}}], 'location': {'width': 554, 'top': 828, 'left': 934, 'height': 74}, 'words': '首充元首充98元'}
```

（10）首6元首充8元

```json
{'chars': [{'char': '首', 'location': {'width': 41, 'top': 835, 'left': 942, 'height': 66}}, {'char': '6', 'location': {'width': 34, 'top': 835, 'left': 1036, 'height': 66}}, {'char': '元', 'location': {'width': 82, 'top': 835, 'left': 1084, 'height': 66}}, {'char': '首', 'location': {'width': 41, 'top': 835, 'left': 1247, 'height': 66}}, {'char': '充', 'location': {'width': 39, 'top': 835, 'left': 1288, 'height': 66}}, {'char': '8', 'location': {'width': 33, 'top': 835, 'left': 1382, 'height': 66}}, {'char': '元', 'location': {'width': 82, 'top': 835, 'left': 1431, 'height': 66}}], 'location': {'width': 587, 'top': 835, 'left': 942, 'height': 66}, 'words': '首6元首充8元'}
```

等等情况。

所以如果用如下逻辑（正则）

```python
"首充((\d+元)|(元)|(\d+))", # 首充元 / 首充xxx元 / 首充xxx
```

的代码

```python
    def isGotoPayPopupPage(self, isRespLocation=False):
        """Check is goto payment popup page or not"""
        gotoPayStrList = [
            "^前往充值$", # 剑玲珑
            "^立即充值$", # 至尊屠龙
            "^充值$", # 剑玲珑，首充之后，手动点击 每日充值 后
            "^充点小钱$", # 御剑仙缘
            # "^首充豪礼?", # 暗黑觉醒：首充豪礼 但有时候无法识别出 礼 -》只能用于识别，无法用于点击，所以换成下面：
            # 暗黑觉醒：首充6元 和 首充98元 -> 
            # "首充\d+元", #（1）识别成 首元首充8元
            # "首充?\d+元", # （2）首元首8元
            "首充((\d+元)|(元)|(\d+))", # 首充元 / 首充xxx元 / 首充xxx


            # 剑玲珑，首充 之后，但是点击 领取 并不能进入下一页
            # "^领取$",
            # "^领取奖励$", # 偶尔会由于屏幕弹框 领域奖励 而误判进入了 前往充值 页面
        ]
        respBoolOrTuple = self.isExistAnyStr(gotoPayStrList, isRespFullInfo=isRespLocation)
        logging.info("GotoPay: respBoolOrTuple=%s", respBoolOrTuple)
        return respBoolOrTuple
```

注：`isExistAnyStr`的具体实现，可参考：[图像 · Python常用代码段](https://book.crifan.com/books/python_common_code_snippet/website/common_code/image.html)

去单次调用，往往会误判，而无法识别出我们希望的值

所以，为了稳定的检测出是否是首充豪礼的弹框充值野蛮，后来改用逻辑：

多次尝试调用，直到检测成功为止

具体代码是：

```python
    def isGotoPayPopupPage_multipleRetry(self, isRespLocation=False):
        """鉴于 暗黑觉醒 的 首充弹框 的检测很不稳定，所以 增加此函数 多次尝试 以确保 当是首充页面时，的确能稳定的检测出的确是"""
        respBoolOrTuple = CommonUtils.multipleRetry(
            functionInfoDict = {
                "functionCallback": self.isGotoPayPopupPage,
                "functionParaDict": {
                    "isRespLocation": isRespLocation,
                },
            },
            isRespFullRetValue = isRespLocation,
        )
        return respBoolOrTuple
```

注：其中`multipleRetry`详见：[通用逻辑 · Python常用代码段](https://book.crifan.com/books/python_common_code_snippet/website/common_code/common_logic.html)
