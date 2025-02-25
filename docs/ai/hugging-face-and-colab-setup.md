# Running your own LLMs: Hugging Face and Google Colab setup

There are advantages to using LLMs and AI tools locally (i.e. downloading the model to the machine you are working on and using it within a Python environment). One of the main advantages is that it lets you use LLMs with private or sensitive data as you are keeping the data on your machine or within an environment you control. 

The challenge with using AI tools that are not hosted (i.e. not available at chatgpt.com) is that they require some setup and often using dedicated hardware (a GPU). 

One of the easiest ways to get up and running with LLMs, and AI tools in general, is via Hugging Face. Hugging Face is a company that hosts trained models online and provides easy-to-use software tools for working with LLMs. To access models from Hugging Face you need to create an account and generate an access token that lets you authenticate your Python environment (to download models). 

Many LLMs will only run on computers with GPUs and smaller LLMs will be more performant on GPUs. Not all computers have GPUs capable of running LLMs; however, it's possible to access computers in the cloud with GPUs. Google Colab provides free access (with some usage limits) to a cloud computer with a Python environment and a GPU.

This guide demonstrates how to setup Hugging Face and Google Colab to work with GPUs. 

Work through this notebook in Google Colab to setup your environment to download models from Hugging Face and run them using Hugging Face's Python tools:

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-1_0_llms.ipynb" target="_blank">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

### Setup Hugging Face token

**Create a Hugging Face account:**

<img src="https://github.com/geog3300-agri3003/coursebook/raw/main/docs/img/hf-1-annotated.png" alt="HF account" width="100%">

Once, you have created a Hugging Face account you can gain access to models. Many LLMs require you accept terms and conditions. For example, Google's Gemma 2 2b it model is performant, recently trained and is performant on Google Colab instances (2b represents 2 billion parameters and it represents instruction tuned). Agree to the model's terms and conditions and usage policy <a href="https://huggingface.co/google/gemma-2-2b-it" target="_blank">here</a>.

**Create an *Access Token*:**

<img src="https://github.com/geog3300-agri3003/coursebook/raw/main/docs/img/hf-2-annotated.png" alt="HF account" width="100%">

**Click on *Create new token***

<img src="https://github.com/geog3300-agri3003/coursebook/raw/main/docs/img/hf-3-annotated.png" alt="HF account" width="100%">

**Set the token permissions**

You can initially set token permissions to *Read*, which has read access to all your resources. This is a good option for getting started.

<img src="https://github.com/geog3300-agri3003/coursebook/raw/main/docs/img/hf-4-read-annotated.png" alt="HF account" width="100%">

However, as you start developing resources such as models and datasets and using Hugging Face in different environments, it's a good idea to create access tokens with fine-grained permissions with just enough permissions to complete tasks associated with the token.

<img src="https://github.com/geog3300-agri3003/coursebook/raw/main/docs/img/hf-4-annotated.png" alt="HF account" width="100%">

If you have selected fine-grained permissions, you will need to add repositories (models) that you want that token to grant permission to.

<img src="https://github.com/geog3300-agri3003/coursebook/raw/main/docs/img/hf-5-annotated.png" alt="HF account" width="100%">

**Click *Create token* to generate the access token**

<img src="https://github.com/geog3300-agri3003/coursebook/raw/main/docs/img/hf-6-annotated.png" alt="HF account" width="100%">

Copy the access token. **This is your only opportunity to do this - keep a record of the token (in a secure location)**. If you lose your token, it's easy to generate a new one.

<img src="https://github.com/geog3300-agri3003/coursebook/raw/main/docs/img/hf-7-annotated.png" alt="HF account" width="100%">

**In Google Colab, click on the *key* icon in the left-hand sidebar.**

<img src="https://github.com/geog3300-agri3003/coursebook/raw/main/docs/img/hf-8-annotated.png" alt="HF account" width="100%">

Add your Hugging Face access token with the name `HF_TOKEN` and make sure the Notebook access it checked. **Restart your Google Colab session to load the token into your environment**.

<img src="https://github.com/geog3300-agri3003/coursebook/raw/main/docs/img/hf-9-annotated.png" alt="HF account" width="50%">

### Setup Google Colab runtime

Use Google Colab with a *T4 GPU* runtime type.

**Before running any code, set the runtime type to *T4 GPU*.**

<img src="https://github.com/geog3300-agri3003/coursebook/raw/main/docs/img/week-1-colab-runtime-1.jpg" alt="colab runtime menu" width="50%">

<p></p>

<img src="https://github.com/geog3300-agri3003/coursebook/raw/main/docs/img/week-1-colab-runtime-2.jpg" alt="colab runtime menu" width="50%">

