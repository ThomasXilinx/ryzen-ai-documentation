########
Overview
########

************************************
OGA-based Flow with Hybrid Execution
************************************

Starting with version 1.4, the Ryzen AI Software supports deploying quantized LLMs on Ryzen AI PCs through the ONNX Runtime generate() API (OGA) using a hybrid execution mode, which leverages both the NPU and iGPU.

The software optimally partitions the model such that different layers/operations in the model are scheduled on NPU/iGPU for optimal time-to-first-token (TTFT) in prefill-phase and fastest token generation (TPS) in decode phase. 

This approach is designed to maximize the use of available compute resources, leading to optimal performance and energy efficiency. Benefits of AMD’s hybrid flow include:

- Efficiency: By distributing the workload between NPU and iGPU, the hybrid flow optimizes resource utilization resulting in better workload efficiency for power efficient LLM generation. 
- Scalability: The hybrid flow is highly scalable, allowing for seamless integration with various hardware configurations and enhancing the platform's versatility.
- Performance: Leveraging both NPU and iGPU provides a balanced approach to handling complex AI tasks, giving the user high throughput, low latency and stable performance.

Supported Configurations
========================

The OGA-based flow supports Strix (STX) and Krackan Point (KRK) processors running Windows 11. Phoenix (PHX) and HawkPoint (HPT) processors are not supported.


*******************************
Development Interfaces
*******************************
The Ryzen AI LLM software stack is available through different development interfaces, each suited for specific use cases:

Simple Python API
=================

This interface is the fastest way to:

- Try models on Ryzen AI hardware.
- Validate inference speed and task performance.
- Integrate with Python apps.

To get started in Python, follow these instructions :doc:`simple_python`.


Server Interface
================

The Server Interface provides a convenient means to integrate with applications that:

- Already support an LLM server interface, such as the `ollama server <https://github.com/ollama/ollama/blob/main/docs/api.md>`_ or `OpenAI API <https://platform.openai.com/docs/api-reference/introduction>`_.
- Are written in any language (C++, C#, Javascript, etc.) that supports REST APIs.
- Benefits from a silent installer or process isolation for the LLM backend.

To get started with the server interface, follow these instructions :doc:`server_interface`.


Native OGA Libraries
====================

The Native OGA Libraries are available in C++ and Python and give full customizability for deployment into native applications.

To get started with the native OGA libraries, follow these instructions :doc:`native_oga`.


.. _supported-llms:

*******************************
Supported LLMs
*******************************

Pre-optimized models for Hybrid execution are available on Hugging Face: https://huggingface.co/collections/amd/quark-awq-g128-int4-asym-fp16-onnx-hybrid-674b307d2ffa21dd68fa41d5

- microsoft/Phi-3-mini-4k-instruct
- microsoft/Phi-3.5-mini-instruct
- mistralai/Mistral-7B-Instruct-v0.3
- Qwen/Qwen1.5-7B-Chat  
- THUDM/chatglm3-6b
- meta-llama/Llama-2-7b-hf
- meta-llama/Llama-2-7b-chat-hf
- meta-llama/Meta-Llama-3-8B
- meta-llama/Llama-3.1-8B
- meta-llama/Llama-3.2-1B-Instruct
- meta-llama/Llama-3.2-3B-Instruct

It is also possible to run fine-tuned versions of the models listed above (for example, fine-tuned versions of Llama2 or Llama3). For instructions on how to prepare a fine-tuned OGA model for hybrid execution, refer to :doc:`oga_model_preparation`



******************
Alternate Flows
******************

.. note::
   
   The alternate flows for LLMs described below are currently in the Early Access stage. Early Access features are features which are still undergoing some optimization and fine-tuning. These features are not in their final form and may change as we continue to work in order to mature them into full-fledged features.


OGA-based Flow with NPU-only Execution
======================================

The primary OGA-based flow for LLMs employs an hybrid execution mode which leverages both the NPU and iGPU. AMD also provides support for an OGA-based flow where the iGPU is not sollicited and where the compute-intensive operations are exclusively offloaded to the NPU.

The OGA-based NPU-only execution mode is supported on STX and KRK platforms.

To get started with the OGA-based NPU-only execution mode, follow these instructions :doc:`native_oga_npu_only`.


PyTorch-based Flow
==================

An experimental flow based on PyTorch is available here: https://github.com/amd/RyzenAI-SW/blob/main/example/transformers/models/llm/docs/README.md 

This flow provides functional support for a broad set of LLMs. It is intended for prototyping and experimental purposes only. It is not optimized for performance and it should not be used for benchmarking. 

The Pytorch-based flow is supported on PHX, HPT and STX platforms.


..
  ------------

  #####################################
  License
  #####################################

 Ryzen AI is licensed under `MIT License <https://github.com/amd/ryzen-ai-documentation/blob/main/License>`_ . Refer to the `LICENSE File <https://github.com/amd/ryzen-ai-documentation/blob/main/License>`_ for the full license text and copyright notice.
