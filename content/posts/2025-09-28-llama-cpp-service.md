---
title: "How to setup llama.cpp as a systemd service"
draft: true
date: "2025-09-28"
description: "How to setup llama.cpp as a systemd service."
tags: ["llama.cpp", "local-llm", "llama-swap", "linux"]
---

I finally found some time and motivation to host my own LLMs on my server - Intel i7 14700F with 80 GiB of RAM and NVIDIA's RTX 4060 Ti 16 GB. Here's a quick post on how you can do it to.

The steps are the following:

1. Install llama.cpp
2. Install llama-swap
3. Create a llama-swap config file
4. Create and enable llamaswap.service

## Install llama.cpp

[llama.cpp](https://github.com/ggml-org/llama.cpp) is known for being a bit cumbersome to setup, especially if you want to run it on your NVIDIA GPU. Since there are no prebuilt binaries, as there are for CPUs, you'll need to mess around with NVIDIA Toolkit a bit to get everything up-and-running. I found the following guide more than enough to set everything up - https://blog.steelph0enix.dev/posts/llama-cpp-guide/.

## Install llama-swap

As opposed to llama.cpp, setting up [llama-swap](https://github.com/mostlygeek/llama-swap) is easy. Download the prebuilt binaries and add them to a folder that is in your PATH. For me this is `.local/bin`. Then, you should be able to run `llama-swap -h`.

## Create llama-swap config

You need to create the necessary llama-swap config file. This config file determines how llama-swap should behave. Here's a current version of my (relatively messy) config file:

```yaml
healthCheckTimeout: 60

logLevel: info

metricsMaxInMemory: 200

startPort: 8080

models:
  "qwen3":
    # cmd: the command to run to start the inference server.
    cmd: |
      llama-server -hf unsloth/Qwen3-30B-A3B-Instruct-2507-GGUF --ctx-size 32768 --jinja -ub 2048 -b 4096 --host 0.0.0.0 --port 8081 --temp 0.7 --top-p 0.8 --min-p 0.0 --top-k 20 -ngl 32

    # name: a display name for the model
    name: "Qwen3 30B A3B Instruct"

    # proxy: the URL where llama-swap routes API requests
    proxy: http://127.0.0.1:8081

  "qwen3-coder":
    # cmd: the command to run to start the inference server.
    cmd: |
      llama-server -hf unsloth/Qwen3-Coder-30B-A3B-Instruct-GGUF --ctx-size 32768 --jinja -ub 2048 -b 4096 --host 0.0.0.0 --port 8082 --temp 0.7 --top-p 0.8 --min-p 0.0 --top-k 20 -ngl 32

    # name: a display name for the model
    name: "Qwen3 Coder 30B A3B Instruct"

    # proxy: the URL where llama-swap routes API requests
    proxy: http://127.0.0.1:8082

groups:
  # group1 works the same as the default behaviour of llama-swap where only one model is allowed
  # to run a time across the whole llama-swap instance
  "standard":
    # swap: controls the model swapping behaviour in within the group
    swap: true

    # exclusive: controls how the group affects other groups
    exclusive: true

    # members references the models defined above
    members:
      - "qwen3"
      - "qwen3-coder"
```

I host a llama-swap proxy server on port 8080 and host each model on a seprate port. This is not necessary since I cannot have two models loaded in the GPU at the same time, but this just makes the setup a bit cleaner in my opinion.

Also, you'll need to play around with the `llama-server` a bit to see which models you can run.

## Create and enable llamaswap.service

And finally you need to create the `llamaswap.service` file. Run the `sudo nano /etc/systemd/system/llamaswap.service` and enter the following:

```service
[Unit]
Description=Start llama-swap service
After=network.target

[Service]
Type=simple
User=<your-user>
WorkingDirectory=<path/to/llama-swap-config>
ExecStart=</path/to/>llama-swap -config </path/to/>llamaswap.yaml
Restart=on-failure
RestartSec=10

[Install]
WantedBy=multi-user.target
```

You might need to enter the additional environment variables if your llama.cpp binaries are not in the `.local/bin` and if your llama.cpp cache is also somewhere else:

```service
Environment=PATH=/path/to/llama.cpp/build/bin
Environment=LLAMA_CACHE=/path/to/llamacpp_cache
```

Add these just after WorkingDirectory entry.

And that is it. You need to run the following:

```bash
sudo systemctl daemon-reload
sudo systemctl enable llamaswap.service
sudo systemctl start llamaswap.service
sudo systemctl status llamaswap.service
```

You should see the something like:

```bash
● llamaswap.service - Start llama-swap service
     Loaded: loaded (/etc/systemd/system/llamaswap.service; enabled; preset: en>
     Active: active (running) since Sun 2025-09-28 07:20:51 UTC; 42min ago
   Main PID: 1053 (llama-swap)
   ...
```

You should now be able to access the `http://localhost:8080` and see the llama-swap ui.