!!! note "注意"
    `rclcpp/rclcpp.hpp` 是一个为方便使用而提供的汇总头文件。它会一次性引入整个 `rclcpp` API，包括节点、发布者、订阅、服务、定时器、参数、执行器、频率控制、等待集等。因此，包含它的每个翻译单元在编译时都要处理一些从未使用的功能。

    在教程之外，建议仅包含实际使用的 API 所对应的头文件。例如，`rclcpp::Node` 声明于 `rclcpp/node.hpp`，`rclcpp::spin` 声明于 `rclcpp/executors.hpp`，而 `rclcpp::init` 和 `rclcpp::shutdown` 声明于 `rclcpp/utilities.hpp`。对于不会创建节点或调用 spin 的翻译单元，例如仅需 `rclcpp/qos.hpp` 或 `rclcpp/time.hpp` 这类小型头文件的头文件、插件和辅助库，这样做带来的节省最为明显，因为 `rclcpp/node.hpp` 和 `rclcpp/executors.hpp` 本身也很大。`rclcpp/rclcpp.hpp` 基本上就是这些头文件的集合，因此可以从它入手，确定自己真正需要包含哪些头文件。
