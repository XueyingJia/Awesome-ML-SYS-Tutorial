# 一些关于quantization的context
- bnb在checkpoint的命名很常见，是bitsandbytes这个库的缩写
- GGUF:  a binary format that is optimized for quick loading and saving of models, making it highly efficient for inference purposes



# quantization method分类方式
1. offline vs online quantization
2. standard vs dynamic quantization
3. 量化感知训练（Quantization Aware Training, QAT） vs 后训练量化（Post-Training Quantization, PTQ）vs 量化感知微调（Quantization-Aware Fine-tuning，QAF）
4. per-layer vs (per-token / per-channel) vs per-group

# 量化形式: per-layer vs (per-token / per-channel) vs per-group
1. per-layer量化，以一层网络为量化单位，每层网络一组量化参数；
2. per-token 和 per-channel量化，以一行或一列尾量化单位，每行或每列一组量化参数；for example, X*W, per-token是输入矩阵X的每一行，per-channel是矩阵W的每一列
3. per-group，基于per-token和per-channel，instead of只有一行一列，变成N行或者N列为一组，共享一组参数
<!-- TODO: bitsandbytes use which method -->

# offline vs online quantization
## 常见 offline quantization methods
Bitsandbytes
## 常见 online quantization methods
AWQ, GPTQ, Bitsandbytes, marlin

# QAT vs PTQ vs QAF
## QAF
发生在预训练
## QAT
发生在微调
QLoRA
## PTQ
与训练无关，进一步分为两种：量化weight，量化weight + activation；前者无法提高推理速度，后者可以提高推理速度
只量化weight：在推理的时候dequantization会float32计算，实际计算量并没有减少；
代表工作：LLM.int8()
量化weight + activation：#TODO
GPTQ, AWQ

