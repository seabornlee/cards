Alt + Insert 新建测试类
Alt + Insert 创建测试方法
输入测试用名称，回车
输入1，Ctrl + Alt + V 提取变量 number
String result = FizzBuzz.of(number)，按 Ctrl + Shift + Enter 自动补全行尾分号，回车
assertThat(result).isEqualTo("1")，Ctrl + Shift + Enter 自动化补全行尾分号
F2 定位到 FizzBuzz 类名上，Alt +  Shift + Enter 创建类
Ctrl + Shift +  T 切换回测试类
F2 跳转到 of 方法上，Alt + Shift + Enter 创建方法
3次 回车
Ctrl + Shift + T 切换回测试类
F2 定位到 assertThat 上，Alt + Shift + Enter 导入方法
Ctrl + Shift + F10 运行当前测试方法
Ctrl + Shift + T 切换到 FizzBuzz 类
return "" + number;
Shift + F10 运行测试，测试应该通过
光标移动到方法结尾，按 Ctrl +  W  直到选中整个测试方法
Ctrl + D 复制当前选中的代码
按一次左键，定位到方法签名行，修改方法名称
......
