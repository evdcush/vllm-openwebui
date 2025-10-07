

# Model-unique errors

## "Custom code"
> *`The repository nvidia/NVIDIA-Nemotron-Nano-9B-v2 contains custom code which must be executed to correctly load the model.`*

What does it mean?

It means this model has code that lives on the hub (the repo in the hub), rather than natively in the transformers library.
To run models with custom code, **you must provide the flag**:
`--trust-remote-code`

or else the model will not be successfully loaded by vllm!

Models with this flag follow this library feature:
https://huggingface.co/docs/transformers/custom_models

In all cases I've observed, there is no shady stuff going on; just some custom ops.
But while you might trust, always confirm!


```
INFO 10-07 05:49:50 [api_server.py:1839] vLLM API server version 0.11.0
INFO 10-07 05:49:50 [utils.py:233] non-default args: {'host': '0.0.0.0', 'model': 'nvidia/NVIDIA-Nemotron-Nano-9B-v2', 'gpu_memory_utilization': 0.85}
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/usr/local/lib/python3.12/dist-packages/vllm/entrypoints/openai/api_server.py", line 1953, in <module>
    uvloop.run(run_server(args))
  File "/usr/local/lib/python3.12/dist-packages/uvloop/__init__.py", line 109, in run
    return __asyncio.run(
           ^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/asyncio/runners.py", line 195, in run
    return runner.run(main)
           ^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/asyncio/runners.py", line 118, in run
    return self._loop.run_until_complete(task)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "uvloop/loop.pyx", line 1518, in uvloop.loop.Loop.run_until_complete
  File "/usr/local/lib/python3.12/dist-packages/uvloop/__init__.py", line 61, in wrapper
    return await main
           ^^^^^^^^^^
  File "/usr/local/lib/python3.12/dist-packages/vllm/entrypoints/openai/api_server.py", line 1884, in run_server
    await run_server_worker(listen_address, sock, args, **uvicorn_kwargs)
  File "/usr/local/lib/python3.12/dist-packages/vllm/entrypoints/openai/api_server.py", line 1902, in run_server_worker
    async with build_async_engine_client(
               ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/contextlib.py", line 210, in __aenter__
    return await anext(self.gen)
           ^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.12/dist-packages/vllm/entrypoints/openai/api_server.py", line 180, in build_async_engine_client
    async with build_async_engine_client_from_engine_args(
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/contextlib.py", line 210, in __aenter__
    return await anext(self.gen)
           ^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.12/dist-packages/vllm/entrypoints/openai/api_server.py", line 206, in build_async_engine_client_from_engine_args
    vllm_config = engine_args.create_engine_config(usage_context=usage_context)
                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.12/dist-packages/vllm/engine/arg_utils.py", line 1142, in create_engine_config
    model_config = self.create_model_config()
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.12/dist-packages/vllm/engine/arg_utils.py", line 994, in create_model_config
    return ModelConfig(
           ^^^^^^^^^^^^
  File "/usr/local/lib/python3.12/dist-packages/pydantic/_internal/_dataclasses.py", line 123, in __init__
    s.__pydantic_validator__.validate_python(ArgsKwargs(args, kwargs), self_instance=s)
pydantic_core._pydantic_core.ValidationError: 1 validation error for ModelConfig
  Value error, The repository nvidia/NVIDIA-Nemotron-Nano-9B-v2 contains custom code which must be executed to correctly load the model. You can inspect the repository content at https://hf.co/nvidia/NVIDIA-Nemotron-Nano-9B-v2 .
 You can inspect the repository content at https://hf.co/nvidia/NVIDIA-Nemotron-Nano-9B-v2.
Please pass the argument `trust_remote_code=True` to allow custom code to be run. [type=value_error, input_value=ArgsKwargs((), {'model': ...rocessor_plugin': None}), input_type=ArgsKwargs]
    For further information visit https://errors.pydantic.dev/2.11/v/value_error
```
