<img src='https://img2.baidu.com/it/u=147386698,3468113149&fm=253&fmt=auto&app=138&f=JPEG?w=362&h=270' />

用 IntelliJ TDD FizzBuzz 的详细步骤：

1. Alt + Insert 新建测试类
1. Alt + Insert 创建测试方法
1. 输入测试用名称，回车
1. 输入1，Ctrl + Alt + V 提取变量 number
1. String result = FizzBuzz.of(number)，按 Ctrl + Shift + Enter 自动补全行尾分号，回车
1. assertThat(result).isEqualTo("1")，Ctrl + Shift + Enter 自动化补全行尾分号
1. F2 定位到 FizzBuzz 类名上，Alt +  Shift + Enter 创建类
1. Ctrl + Shift +  T 切换回测试类
1. F2 跳转到 of 方法上，Alt + Shift + Enter 创建方法
1. 3次 回车
1. Ctrl + Shift + T 切换回测试类
1. F2 定位到 assertThat 上，Alt + Shift + Enter 导入方法
1. Ctrl + Shift + F10 运行当前测试方法
1. Ctrl + Shift + T 切换到 FizzBuzz 类
1. return "" + number;
1. Shift + F10 运行测试，测试应该通过
1. 光标移动到方法结尾，按 Ctrl +  W  直到选中整个测试方法
1. Ctrl + D 复制当前选中的代码
1. 按一次左键，定位到方法签名行，修改方法名称
1. ......
