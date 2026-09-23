---
translation_status: machine_translated
source: Concepts/Basic/About-Interfaces.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="interfaces"></span>

# 接口

<span id="background"></span>

## 背景

ROS应用程序一般通过三种类型之一的接口进行通信: [话题](About-Topics.md), [服务](About-Services.md),或 [动作](About-Actions.md). ROS 2使用简化的描述语言,即界面定义语言(IDL)来描述这些界面,这种描述使得ROS工具更容易在多个目标语言中自动生成界面类型的源代码.

在本文件中,我们将描述所支持的类型:

- 毫斯克( msg) : `.msg` 文件是描述 ROS 信件字段的简单文本文件。它们用于生成不同语言信件的源代码。

- (原始内容存档于2019-09-21). srv: `.srv` 文件描述一个服务。它们由两个部分组成:请求和回复。请求和回复是信息声明。

- 动作 : `.action` 文件描述动作。它们由三个部分组成:目标、结果和反馈。每个部分都是信息声明本身。

<span id="messages"></span>

## 信件

消息是 ROS 2 节点将网络上的数据发送到其他 ROS 节点的一种方式,没有预期的响应。例如,如果 ROS 2 节点读取传感器的温度数据,它就可以使用一个 `Temperature` 消息。ROS 2网络上的其他节点可以订阅该数据并接收 `Temperature` 留言。

信件描述和定义于 `.msg` 文档中 `msg/` ROS 软件包的目录。 `.msg` 文件由两个部分组成:字段和常数。

<span id="fields"></span>

### 字段

每个字段由一个类型和一个名称组成,以空格分隔,即:

``` bash
fieldtype1 fieldname1
fieldtype2 fieldname2
fieldtype3 fieldname3
```

例如:

``` bash
int32 my_int
string my_string
```

<span id="field-types"></span>

#### 字段类型

字段类型可以是:

- a 内置类型

- 自行定义的信件描述名称, 如“ 几何- msgs/ PoseStamped ”

*目前支持的内建型号 :*

| 类型名称 | [C++](https://design.ros2.org/articles/generated_interfaces_cpp.html) | [Python](https://design.ros2.org/articles/generated_interfaces_python.html) | [DDS 类型](https://design.ros2.org/articles/mapping_dds_types.html) |
|----|----|----|----|
| 嘘 | 嘘 | 内存. bool | 布尔 |
| 字节 | uint8_t | 内建值. 字节\* | octet 数据 |
| 字符 | 字符 | 内建图。 int \* | 字符 |
| 浮点32 | 浮点 | 内建。 float\* | 浮点 |
| 浮点64 | 双倍 | 内建。 float\* | 双倍 |
| 单位8 | int8_t | 内建图。 int \* | octet 数据 |
| 金特8号 | uint8_t | 内建图。 int \* | octet 数据 |
| 单位 16 | int16_t | 内建图。 int \* | 简称 |
| 金特16号 | uint16_t | 内建图。 int \* | 未签名的短片 |
| 单位32 | int32_t | 内建图。 int \* | 长 |
| 金特32号 | uint32_t | 内建图。 int \* | 未签名长 |
| 单位64 | int64_t | 内建图。 int \* | 长长 |
| 金特64号 | uint64_t | 内建图。 int \* | 未签名长 |
| 字符串 | std: 字符串 | 内建图.str | 字符串 |
| 字符串 | std: u16 字符串 | 内建图.str | 字符串 |

*每个内置类型都可以用来定义数组:*

| 类型名称 | [C++](https://design.ros2.org/articles/generated_interfaces_cpp.html) | [Python](https://design.ros2.org/articles/generated_interfaces_python.html) | [DDS 类型](https://design.ros2.org/articles/mapping_dds_types.html) |
|----|----|----|----|
| 静态阵列 | std: 阵列\<T, N\> | 内建的. list\* | T\[N\] |
| 未绑定的动态数组 | std: 显示器 | 内建的. list | 顺序 |
| 边框动态数组 | 自定义类\<T, N\> | 内建的. list\* | 序列\<T, N\> |
| 边框字符串 | std: 字符串 | 内建结构. str\* | 字符串 |

(\*) 所有类型比ROS定义更为宽容,通过软件在范围和长度上强制实施ROS限制。

*使用数组和限定类型来定义信件的示例 :*

``` bash
int32[] unbounded_integer_array
int32[5] five_integers_array
int32[<=5] up_to_five_integers_array

string string_of_unbounded_size
string<=10 up_to_ten_characters_string

string[<=5] up_to_five_unbounded_strings
string<=10[] unbounded_array_of_strings_up_to_ten_characters_each
string<=10[<=5] up_to_five_strings_up_to_ten_characters_each
```

<span id="field-names"></span>

#### 字段名称

字段名称必须是小写字母数字字符,加下划线用于分隔单词。它们必须从字母字符开始,不得以下划线或连续两个下划线结束。

<span id="field-default-value"></span>

#### 字段默认值

默认值可以设定为信件类型中的任何字段。目前不支持字符串数组和复杂类型(即上面内置类型表格中未显示的类型;这适用于所有嵌套信件)的默认值。

定义默认值的方法是在字段定义行中添加第三个元素,即:

``` bash
fieldtype fieldname fielddefaultvalue
```

例如:

``` bash
uint8 x 42
int16 y -2000
string full_name "John Doe"
int32[] samples [-200, -100, 0, 100, 200]
```

> **说明**
>
> - 字符串值必须用单项定义 `'` 双倍 `"` 引用
>
> - 当前字符串值没有被跳出

<span id="constants"></span>

### 常数

每个常数定义都类似于一个带有默认值的字段描述,只是这个值永远不能在程序上更改. 这个值的指派通过使用等QQ符号来表示,例如.

``` bash
constanttype CONSTANTNAME=constantvalue
```

例如:

``` bash
int32 X=123
int32 Y=-123
string FOO="foo"
string EXAMPLE='bar'
```

> **说明**
>
> 常数名称必须是 UPPERCASE

<span id="services"></span>

## 服务

服务是一种请求/响应通信,客户端(请求者)正在等待服务器(答复者)进行简短的计算并返回结果.

服务描述和定义如下: `.srv` 文档中 `srv/` ROS 软件包的目录。

服务描述文件包含一个请求和一个响应 msg 类型, 由 `---`任意两个 `.msg` 配置为 a 的文件 `---` 是一个法律服务描述。

以下是一个非常简单的服务实例,它使用字符串并返回一个字符串:

``` bash
string str
---
string str
```

当然,我们可以更复杂一些(如果你想提及同一软件包的讯息,则不得提及软件包名称):

``` bash
# request constants
int8 FOO=1
int8 BAR=2
# request fields
int8 foobar
another_pkg/AnotherMessage msg
---
# response constants
uint32 SECRET=123456
# response fields
another_pkg/YetAnotherMessage val
CustomMessageDefinedInThisPackage value
uint32 an_integer
```

您无法将其它服务嵌入服务内部 。

<span id="actions"></span>

## 动作

动作是一种长期的请求/应答通信,其中动作客户端(请求者)正在等待动作服务器(响应者)采取一些行动并返回结果。相对于服务,动作可以是长期运行(很多秒或分钟),在进行时提供反馈,也可以被中断。

行动定义的形式如下:

``` default
<request_type> <request_fieldname>
---
<response_type> <response_fieldname>
---
<feedback_type> <feedback_fieldname>
```

与服务一样,请求字段在前,响应字段在后 第一个三加一(`---`),这些字段分别是,第二三段后还有第三套字段,即发送反馈时发送的字段.

可能存在任意数量的请求字段(包括零),任意数量的反应字段(包括零),任意数量反馈字段(包括零).

那个... `<request_type>`, `<response_type>`,以及 `<feedback_type>` 遵守所有相同的规则 `<type>` {\fn黑体\fs20\shad2\2aH82\3aH20\4aH33\fscx95\3cH592001\be1}给一个信息 `<request_fieldname>`, `<response_fieldname>`,以及 `<feedback_fieldname>` 遵守所有相同的规则 `<fieldname>` 给一个信息。

例如, `Fibonacci` 行动定义包括以下内容:

``` default
int32 order
---
int32[] sequence
---
int32[] sequence
```

这是动作客户端发送单曲的动作定义 。 `int32` 字段表示要采取的 Fibonacci 步骤的数量,并期望动作服务器生成一系列 `int32` 包含完整步骤。沿途,动作服务器也可以提供中间数组: `int32` 包含到某一点为止所完成的步骤。
