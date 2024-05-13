# LLM Automated Fine-Tuning with Gradio

This project is aimed at fine-tuning Large Language Models (LLMs) using Gradio, a library for creating web-based UIs for machine learning models. The fine-tuning process is optimized for efficiency and reduced memory consumption with the help of the **unsloth** framework.

## Prerequisites

Before running the code, ensure you have the necessary dependencies installed. You can install them using pip:

```bash
pip install "unsloth[colab-new] @ git+https://github.com/unslothai/unsloth.git"
pip install --no-deps "xformers<0.0.26" trl peft accelerate bitsandbytes
pip install gradio
```

## Usage

The project consists of two main parts:

1. **Fine-tuning (1):** This notebook is used to fine-tune LLMs. It automates the process using the unsloth framework and follows a path with various steps namely:

- **Get dataset**: Choose a dataset from a list of 4 DPO-like datasets hosted on Hugging Face, define the maximum number of samples to keep, as weel as the test size when performing train tes split. Additionnaly, each sample is formatted as follows:

```
prompt:
    <|user|>\n
    [prompt]</s>\n
    <|assistant|>\n

chosen response:
    [chosen]</s>\n

rejected response:
    [rejected]</s>\n
```

**Note**: One randomly chosen data sample can be viewed on the UI when opening the dropdown component under `Get the data`. This requires the pipeline to have completely run (we're trying to show that directly after data upload).

- **Get model**: Choose a model from a provided list including 4 LLama-based, 6 Mistral-based, and 3 Gemma-bases models.

- **Get PEFT model**: Parameter-Efficient Fine-Tuning (PEFT) methods enable efficient adaptation of large pretrained models to various downstream applications by only fine-tuning a small number of (extra) model parameters instead of all the model's parameters. We're taking advantage of this in our project using Low-Rank Adaptation (LoRA). 

- **Get Directed Preference Optimization (DPO) trainer**: Our fine-tuning process relies entirely on this DPO approach which aims to align LLMs with human preference. It's a Reinforcement Learning from Human Feedback (RLHF) method that implies both accepted and rejected responses in the fine-tuning loop.

- **Fine-tune the model**: Here's where all the magic happens. With all the previous configuration set, we're now able to concretely fine-tune our base model on the selected data.

- **Save model**: The new model is then pushed to the Hugging Face Hub, allowing anyone to load, run inference, on even re-fine-tune it!

- **Get training metrics**: The last node in the pipeline consists in retrieving the metrics for the fine-tuning phase: training time, number of epochs, training loss, ... TODO: list all metrics

**Note**: Non-interactive and hidden Gradio components
As you may have noticed, some hyper-parameters and config stuff can't be modified when running the app. This is because we somehow wanted to freeze some risky values whose modification usually caused server crashes when exexuting the pipepline on our Google Colab free plan. For similar reasons, some used parameters doesn't show on the UI just because they're hidden. Feel free to carry out your own experiments, you can make Gradio components interactive by setting `interactive = True`, and make them visible with `visible = True`.

2. **Inference (2):** This notebook utilizes the fine-tuned model for inference using a Gradio chatbot.

**Note**: Sorry for the use of separate notebooks for training and inference, we weren't able to tweak the chatbot block in order to make it load the model and tokenizer from the hub after the fine-tuning has completed.
