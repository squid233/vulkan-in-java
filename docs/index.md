# Vulkan in Java

众所周知，Java 不适合编写游戏（Minecraft：？）。然而在 C++ 中处理字符串和内存之类的实在太痛苦，所以我编写了这套教程来教你如何结合 Java 的 [FFM API](https://openjdk.org/jeps/454) 使用 Vulkan，从而提高程序的运行效率。

本教程重点关注：

1. 利用 FFM API 实现与本机函数的交互
2. OverrunGL 一些 API 的用法

至于 Vulkan 中的概念就看其他教程吧。

本教程源码可在[这里](https://github.com/squid233/vulkan-in-java-src)找到。

## 啥是 OverrunGL？

请看[这里](https://github.com/Over-Run/overrungl)。简单来说就是像 LWJGL 一样能让你调用 C 函数的库。

## 参考资料

- [OverrunGL Javadoc](https://over-run.github.io/overrungl)
- [Vulkan References](https://registry.khronos.org/vulkan/specs/latest/man/html/)
- [Vulkan Tutorial](https://vulkan-tutorial.com/Introduction)：英文教程
- [EasyVulkan](https://easyvulkan.github.io/)：中文教程

```mermaid
flowchart TB
 subgraph s1["Physical devices"]
        n1["Physical device 0"]
        n2["Physical device 1"]
        n7["Queue families"]
  end
 subgraph s3["Command buffers"]
        n20["Begin"]
        n21["Record"]
        n22["End"]
  end
 subgraph s4["Queues"]
        n23["Present"]
        n24["Submit"]
  end
 subgraph s2["Subpasses"]
    direction LR
        n14["Subpass 0"]
        n15["Subpass 1"]
  end
 subgraph s5["Render passes"]
        s2
  end
 subgraph s6["Vertex bindings"]
        n25["Binding 0"]
        n28["Binding 1"]
        n29["Binding 2"]
  end
 subgraph s7["Vertex attributes"]
        n27["Attribute 0"]
        n30["binding=0<br>location=0"]
        n31["Attribute 1"]
        n32["binding=0<br>location=1"]
        n33["Attribute 2"]
        n34["binding=2<br>location=3"]
  end
 subgraph s8["Swap chain recreation"]
        n9["Swap chain"]
        n17["Framebuffers"]
        n39["Images"]
        n40["Image views"]
  end
 subgraph s9["Pipelines"]
        s11["s11"]
        n42["Compute pipelines"]
  end
 subgraph s10["Vertex input assembly"]
        s6
        s7
  end
 subgraph s11["Graphics pipeline"]
        s10
  end
    VkInstance["Instance"] -- <br> --> s1
    n1 --> n7
    n1 -- <br> --> n5["Device"]
    n7 --> n5
    VkInstance --> n8["Surface"]
    n8 --> n9
    n5 --> n10["Shader modules"] & n11["Pipeline layouts"] & n18["Command pools"] & s4 & s5 & n35["Buffers"] & n36["Descriptor set layouts"] & n37["Descriptor pools"] & n39 & n41["Samplers"] & n9 & s9
    n18 --> s3
    n20 --> n21
    n21 --> n22
    n17 --> n20
    s5 --> n17 & n20 & s11
    n24 --> n23
    s3 --> n24
    n27 --> n30
    n30 --> n25
    n31 --> n32
    n32 --> n25
    n33 --> n34
    n34 --> n29
    n35 --> n21 & s6
    n14 --> n15
    n37 --> n38["Descriptor sets"]
    n36 --> n38 & n11
    n38 --> n21
    n9 --> n39
    n39 --> n40
    n35 -.-> n38
    n40 -.-> n38
    n41 -.-> n38
    n11 --> s9
    n10 --> s9
    s9 --> s3
    n40 --> n17
    n30@{ shape: braces}
    n32@{ shape: braces}
    n34@{ shape: braces}
```
