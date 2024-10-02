## 量化

### GPTQ

GPTQ对某个block内所有的参数逐个量化，每个参数量化后，需要适当调整这个block内其他未量化的参数，以弥补量化造成的精度损失。

GPTQ 量化工具是[AutoGPTQ](https://github.com/AutoGPTQ/AutoGPTQ)，它可以量化任何 Transformer 模型。

Huggingface 已经将 AutoGPTQ 集成到了 Transformers 中，具体的使用方法可以[参考这里](https://huggingface.co/blog/zh/gptq-integration)。

![image](https://github.com/user-attachments/assets/0476115b-f1ca-4b92-84fe-d0fa60affded)

GGML 之前要先说下[llama-cpp](https://github.com/ggerganov/llama.cpp)这个项目，它是开发者 Georgi Gerganov 基于 Llama 模型手撸的纯 C/C++ 版本，它最大的优势是可以在 CPU 上快速地进行推理而不需要 GPU。然后作者将该项目中模型量化的部分提取出来做成了一个模型量化工具：GGML，项目名称中的GG其实就是作者的名字首字母。
GGUF其实是 GGML 团队增加的一个新功能，GGUF 与 [GGML](https://github.com/ggerganov/ggml) 相比，GGUF 可以在模型中添加额外的信息，而原来的 GGML 模型是不可以的，同时 GGUF 被设计成可扩展，这样以后有新功能就可以添加到模型中，而不会破坏与旧模型的兼容性。

GPTQ与GGML异同：
* GPTQ 在 GPU 上运行较快，而 GGML 在 CPU 上运行较快
* 同等精度的量化模型，GGML 的模型要比 GPTQ 的稍微大一些，但是两者的推理性能基本一致
* 两者都可以量化 HuggingFace 上的 Transformer 模型

### AWQ

[原始论文](https://arxiv.org/abs/2306.00978) 
[开源链接](https://github.com/mit-han-lab/llm-awq)

AWQ是一种类似于 GPTQ 的量化方法。AWQ 和 GPTQ 之间有几个区别，但最重要的区别是 AWQ 假设并非所有权重对 LLM 的性能都同等重要。在量化过程中，不会对所有权重进行量化；相反，只会量化对于模型保持有效性不重要的权重。因此，他们的论文提到与 GPTQ 相比，它们在保持类似甚至更好性能的同时实现了显著的加速。

![image](https://github.com/user-attachments/assets/02d9af38-6414-47a2-a4da-29b20aad7ded)

[autoawq实操内容](https://github.com/casper-hansen/AutoAWQ)

![image](https://github.com/user-attachments/assets/7f0645cc-1653-48f8-bf7d-c9bbeb94951e)


### 量化选择  

1. 8bit 量化损失较小。 
2. AWQ 4bit量化对8B模型来说有2%性能损失，对70B模型只有0.05%性能损失，模型越大损失越小。
3. 参数越大的模型，低bit量化损失越低。AWQ 3bit 70B 也只有2.7%性能损失。 
4. 如果追求几乎无损的性能损失，8B模型用8bit量化，70B模型用4bit量化；如果能接受2-3%损失，8B模型用4bit量化，70B模型用3bit量化。 
5. 有现有量化模型，优先用AWQ，loss更低。
无现在量化模型，优先使用GPTQ量化，量化成本比AWQ低，若追求准确性（loss低）可选择AWQ量化




72B    16位的 ，2 个字节，  134 

72B    8位      1个字节    62
 
6B     16位的    2个字节 ，  12








