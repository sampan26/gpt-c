# gpt-c

LLM training in simple, pure C/CUDA. There is no need for 245MB of PyTorch or 107MB of cPython. For example, training GPT-2 (CPU, fp32) is ~1,000 lines of clean code in a single file. It compiles and runs instantly, and exactly matches the PyTorch reference implementation. I chose GPT-2 as the first working example because it is the grand-daddy of LLMs, the first time the modern stack was put together.

Currently, I am working on:

- direct CUDA implementation, which will be significantly faster and probably come close to PyTorch.
- speed up the CPU version with SIMD instructions, AVX2 on x86 / NEON on ARM (e.g. Apple Silicon).
- more modern architectures, e.g. Llama2, Gemma, etc.

# quick start

Download and tokenize a dataset. The tinyshakespeare dataset is the fastest to download and tokenize:
The .bin files are raw byte streams of int32 numbers indicating the token ids with the GPT-2 tokenizer. 

In principle we'd be ready to the train the model right here. However the baseline CPU/fp32 reference code is so inefficient that it's not practical to train these models from scratch yet. Instead, we initialize with the GPT-2 weights released by OpenAI and just do finetuning. For that, we have to download the GPT-2 weights and save them as a checkpoint we can load in C:

