---
title: python图形界面学习
author: lovelves
avatar: 'https://cdn.jsdelivr.net/gh/lingdas/note/img/avactor.jpeg'
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 技术
comments: true
tags: python图形界面
keywords: pyside2
photos: 'https://cdn.jsdelivr.net/gh/lingdas/note/img/18.jpg'
date: 2021-06-26 19:37:42
authorLink:
description: pyside2 学习
---
## pyside2 学习
### 虚拟环境准备
**安装虚拟环境**
```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple virtualenv
```
虚拟环境的使用
**创建虚拟环境**（会多了一个目录名字叫venv）
```bash
virtualenv venv
```
![Alt text](./1625105899663.png)

**激活虚拟环境**（进入虚拟环境 正常的通过命令pip 安装自己需要的包 安装的包存放在venv中的lib/python/site-packages中）

**mac下执行以下命令**
我的venv的位置在根目录 执行以下命令
```bash
source venv/bin/activate
```
**windows下 直接用命令行窗口定位到venv\Scripts\  执行以下命令**
```bash
activate
```
**退出虚拟环境**（退出虚拟环境）
```bash
deactivate
```
### pyside2 安装
```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pyside2
```
### 第一个pyside2小案例
![Alt text](1624677869754.png)
```python
from PySide2.QtWidgets import QApplication, QMainWindow, QPushButton,  QPlainTextEdit

app = QApplication([]) #QApplication 提供了整个图形界面程序的底层管理功能

window = QMainWindow() #创建Windows控件
window.resize(500, 400) #定义窗口大小
window.move(300, 310) #定义窗口的位置
window.setWindowTitle('薪资统计') # 定义窗口的标题

textEdit = QPlainTextEdit(window) #创建文本编辑框
textEdit.setPlaceholderText("请输入薪资表") #显示默认语句
textEdit.move(10,25)
textEdit.resize(300,350)

button = QPushButton('统计', window) #创建按钮
button.move(380,80)

window.show() #显示窗口

app.exec_() # 进入QApplication的事件处理循环，接收用户的输入事件（），并且分配给相应的对象去处理
```
**统计按钮的事件绑定**
```python
def handleCalc():
    print('统计按钮被点击了')
    
button.clicked.connect(handleCalc) #用户点击按钮的时候触发handlecalc方法
```
**获取编辑框的文本(toPlainText)**
```python
text = edit.toPlainText()
```
![Alt text](1624681437998.png)
```python
from PySide2.QtWidgets import QApplication, QMainWindow, QPushButton,  QPlainTextEdit,QMessageBox

class Stats():
    def __init__(self):
        self.window = QMainWindow()
        self.window.resize(500, 400)
        self.window.move(300, 300)
        self.window.setWindowTitle('薪资统计')

        self.textEdit = QPlainTextEdit(self.window)
        self.textEdit.setPlaceholderText("请输入薪资表")
        self.textEdit.move(10, 25)
        self.textEdit.resize(300, 350)

        self.button = QPushButton('统计', self.window)
        self.button.move(380, 80)

        self.button.clicked.connect(self.handleCalc)


    def handleCalc(self):
        info = self.textEdit.toPlainText()

        # 薪资20000 以上 和 以下 的人员名单
        salary_above_20k = ''
        salary_below_20k = ''
        for line in info.splitlines():
            if not line.strip():
                continue
            parts = line.split(' ')
            # 去掉列表中的空字符串内容
            parts = [p for p in parts if p]
            name,salary,age = parts
            if int(salary) >= 20000:
                salary_above_20k += name + '\n'
            else:
                salary_below_20k += name + '\n'

        QMessageBox.about(self.window,
                    '统计结果',
                    f'''薪资20000 以上的有：\n{salary_above_20k}
                    \n薪资20000 以下的有：\n{salary_below_20k}'''
                    )

app = QApplication([])
stats = Stats()
stats.window.show()
app.exec_()
```
### 用Designer来编辑窗口界面
前面的代码都是手动定义按键的位置的 这样太麻烦了 我们可以用界面设计师来完成界面的设计

我是用虚拟环境安装的  安装了pyside2后  在site-packages中的pyside2文件中会找到
![Alt text](1624695029795.png)

打开的界面如下
![Alt text](1624695767042.png)

把要用到的组件往界面拖就是了  很简单
当你把组件放上去 你会发现 有些很难对齐 即使对齐了 界面拖到后 不会自动改变自己的大小  这时候我们就用到布局

Qt是通过界面布局Layout类来实现这种功能的。
我们最常用的 Layout布局 有4种，分别是
QHBoxLayout 水平布局
QHBoxLayout 把控件从左到右 水平横着摆放
QVBoxLayout 垂直布局
QHBoxLayout 把控件从上到下竖着摆放
![Alt text](./1625107312176.png)
![Alt text](./1625107334597.png)

QGridLayout 栅格布局
QGridLayout 把多个控件 按表格布局
![Alt text](./1625107590923.png)

**对界面布局的一些建议**
先不使用任何layout，所有控件按自己喜欢的位置往上摆
然后在从内层开始布局 在到外层
最后通过layout控件大小比例来控制位置**layoutstrentch**
![Alt text](./1625107865147.png)

设置好后保存后 会有一个UI文件
![Alt text](./1625107906673.png)

**用python代码来加载刚创建的ui文件**
加载界面文件
```python
ui = QUiLoader().load('m.ui')  
```
里面的控件对象也成为窗口对象的属性了 可以通过代码定位到组件 比如我想把按钮添加一个方法
```python
 ui.pushButton.clicked.connect(self.handleCalc) 
```
属性叫什么名字 都是自己定义的 比如这个按钮就是 ui.pushButton_5
![Alt text](./1625108948043.png)

### 一个小的案例
![Alt text](1624696822743.png)
```python
from PySide2.QtWidgets import QApplication, QMessageBox
from PySide2.QtUiTools import QUiLoader

class Stats:

    def __init__(self):
        # 从文件中加载UI定义

        # 从 UI 定义中动态 创建一个相应的窗口对象
        # 注意：里面的控件对象也成为窗口对象的属性了
        # 比如 self.ui.button , self.ui.textEdit
        self.ui = QUiLoader().load('m.ui')

        self.ui.pushButton.clicked.connect(self.handleCalc)

    def handleCalc(self):
        info = self.ui.plainTextEdit.toPlainText()

        salary_above_20k = ''
        salary_below_20k = ''
        for line in info.splitlines():
            if not line.strip():
                continue
            parts = line.split(' ')

            parts = [p for p in parts if p]
            name,salary,age = parts
            if int(salary) >= 20000:
                salary_above_20k += name + '\n'
            else:
                salary_below_20k += name + '\n'

        QMessageBox.about(self.ui,
                    '统计结果',
                    f'''薪资20000 以上的有：\n{salary_above_20k}
                    \n薪资20000 以下的有：\n{salary_below_20k}'''
                    )

app = QApplication([])
stats = Stats()
stats.ui.show()
app.exec_()
```
### 常用组件
#### QPushButton 按钮
![Alt text](./1625108448457.png)
**被点击**
```python
pushButton.clicked.connect(handleCalc) #handleCalc是一个方法的名字 负责处理点击后执行的内容
```
**改变按键的的文本**
```python
pushButton.setText(text) #text 字符串  要定义的文本
```
**把按键禁用和启用**
```python
pushButton.setEnabled(False) #禁用
pushButton.setEnabled(True) #启用
```

#### 单行文本框
![Alt text](./1625111786113.png)
**文本被修改**
当文本框中的内容被键盘编辑，被点击就会发出 textChanged 信号，可以这样指定处理该信号的函数
```python
edit.textChanged.connect(handleTextChange) #handleTextChange 是一个方法的名字 负责处理
```
**按下回车键**
当用户在文本框中任何时候按下回车键，就会发出 returnPressed 信号
```python
passwordEdit.returnPressed.connect(onLogin) # onLogin 是一个方法的名字 负责处理
```
**获取文本**
通过 text 方法获取编辑框内的文本内容，比如
```python
text = edit.text()
```
**设置提示**
通过 setPlaceholderText 方法可以设置提示文本内容，比如
```python
edit.setPlaceholderText('请在这里输入URL')
```
**设置文本**
```python
edit.setText('填入你要写的文本')
```
**清除所有文本**
```python
edit.clear()
```
**拷贝文本到剪贴板**
```python
edit.copy()
```
**粘贴剪贴板文本**
```python
edit.paste()
```

#### 多行纯文本框

QPlainTextEdit 是可以 多行 的纯文本编辑框
![Alt text](./1625119956166.png)
注意：在苹果MacOS上，有 更新文本框内容后，需要鼠标滑过才能更新显示的bug

**文本被修改**
当文本框中的内容被键盘编辑，被点击就会发出 textChanged 信号，可以这样指定处理该信号的函数
```python
edit.textChanged.connect(handleTextChange)
```

**光标位置改变**
当文本框中的光标位置变动，就会发出 cursorPositionChanged 信号，可以这样指定处理该信号的函数
```python
edit.cursorPositionChanged.connect(handleChanged)
```

**获取文本**
通过 toPlainText 方法获取编辑框内的文本内容，比如
```python
text = edit.toPlainText()
```

**获取选中文本**
```python
# 获取 QTextCursor 对象
textCursor = edit.textCursor()
selection = textCursor.selectedText()
```

**设置提示**
```python
edit.setPlaceholderText('请在这里输入')
```

**设置文本**
通过 setPlainText 方法设置编辑框内的文本内容 为参数里面的文本字符串，比如
```python
edit.setPlainText('hello')# 原来的所有内容会被清除
```

**在末尾添加文本**
通过 appendPlainText 方法在编辑框末尾添加文本内容，比如
```python
edit.appendPlainText('末尾添加文本内容') # 注意：这种方法会在添加文本后 自动换行
```

**在光标处插入文本**
通过 insertPlainText 方法在编辑框末尾添加文本内容，比如
```python
edit.insertPlainText('末尾添加文本内容') # 注意：这种方法 不会 在添加文本后自动换行
```

**清除所有文本**
clear 方法可以清除编辑框内所有的文本内容，比如
```python
edit.clear() 
```

**拷贝文本到剪贴板**
copy 方法可以拷贝当前选中文本到剪贴板，比如
```python
edit.copy()
```

**粘贴剪贴板文本**
paste 方法可以把剪贴板内容，拷贝到编辑框当前光标所在处，比如
```python
edit.paste()
```

#### 文本浏览框
QTextBrowser 是 只能查看文本 控件。
通常用来显示一些操作日志信息、或者不需要用户编辑的大段文本内容。
该控件 获取文本、设置文本、清除文本、剪贴板复制粘贴 等等， 都和上面介绍的 多行纯文本框是一样的。

**在末尾添加文本**
通过 append 方法在编辑框末尾添加文本内容，比如
```python
textBrowser.append('末尾添加文本内容')
```
有时，浏览框里面的内容长度超出了可见范围，我们在末尾添加了内容，往往希望控件自动翻滚到当前添加的这行，
可以通过 ensureCursorVisible 方法来实现
```python
textBrowser.append('末尾添加文本内容')
textBrowser.ensureCursorVisible() #注意：这种方法会在添加文本后 自动换行
```

**在光标处插入文本**
通过 insertPlainText 方法在编辑框末尾添加文本内容，比如
```python
edit.insertPlainText('添加文本内容') # 这种方法 不会 在添加文本后自动换行
```

#### 标签
QLabel 就是常见的标签，可以用来显示文字（包括纯文本和富文本）、图片 甚至动画
![Alt text](./1625124644490.png)

**改变文本**
代码中可以使用 setText 方法来改变标签文本内容，比如
```python
button.setText(text)
```

**图片**
QLabel可以用来显示图片，有时一个图片可以让界面好看很多，可以在 Qt Designer上 属性编辑器 QLabel 栏 的 pixmap 属性设置中选择图片文件指定。
![Alt text](./1625124996613.png)

#### 组合选择框
![Alt text](./1625126750863.png)

**选项改变**
如果用户操作修改了QComboBox中的选项就会发出 currentIndexChanged 信号，可以这样指定处理该信号的函数
```python
cbox.currentIndexChanged.connect(handleSelectionChange)
```

**添加一个选项**
代码中可以使用 addItem 方法来添加一个选项到 末尾 ，参数就是选项文本
```python
cbox.addItem('byhy')
```

**添加多个选项**
代码中可以使用 addItems 方法来添加多个选项到 末尾，参数是包含了多个选项文本的列表
```python
cbox.addItems(['java','c++','python教程'])
```

**清空选项**
代码中可以使用 clear 方法来清空选项，也就是删除选择框内所有的选项
```python
cbox.clear()
```

**获取当前选项文本**
代码中可以使用 currentText 方法来获取当前 选中的选项 的文本，比如
```python
method = cbox.currentText()
```

#### 列表
QListWidget 是列表控件，如下图所示
![Alt text](./1625127093310.png)

**添加一个选项**
代码中可以使用 addItem 方法来添加一个选项到 末尾 ，参数就是选项文本
```python
listWidget.addItem('helo')
```

**添加多个选项**
代码中可以使用 addItems 方法来添加多个选项到 末尾，参数是包含了多个选项文本的列表
```python
listWidget.addItems(['go','java','python教程'])
```

**删除一个选项**
代码中可以使用 takeItem 方法来删除1个选项，参数是该选项所在行
```python
listWidget.takeItem(1) #就会删除第二行选项
```

**清空选项**
代码中可以使用 clear 方法来清空选项，也就是删除选择框内所有的选项
```python
listWidget.clear()
```

**获取当前选项文本**
currentItem 方法可以得到列表当前选中项对象（QListWidgetItem） ，再调用这个对象的 text 方法，就可以获取文本内容，比如
```python
listWidget.currentItem().text() # 就获取了 第1行，第1列 的单元格里面的文本。
```

#### 表格
QTableWidget 是表格控件，如下图所示
![Alt text](./1625131379550.png)

**创建列 和 标题栏**
我们可以通过 Qt designer 为一个表格创建列和对应的标题栏。
只需要双击 Qt designer 设计的窗体中的 表格控件， 就会出现这样的对话框。
![Alt text](./1625131440628.png)

**插入一行、删除一行**
insertRow 方法可以在指定位置插入一行，比如
```python
table.insertRow(0) # 就插入一行到第 1 行这个位置， 表格原来第1行（包括原来的第1行）以后的内容，全部往下移动一行。
```
removeRow 方法可以删除指定位置的一行，比如
```python
table.removeRow(0) # 就插入一行到第 1 行这个位置， 表格原来第1行（包括原来的第1行）以后的内容，全部往下移动一行。
```

**设置单元格文本内容**
qt表格的单元格内的内容对象 是一个 单元格对象 QTableWidgetItem 实例
如果单元格 没有被设置过 内容，可以这样
```python
from PySide2.QtWidgets import QTableWidgetItem
table.setItem(0, 0, QTableWidgetItem('hello'))# 0行0列添加文本hello
```

如果希望某个单元格为 只读，不允许修改，可以使用QTableWidgetItem对象的 setFlags 方法，像这样
```python
from PySide2.QtWidgets import QTableWidgetItem
from PySide2.QtCore import Qt

item = QTableWidgetItem('hello')
item.setFlags(Qt.ItemIsEnabled) # 参数名字段不允许修改
table.setItem(row, 0, item)
```
如果想文本内容 居中对齐，每个当对应的QTableWidgetItem 调用 setTextAlignment，如下
```python
from PySide2.QtWidgets import QTableWidgetItem
from PySide2.QtCore import Qt

item = QTableWidgetItem()
item.setText('hello')
# 文本居中
item.setTextAlignment(Qt.AlignHCenter) 
table.setItem(row, 0, item)
```

**获取单元格文本的内容**
item 方法可以指定位置的单元格对象（QTableWidgetItem） ，再调用这个对象的 text 方法，就可以获取文本内容，比如
```python
table.item(0,0).text()
```

**获取所有行数、列数**
代码中可以使用 rowCount 方法来获取表格所有的 行数 ，比如
```python
rowcount = table.rowCount()
```
可以使用 columnCount 方法来获取表格所有的 列数 ，比如
```python
rowcount = table.columnCount()
```

**获取当前选中是第几行**
代码中可以使用 currentRow 方法来获取当前选中是第几行，比如
```python
currentrow = table.currentRow()
```
获取选中的行的内容
```python
table.selectedItems()
```

**设置表格行数、列数**
代码中可以使用 setRowCount 方法来设置表格 行数 ，比如
```python
table.setRowCount(10)
```
代码中可以使用 setColumnCount 方法来设置表格 列数 ，比如
```python
table.setColumnCount(10)
```

**清除/删除所有内容**
clearContents 方法可以清除表格所有的内容，比如
```python
table.clearContents()
```
清除后，仍然会留下表格栏
如果连表格栏都要删除，可以使用 setRowCount(0)，像这样
```python
table.setRowCount(0)
```

**设定列宽、宽度自动缩放**
Qt Designer 上目前没法拖拽设定 每个列的宽度，只能在代码中指定。
```python
#设定第1列的宽度为 180像素
table.setColumnWidth(0, 180)
#设定第2列的宽度为 100像素
table.setColumnWidth(1, 100)
```
如想让 表格控件宽度 随着父窗口的缩放自动缩放，可以
```python
table.horizontalHeader().setStretchLastSection(True)
```

**单元格内容改动**
当用户修改了一个单元格的内容，会发出 cellChanged 信号，并且携带参数指明该单元格的行号和列号。
我们的代码可以对该信号进行相应的处理。
示例代码如下
```python
def __init__(self):
        # 指定单元格改动信号处理函数
        self.ui.table.cellChanged.connect(self.cfgItemChanged)

    def cfgItemChanged(self,row,column):
        # 获取更改内容
        cfgName = self.ui.table.item(row, 0).text() # 首列为配置名称
        cfgValue = self.ui.table.item(row, column).text()
```
