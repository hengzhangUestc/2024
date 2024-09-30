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


![image](https://github.com/user-attachments/assets/02d9af38-6414-47a2-a4da-29b20aad7ded)
