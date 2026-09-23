<span id="interfaces"></span>
# 接口

<span id="background"></span>
## 背景

ROS 应用通常通过三种接口之一进行通信：[话题](About-Topics.md)、[服务](About-Services.md)或[动作](About-Actions.md)。ROS 2 使用一种简化的描述语言，即接口定义语言（IDL），来描述这些接口。借助这种描述，ROS 工具可以轻松地为接口类型自动生成多种目标语言的源代码。

本文介绍以下受支持的类型：

- msg：`.msg` 是描述 ROS 消息字段的简单文本文件，用于生成不同语言的消息源代码。
- srv：`.srv` 文件描述服务，由请求和响应两部分组成。请求和响应各自都是一份消息声明。
- action：`.action` 文件描述动作，由目标、结果和反馈三部分组成。每一部分本身都是一份消息声明。

<span id="messages"></span>
## 消息

消息使 ROS 2 节点能够通过网络向其他 ROS 节点发送数据，而不期望对方作出响应。例如，ROS 2 节点从传感器读取温度数据后，可以使用 `Temperature` 消息在 ROS 2 网络上发布数据。网络中的其他节点可以订阅这些数据，接收 `Temperature` 消息。

消息在 ROS 软件包 `msg/` 目录中的 `.msg` 文件内描述和定义。`.msg` 文件由字段和常量两部分组成。

<span id="fields"></span>
### 字段

每个字段由类型和名称组成，两者以空格分隔，即：

```bash
fieldtype1 fieldname1
fieldtype2 fieldname2
fieldtype3 fieldname3
```

例如：

```bash
int32 my_int
string my_string
```

<span id="field-types"></span>
#### 字段类型

字段类型可以是：

- 内置类型。
- 单独定义的消息类型名称，例如 `geometry_msgs/PoseStamped`。

**目前支持的内置类型：**

| 类型名称 | [C++](https://design.ros2.org/articles/generated_interfaces_cpp.html) | [Python](https://design.ros2.org/articles/generated_interfaces_python.html) | [DDS 类型](https://design.ros2.org/articles/mapping_dds_types.html) |
| --- | --- | --- | --- |
| `bool` | `bool` | `builtins.bool` | `boolean` |
| `byte` | `uint8_t` | `builtins.bytes`* | `octet` |
| `char` | `char` | `builtins.int`* | `char` |
| `float32` | `float` | `builtins.float`* | `float` |
| `float64` | `double` | `builtins.float`* | `double` |
| `int8` | `int8_t` | `builtins.int`* | `octet` |
| `uint8` | `uint8_t` | `builtins.int`* | `octet` |
| `int16` | `int16_t` | `builtins.int`* | `short` |
| `uint16` | `uint16_t` | `builtins.int`* | `unsigned short` |
| `int32` | `int32_t` | `builtins.int`* | `long` |
| `uint32` | `uint32_t` | `builtins.int`* | `unsigned long` |
| `int64` | `int64_t` | `builtins.int`* | `long long` |
| `uint64` | `uint64_t` | `builtins.int`* | `unsigned long long` |
| `string` | `std::string` | `builtins.str` | `string` |
| `wstring` | `std::u16string` | `builtins.str` | `wstring` |

**每种内置类型都可以用来定义数组：**

| 类型名称 | [C++](https://design.ros2.org/articles/generated_interfaces_cpp.html) | [Python](https://design.ros2.org/articles/generated_interfaces_python.html) | [DDS 类型](https://design.ros2.org/articles/mapping_dds_types.html) |
| --- | --- | --- | --- |
| 固定长度数组 | `std::array<T, N>` | `builtins.list`* | `T[N]` |
| 无长度上限的动态数组 | `std::vector` | `builtins.list` | `sequence` |
| 有长度上限的动态数组 | `custom_class<T, N>` | `builtins.list`* | `sequence<T, N>` |
| 有长度上限的字符串 | `std::string` | `builtins.str`* | `string` |

（*）对于比 ROS 定义允许更宽取值范围或长度的类型，会通过软件检查来实施 ROS 的范围和长度约束。

**使用数组和有界类型的消息定义示例：**

```bash
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

字段名称只能使用小写字母、数字和用于分隔单词的下划线。名称必须以字母开头，不能以下划线结尾，也不能包含两个连续的下划线。

<span id="field-default-value"></span>
#### 字段默认值

可以为消息类型中的字段设置默认值。不过，目前字符串数组和复杂类型尚不支持默认值。复杂类型指未出现在上述内置类型表中的类型，这一限制适用于所有嵌套消息。

在字段定义行中添加第三项即可定义默认值，即：

```bash
fieldtype fieldname fielddefaultvalue
```

例如：

```bash
uint8 x 42
int16 y -2000
string full_name "John Doe"
int32[] samples [-200, -100, 0, 100, 200]
```

!!! note "注意"
    - 字符串值必须使用单引号 `'` 或双引号 `"` 包围。
    - 目前不会对字符串值进行转义处理。

<span id="constants"></span>
### 常量

常量的定义类似于带默认值的字段定义，但它的值不能通过程序修改。常量通过等号 `=` 赋值，例如：

```bash
constanttype CONSTANTNAME=constantvalue
```

例如：

```bash
int32 X=123
int32 Y=-123
string FOO="foo"
string EXAMPLE='bar'
```

!!! note "注意"
    常量名称必须使用大写字母。

<span id="services"></span>
## 服务

服务采用请求/响应通信方式，客户端（请求方）等待服务端（响应方）完成一次短时间的计算并返回结果。

服务在 ROS 软件包 `srv/` 目录中的 `.srv` 文件内描述和定义。

服务描述文件由请求和响应两个消息类型组成，中间以 `---` 分隔。任意两个 `.msg` 文件用 `---` 连接起来，便是一份合法的服务描述。

下面是一个非常简单的服务示例：接收一个字符串，返回一个字符串。

```bash
string str
---
string str
```

当然，也可以定义更复杂的服务。如果要引用同一软件包中的消息，不能写上软件包名称：

```bash
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

不能在一个服务中嵌套另一个服务。

<span id="actions"></span>
## 动作

动作是一种面向耗时任务的请求/响应通信方式，动作客户端（请求方）等待动作服务端（响应方）执行某项操作并返回结果。与服务不同，动作可以运行较长时间（数秒或数分钟），在执行期间提供反馈，并且可以被中断。

动作定义采用以下形式：

```text
<request_type> <request_fieldname>
---
<response_type> <response_fieldname>
---
<feedback_type> <feedback_fieldname>
```

与服务一样，请求字段位于第一个三连字符分隔符 `---` 之前，响应字段位于其后。第二个三连字符之后还有第三组字段，用于发送反馈。

请求字段、响应字段和反馈字段的数量都可以是任意值，包括零。

`<request_type>`、`<response_type>` 和 `<feedback_type>` 遵循与消息字段 `<type>` 相同的规则。`<request_fieldname>`、`<response_fieldname>` 和 `<feedback_fieldname>` 遵循与消息字段 `<fieldname>` 相同的规则。

例如，`Fibonacci` 动作的定义如下：

```text
int32 order
---
int32[] sequence
---
int32[] sequence
```

在这个动作定义中，动作客户端发送一个 `int32` 字段，表示要计算的斐波那契数列步数，并期望动作服务端返回一个包含完整计算序列的 `int32` 数组。计算过程中，动作服务端也可以提供一个中间的 `int32` 数组，包含截至某一时刻已经计算出的序列。
