# profile sglang server with torch profiler

this is my personal guide to profiling sglang server using the in built profiler.


### set your sglang flags and launch the server 

```bash
sglang serve \
  --model-path Qwen/Qwen3.5-0.8B
```

### start the profiler


# Wait 5 steps (warmup), then profile for 2 steps

 curl -X POST http://127.0.0.1:30000/start_profile \
    -H "Content-Type: application/json" \
    -d '{
      "output_dir": "/workspace/traces",
      "start_step": 5,
      "num_steps": 2,
      "activities": ["CPU", "GPU"],
      "profile_by_stage": true,
      "record_shapes": true,
      "with_stack": true
    }'

# use 1 prompt to confirm if num steps is 2 forward passes or 2 queries

python -m sglang.bench_serving --backend sglang --num-prompts 1
