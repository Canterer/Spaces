https://github.com/ollama/ollama/releases/latest/download/OllamaSetup.exe

自定义目录：
D:\SoftSpace\Ollama>./OllamaSetup.exe /DIR=D:\SoftSpace\Ollama

模型默认位置： C:\Users\YCK\.ollama\

设置环境变量 重启生效
OLLAMA_MODELS		D:\ollama\		模型文件存放目录
OLLAMA_HOST		0.0.0.0			Ollama服务监听的网络地址，默认127.0.0.1
OLLAMA_ORIGINS		*			HTTP客户端请求来源，半角逗号分隔列表
OLLAMA_PORT		11438			Ollama服务监听的默认端口，默认为11438

OLLAMA_KEEP_ALIVE	5m	大模型加载到内存中后存活的时间，300代表300秒，任何负数表示一直存货，默认5m即五分钟
OLLAMA_DEBUG	1	输出Debug日志，应用研发阶段设置为1
OLLAMA_NUM_PARALLEL	请求处理的并发数量，默认为1
OLLAMA_MAX_QUEUE	请求队列长度，默认为512

模型地址：
https://ollama.com/search	查看大模型列表

https://ollama.com/library/deepseek-r1	Deepseek-r1 大模型
选择7b参数大小，拷贝运行命令：ollama run deepseek-r1 ，查看文件标识0a8c26691023 · 4.7GB


Ctrl+C组合键 中断下载



1. DeepSeek模型概述

DeepSeek模型是完全开源的，采用MIT协议，不存在任何付费下载模型的说法。用户只需在DeepSeek官网注册，即可免费使用DeepSeek-V3和DeepSeek-R1系列模型。官方API价格也非常亲民，适合个人开发者和小型企业使用。


本次开源的DeepSeek-R1系列包含8个不同模型，具体如下：

- DeepSeek-R1-671B

- DeepSeek-R1-Zero-671B（较为特殊，不建议使用）

- DeepSeek-R1-Distill-Llama-70B

- DeepSeek-R1-Distill-Qwen-32B

- DeepSeek-R1-Distill-Qwen-14B

- DeepSeek-R1-Distill-Llama-8B

- DeepSeek-R1-Distill-Qwen-7B

- DeepSeek-R1-Distill-Qwen-1.5B

请注意：除了DeepSeek-R1-671B，其余模型都不是“正牌”R1模型，而是基于蒸馏技术的衍生版本。




2. 模型参数与性能的关系

在这里，给各位第一次接触语言模型的同学科普一下模型参数量与模型性能与硬件占用的相关知识。

2.1 模型参数量

模型参数量可以简单理解为模型神经网络的权重总大小。从模型名称中的“B”（Billion）可以看出模型的参数量。例如：

- DeepSeek-R1-671B：参数量为6710亿。

- DeepSeek-R1-Distill-Qwen-1.5B：参数量仅为15亿。

简单来说就是模型的脑容量大小。


2.2 参数与性能的关系

一般来说，在同结构和训练模式下，参数量越大，模型性能越好。大参数模型的性能提升是全方位的，包括：

- 知识面更广：能够处理更多样化的问题。

- 幻觉概率降低：生成的内容更加准确和可靠。

- 逻辑性更强：在复杂推理任务中表现更好。

- 推理能力更优：能够更好地理解和解决逻辑问题。


例如，DeepSeek-R1-671B的性能远远优于DeepSeek-R1-Distill-Qwen-1.5B，因为两者的参数量差距高达400倍。


2.3 参数与硬件占用的关系

模型参数量越大，对硬件的要求越高。以下是部分模型的显存需求：

- DeepSeek-R1-671B：需要接近1400GB显存才能加载原始权重。

- DeepSeek-R1-Distill-Qwen-1.5B：仅需6GB显存即可加载。


2.4 模型量化

很多营销号都会说，用他们提供的模型，只需要非常小的显存就可以加载，这是因为他们用的是量化后的模型。

量化是通过压缩模型计算精度来降低硬件需求的技术。常见的量化级别包括1-8bit：

- 8bit量化：性能损失约2%-5%。

- 4bit量化：性能损失约10%-15%。

- 低于4bit量化：性能损失较大，通常不推荐使用。


量化后的模型可以在较低显存下运行，但性能会有所下降。例如：

- 7B模型：原始权重需要16GB显存，4bit量化后仅需8GB显存。

- 14B模型：原始权重需要26GB显存，4bit量化后仅需16GB显存。




3. 模型蒸馏技术

除了DeepSeek-R1-671B，其他模型都带有“Distill”标签，表示它们是蒸馏模型。蒸馏模型通过技术手段从大模型（教师模型）中学习知识，并将其迁移到小模型（学生模型）中。


在DeepSeek-R1系列中，671B的R1是教师模型，其他蒸馏模型基于各自的底模（如Llama、Qwen）对R1进行了蒸馏，使其具备类似R1的思维链推理能力。

然而，蒸馏模型的性能受限于其原始底模的参数量与性能，虽然推理能力有所提升，但其他方面（如知识面、通用能力）可能会出现劣化。


重要提示：不要轻信“本地部署R1”的说法，因为这些本地部署的模型通常是蒸馏版本，性能远不及“正统”R1-671B。个人能部署的玩意都是严重劣化的蒸馏模型。




4. 部署建议

4.1 硬件需求

原始权重

- 1.5B模型：6GB显存。

- 7B/8B模型：16GB显存。

- 14B模型：26GB显存。

- 32B及以上模型：需要高端GPU，显存需求超过48GB。


所以对于大多数个人用户，建议部署4bit量化模型：

- 7B/8B模型：8GB显存。

- 14B模型：16GB显存。

- 32B模型：22GB显存。

- 70B模型：48GB显存。


4.2 部署平台

- 有UI且简单：LM Studio、Text Generation WebUI。

- 高效率但无UI：Ollama、LLama.cpp、Tabby。




5. 常见问题解答

Q1：模型后缀gguf和exl2是什么意思？

- gguf：支持使用CPU和内存补足显存不足，但非全显存加载时会显著降速（80%以上）。

- exl2：可将模型推理速度提升至最多3倍。

更多详情可以去看我的这期视频： https://b23.tv/LmtGmqz


Q2：如何选择部署平台？

- 如果你是新手，推荐使用LM Studio或Text Generation WebUI，它们提供了友好的用户界面。

- 如果你追求高效率，可以选择Ollama或LLama.cpp，tabby api等


Q3：这些模型的性能和普通模型相比到底如何？

- 你可以理解为这些模型是他们底模模型的推理强化版，在逻辑推理方面的各类任务，14b的那个qwen蒸馏，就可以解决gemma2 27b无法正确推理的逻辑题，但是在其他通用能力上，就完全不行了。这些蒸馏模型除了32b和70b还保留有一定通用性能，其他基本上都是类似情况。

- 建议优先部署32B及以上的模型，或者直接使用在线R1（免费且性能强）。

- 特别注意，R1系列模型不适合进行翻译




6. 总结

通过本文，希望大家能够更好地理解DeepSeek模型，避免被网络上的误导信息所迷惑。




附录：模型参数对比表

| 模型名称         | 参数量 | 原始显存需求 | 4bit量化显存需求 | 性能特点           |

|---------------------------|--------|--------------|------------------|------------------------------|

| DeepSeek-R1-671B     | 671B  | 1400GB    | 别看了，跑不动的    | 全方位高性能         |

| DeepSeek-R1-Distill-Llama-70B | 70B  | 140GB    | 48GB       | 推理能力强，通用性能较好   |

| DeepSeek-R1-Distill-Qwen-32B | 32B  | 64GB     | 22GB       | 推理能力优秀，通用性能中等  |

| DeepSeek-R1-Distill-Qwen-14B | 14B  | 28GB     | 16GB       | 推理能力较强，通用性能较弱  |

| DeepSeek-R1-Distill-Qwen-7B | 7B   | 14GB     | 8GB       | 适合低显存设备，性能有限   |




