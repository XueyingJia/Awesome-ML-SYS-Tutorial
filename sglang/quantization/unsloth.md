# Unsloth background
Unsloth是一个开源的大模型训练和推理加速项目，通过将模型参数量化，大幅提升模型训练速度以及降低显存占用，使得更多的开发者能够在有限的计算资源下进行大模型微调和推理。Unsloth能在不牺牲模型performance的前提下，显著提高训练效率和降低资源消耗。

<!-- # 技术原理 TODO:（double check）
Unsloth使用OpenAI的Triton编程语言对模型的计算过程进行重写，手动实现反向传播引擎。这种方法相比传统的PyTorch实现有几个显著优势：
- 能够针对GPU特性进行更细粒度的优化
- 实现更高效的内存访问模式
- 减少不必要的中间结果存储 -->

# 与HuggingFace的集成
Unsloth设计了与HuggingFace生态系统的无缝集成接口，用户只需将模型加载时的AutoModelForCausalLM替换为FastLanguageModel，其他训练代码几乎不需要修改。 Unsloth主要用于加速训练，但是unsloth产生的ckpt中的参数是量化过的参数，所以在推理的时候也要提供相应的支持。

# Quantization Techniques
unsloth一般使用bitsandbytes做quantization，quantization的具体实现主要分为两种方式：standard quantization和dynamic quantization。unsloth以结合lora和qlora的技术著称。

## Standard Quantization
- 基于bitsandbytes库实现
- 能够显著减少显存使用
- 适用于较大的模型（8B及以上），但对于较小模型可能导致精度损失

## Dynamic Quantization
- background：quantizing a model down to lower bits sometimes breaks the model entirely
- object：通过quantize model不同的部分， by analyzing activation quantization error and weight quantization error, 保证quantized model的performance和非quantize model的performance最接近的情况下，使得quantized model的size最小
- 每个model的dynamic quantization的具体实现目前unsloth称是机密，无法直接透露每个model的quantization的部分是什么
- 但是unlsoth称vllm已经有支持，猜测是load出来model之后通过参数名字reverse engineer知道要quantize哪些layer （待查证）

## bnb-4b == qlora? (TODO)

## unsloth ckpt的保存形式
