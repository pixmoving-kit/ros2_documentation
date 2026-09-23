---
translation_status: machine_translated
source: Tutorials/Advanced/Allocator-Template-Tutorial.rst
---

<span id="implementing-a-custom-memory-allocator"></span>

# 实现自定义内存分配器

**目标：** 此教程将显示在写入 ROS 2 C++ 代码时如何使用自定义内存分配器 。

**教程级别：** 高级

**用时：** 20分钟

此教程会教你如何为出版商和订阅者集成自定义的分配器, 这样默认的堆积分配器将不会在 ROS 节点执行时被调用 。 此教程的代码是可用的 [这儿](https://github.com/ros2/demos/blob/rolling/demo_nodes_cpp/src/topics/allocator_tutorial.cpp).

<span id="background"></span>

## 背景

假设你想写实时安全代码, 并且你已经听说过在实时关键区段使用“新”的多种危险, 因为大多数平台上的默认堆积分配器是非决定性的。

默认情况下,许多 C++ 标准库结构会随着内存的增长而隐含分配,例如 `std::vector`然而,这些数据结构也接受“ Allocator” 模板参数。如果您为其中一个数据结构指定了自定义的 ocator , 它会用该 ocator 代替系统 ocator 来生长或缩小数据结构。 您的自定义 ocator 可以在堆栈上预先配置一个内存库, 这可能更适合实时应用 。

在 ROS 2 C++ 客户端库(rclcpp) 中,我们正在遵循一个类似于 C++ 标准库的哲学。 发布者、 订阅者和执行者接受一个控制该实体在执行期间分配的分配符模板参数。

<span id="writing-an-allocator"></span>

## 写一个分配符

要写出一个兼容ROS 2的配位器接口,您的配位器必须兼容C++标准库配位器接口.

C++11 库提供了名为 `allocator_traits`. C++11标准规定,自定义分配符只需要满足一套最低限度的要求,用于以标准方式分配和处理内存. `allocator_traits` 是一种通用结构,它根据一个写有最低要求的分层器来填充分层器的其他品质.

例如,下列关于习惯分配人的声明将满足下列要求: `allocator_traits` (当然,你仍然需要执行本结构中宣布的职能):

``` c++
template <class T>
struct custom_allocator {
  using value_type = T;
  custom_allocator() noexcept;
  template <class U> custom_allocator (const custom_allocator<U>&) noexcept;
  T* allocate (std::size_t n);
  void deallocate (T* p, std::size_t n);
};

template <class T, class U>
constexpr bool operator== (const custom_allocator<T>&, const custom_allocator<U>&) noexcept;

template <class T, class U>
constexpr bool operator!= (const custom_allocator<T>&, const custom_allocator<U>&) noexcept;
```

然后,您可以访问其他功能 和分配器的成员 `allocator_traits` 像这样: `std::allocator_traits<custom_allocator<T>>::construct(...)`

学习有关以下各方面的充分能力: `allocator_traits`,见 <https://en.cppreference.com/w/cpp/memory/allocator_traits> .

然而,一些只有部分C++11支持的编译器,如GCC 4.8,仍然需要分配器执行许多锅炉板代码,以配合矢量和弦等标准库结构,因为这些结构不使用. `allocator_traits` 因此,如果您使用 C++11 部分支持的编译器,您的分配器需要看起来更像:

``` c++
template<typename T>
struct pointer_traits {
  using reference = T &;
  using const_reference = const T &;
};

// Avoid declaring a reference to void with an empty specialization
template<>
struct pointer_traits<void> {
};

template<typename T = void>
struct MyAllocator : public pointer_traits<T> {
public:
  using value_type = T;
  using size_type = std::size_t;
  using pointer = T *;
  using const_pointer = const T *;
  using difference_type = typename std::pointer_traits<pointer>::difference_type;

  MyAllocator() noexcept;

  ~MyAllocator() noexcept;

  template<typename U>
  MyAllocator(const MyAllocator<U> &) noexcept;

  T * allocate(size_t size, const void * = 0);

  void deallocate(T * ptr, size_t size);

  template<typename U>
  struct rebind {
    typedef MyAllocator<U> other;
  };
};

template<typename T, typename U>
constexpr bool operator==(const MyAllocator<T> &,
  const MyAllocator<U> &) noexcept;

template<typename T, typename U>
constexpr bool operator!=(const MyAllocator<T> &,
  const MyAllocator<U> &) noexcept;
```

<span id="writing-an-example-main"></span>

## 写入示例主

一旦您写出了有效的 C++ 分配符, 您必须把它作为共享指针传递给您的出版商, 订阅者, 以及执行者 。

``` c++
auto alloc = std::make_shared<MyAllocator<void>>();
rclcpp::PublisherOptionsWithAllocator<MyAllocator<void>> publisher_options;
publisher_options.allocator = alloc;
auto publisher = node->create_publisher<std_msgs::msg::UInt32>(
  "allocator_tutorial", 10, publisher_options);

rclcpp::SubscriptionOptionsWithAllocator<MyAllocator<void>> subscription_options;
subscription_options.allocator = alloc;
auto msg_mem_strat = std::make_shared<
  rclcpp::message_memory_strategy::MessageMemoryStrategy<
    std_msgs::msg::UInt32, MyAllocator<void>>>(alloc);
auto subscriber = node->create_subscription<std_msgs::msg::UInt32>(
  "allocator_tutorial", 10, callback, subscription_options, msg_mem_strat);

std::shared_ptr<rclcpp::memory_strategy::MemoryStrategy> memory_strategy =
  std::make_shared<AllocatorMemoryStrategy<MyAllocator<void>>>(alloc);
rclcpp::ExecutorOptions options;
options.memory_strategy = memory_strategy;
rclcpp::executors::SingleThreadedExecutor executor(options);
```

您还需要使用您的分配器来分配您通过执行代码路径的任何信件 。

``` c++
auto alloc = std::make_shared<MyAllocator<void>>();
```

一旦你将节点立即切换,

``` c++
uint32_t i = 0;
while (rclcpp::ok()) {
  msg->data = i;
  i++;
  publisher->publish(msg);
  rclcpp::sleep_for(std::chrono::milliseconds(1));
  executor.spin_some();
}
```

<span id="passing-an-allocator-to-the-intra-process-pipeline"></span>

## 将分配器传递到流程内管道

尽管我们在同一个过程中对一个出版商和订户进行了即兴宣传,但我们还没有使用进程内部的管道。

IntraProcessManager 是一个通常对用户隐藏的类,但为了传递自定义的分区符,我们需要通过从 rcpp 上下文获取它来曝光它. IntraProcessManager 使用了多个标准库架构,所以没有自定义的分区符,它会称为默认的新.

``` c++
auto context = rclcpp::contexts::get_global_default_context();
auto options = rclcpp::NodeOptions()
  .context(context)
  .use_intra_process_comms(true);
auto node = rclcpp::Node::make_shared("allocator_example", options);
```

确保即时化出版商和订户AFTER以这种方式构建节点.

<span id="testing-and-verifying-the-code"></span>

## 测试和验证代码

你怎么知道你的定制分配器 居然被叫来?

显而易见的是,应该数一数 给您的传统分配器的电话 `allocate` 财务报告和财务报告 `deallocate` 函数,并将此功能与呼叫进行对比 `new` 财务报告和财务报告 `delete`.

在自定义的分配器中添加计数是容易的:

``` c++
T * allocate(size_t size, const void * = 0) {
  // ...
  num_allocs++;
  // ...
}

void deallocate(T * ptr, size_t size) {
  // ...
  num_deallocs++;
  // ...
}
```

您也可以覆盖全局新建并删除运算符 :

``` c++
void operator delete(void * ptr) noexcept {
  if (ptr != nullptr) {
    if (is_running) {
      global_runtime_deallocs++;
    }
    std::free(ptr);
    ptr = nullptr;
  }
}

void operator delete(void * ptr, size_t) noexcept {
  if (ptr != nullptr) {
    if (is_running) {
      global_runtime_deallocs++;
    }
    std::free(ptr);
    ptr = nullptr;
  }
}
```

我们的增量变量只是全球静态整数 `is_running` 是一个全球性的静态布尔,在呼叫之前被切换到 `spin`.

那个... [示例可执行文件](https://github.com/ros2/demos/blob/rolling/demo_nodes_cpp/src/topics/allocator_tutorial.cpp) 打印变量的值。要运行可执行示例,请使用:

``` bash
$ ros2 run demo_nodes_cpp allocator_tutorial
```

或,用流程内管道运行实例:

``` bash
$ ros2 run demo_nodes_cpp allocator_tutorial intra
Global new was called 15590 times during spin
Global delete was called 15590 times during spin
Allocator new was called 27284 times during spin
Allocator delete was called 27281 times during spin
```

但其余的1/3来自何处?

事实上,这些分配/交易地点来源于本例子中所使用的基本的DDS执行。

证明这不属于此教程的范围, 但你可以检查作为ROS 2 连续集成测试的一部分运行的分配路径的测试, 该测试通过代码追溯, 并计算出某些函数的调用是否起源于 Rmw 执行或 DDS 执行中 :

<https://github.com/ros2/realtime_support/blob/rolling/tlsf_cpp/test/test_tlsf.cpp#L41>

注意,这个测试不是使用我们刚刚创建的自定义分配器,而是TLSF分配器(见下文).

<span id="the-tlsf-allocator"></span>

## TLSF 分配器

ROS 2为TLSF(两层隔离功能)分配器提供支持,该分配器旨在满足实时需求:

<https://github.com/ros2/realtime_support/tree/rolling/tlsf_cpp>

关于TLSF的更多信息,请参见: [本页通过瓦莱尼亚大学(Universitat Politècnica de Vallència)](http://www.gii.upv.es/tlsf/).

请注意,TLSF分配器根据双GPL/LGPL许可证获得许可.

以下是使用 TLSF 分配器的完整实例 : <https://github.com/ros2/realtime_support/blob/rolling/tlsf_cpp/example/allocator_example.cpp>
