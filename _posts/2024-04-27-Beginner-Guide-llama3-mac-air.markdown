---
layout: post
title:  "Beginner's Guide to Running Llama-3-8B on a MacBook Air"
date:   2024-04-27 09:33:12 -0400
categories: mlx-lm
published: true
---
![lm](/assets/llama3_mac/llama_mac.png)

### [link to the notebook](https://github.com/xueerchen1990/blog_notebooks/blob/main/begginer_guide_llama3_mlx_macbook_air/llama3-8B-mlx-macbook-air.ipynb)

## Introduction:
Are you excited to explore the world of large language models on your MacBook Air? In this blog post, we'll walk you through the steps to get Llama-3-8B up and running on your machine. We'll also share a recent discovery that improves the model's responses by applying a templating fix, which is especially crucial when working with low-precision quantized models like the 4-bit Llama-3-8B used in this example. Low-precision quantization reduces the memory footprint and computational requirements of large language models, enabling faster inference on resource-constrained devices like the MacBook Air. However, this quantization may result in a loss of expressiveness and fine-grained detail capture. Templating compensates for this by providing a well-structured context for the model to follow, guiding its responses and helping it generate more coherent and relevant outputs. Let's dive in!

## Hardware:
For this experiment, I used a MacBook Air 15" with an M2 chip and 16GB of memory. Unfortunately, I was unable to run the model on my 8GB Mac mini. If you have a Mac mini and are looking for a model that can run comfortably on it, don't worry! You can try [phi3-mini](https://huggingface.co/mlx-community/Phi-3-mini-4k-instruct-4bit), which is a smaller model that works well on a 8GB Mac.

![lm](/assets/llama3_mac/usage.png)

## Setting Up the Environment:
1. Make sure you have Python installed on your MacBook Air. I recommend using a virtual environment such as [mamba miniforge](https://github.com/conda-forge/miniforge?tab=readme-ov-file#miniforge3) to keep your dependencies isolated.
2. Install the required libraries: `pip install mlx-lm torch`
3. Download the pre-trained Llama-3-8B model and tokenizer. In this example, I'll be using the "mlx-community/Meta-Llama-3-8B-Instruct-4bit" model. The model weights are 5GB.

## Loading the Model and Tokenizer:
1. Use the `load` function from `mlx_lm` to load the pre-trained model and tokenizer:
   ```python
   from mlx_lm import load, generate
   model, tokenizer = load("mlx-community/Meta-Llama-3-8B-Instruct-4bit")
   ```
2. This step might take a few minutes depending on your internet connection and machine specs.

## Generating Responses:
1. To generate responses from the model, you can use the `generate` function from `mlx_lm`:
   ```python
   response = generate(model, tokenizer, prompt="Your prompt here")
   ```
2. Replace "Your prompt here" with the text you want the model to generate a response for.

## Applying the Templating Fix:
1. A recent discovery I made is that the default [example code of mlx-lm llama3 models doesn't have proper templating](https://huggingface.co/mlx-community/Meta-Llama-3-8B-Instruct-4bithttps://huggingface.co/mlx-community/Meta-Llama-3-8B-Instruct-4bit). By applying a templating fix can significantly improve the model's responses.
2. Instead of directly passing the prompt to the `generate` function, I'll use the tokenizer's `apply_chat_template` method to format the input:
   ```python
   messages = [
       {"role": "system", "content": "You are a friendly chatbot."},
       {"role": "user", "content": "Hello, what's your name?"},
   ]
   input_ids = tokenizer.apply_chat_template(
       messages,
       add_generation_prompt=True,
       return_tensors="pt"
   )
   ```
3. The `apply_chat_template` method formats the messages into a template that the model understands. However, it returns a list of token IDs (integers) rather than a string.
4. To pass the templated input to the `generate` function, I need to decode the token IDs back into a string using the tokenizer's `decode` method:
   ```python
   prompt = tokenizer.decode(input_ids[0].tolist())
   response = generate(model, tokenizer, prompt=prompt)
   ```
5. By decoding the token IDs, I restore the templated input to its string representation, which can be directly passed to the `generate` function.

## The Power of Templating:
Let's compare the responses with and without the templating fix for the prompt $"Hello, what's your name?"$.

- Without templating, the model generates a response about code:
   ```
   ")\n\n    # Check if the user's message is in the list of expected messages\n    if message in expected_messages:\n        # If the user's message is in the list of expected messages, respond with a friendly message\n        response = "Hello! My name is Chatbot. I'm here to help you with any questions or concerns you may have. How can I assist you today?"\n    else:\n        # If the user's message is not in the list of expected messages, respond with
   ```
- With the templating fix, the model generates a more appropriate response:
   ```
   Hello! My name is Chatty, and I'm a friendly chatbot. I'm here to help answer your questions, provide information, and have a nice conversation with you. How are you today?
   ```
The templating fix helps the model understand the context and generate more relevant and coherent responses.

## Conclusion:
Running Llama-3-8B on your MacBook Air is a straightforward process. By applying the templating fix and properly decoding the token IDs, you can significantly improve the model's responses and engage in more natural conversations. The apply_chat_template method from the tokenizer is particularly beneficial for low-precision quantized models like the 4-bit Llama-3-8B, as it formats the input messages into a template that the model can better understand and follow, mitigating the limitations imposed by the reduced precision. Remember that the `generate` function expects the prompt argument to be a string, so make sure to decode the output of `apply_chat_template` before passing it to generate. Experiment with different prompts and have fun exploring the capabilities of this powerful language model!
