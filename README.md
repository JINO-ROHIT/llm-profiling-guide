# llm-profiling-guide

this is my personal guide to profiling kernels and then bottlenecks in a llm. we will first start with a dense model and then scale up to latest model architectures.


### understand the architecture


lets take qwen3 14b as a example.

in bf16, the weights alone make up = 14B * 2 bytes = 28 GB


for a batch size 1

1. theoretical bandwith roof

BW = 1792 GB/s
bandwith limited throughput = 1792 GBs / 28GB =  64 tokens/s

(this only includes active params * bytes, not kv cache.)

2. theoretical compute roof

a rough estimate for flops/token ~ 2P
so for our model = 2 * 14B = 28 GFLOPs
peak tensor throughput = 1 pflop/s = 1_000_000 gflop/s

compute limited throughput = 1_000_000 / 28 = 35k tokens/s

64 < 35k, memory bound.
