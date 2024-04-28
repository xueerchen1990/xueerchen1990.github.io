---
layout: post
title:  "Beginner's Guide to Running Llama-3-8B on a MacBook Air"
date:   2024-04-27 09:33:12 -0400
categories: mlx-lm
published: true
---
## Introduction:
Are you excited to explore the world of large language models on your MacBook Air? In this blog post, we'll walk you through the steps to get Llama-3-8B up and running on your machine. We'll also share a recent discovery that improves the model's responses by applying a templating fix. Let's dive in!

## Setting Up the Environment:
1. Make sure you have Python installed on your MacBook Air. We recommend using a virtual environment to keep your dependencies isolated.
2. Install the required libraries, such as `mlx_lm` and `transformers`, using pip or conda.
3. Download the pre-trained Llama-3-8B model and tokenizer. In this example, we'll be using the "mlx-community/Meta-Llama-3-8B-Instruct-4bit" model.

## Loading the Model and Tokenizer:
1. Use the `load` function from `mlx_lm` to load the pre-trained model and tokenizer:
   ```python
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
1. A recent discovery has shown that applying a templating fix can significantly improve the model's responses.
2. Instead of directly passing the prompt to the `generate` function, we'll use the tokenizer's `apply_chat_template` method to format the input:
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
4. To pass the templated input to the `generate` function, we need to decode the token IDs back into a string using the tokenizer's `decode` method:
   ```python
   prompt = tokenizer.decode(input_ids[0].tolist())
   response = generate(model, tokenizer, prompt=prompt)
   ```
5. By decoding the token IDs, we restore the templated input to its string representation, which can be directly passed to the `generate` function.

## The Power of Templating:
1. Let's compare the responses with and without the templating fix for the prompt "Hello, what's your name?".
2. Without templating, the model generates a response about code:
   ```
   ")\n\n    # Check if the user's message is in the list of expected messages\n    if message in expected_messages:\n        # If the user's message is in the list of expected messages, respond with a friendly message\n        response = "Hello! My name is Chatbot. I'm here to help you with any questions or concerns you may have. How can I assist you today?"\n    else:\n        # If the user's message is not in the list of expected messages, respond with
   ```
3. With the templating fix, the model generates a more appropriate response:
   ```
   Hello! My name is Chatty, and I'm a friendly chatbot. I'm here to help answer your questions, provide information, and have a nice conversation with you. How are you today?
   ```
4. The templating fix helps the model understand the context and generate more relevant and coherent responses.

## Conclusion:
Running Llama-3-8B on your MacBook Air is a straightforward process. By applying the templating fix and properly decoding the token IDs, you can significantly improve the model's responses and engage in more natural conversations. Remember that the `generate` function expects the `prompt` argument to be a string, so make sure to decode the output of `apply_chat_template` before passing it to `generate`. Experiment with different prompts and have fun exploring the capabilities of this powerful language model!