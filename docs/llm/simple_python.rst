.. Heading guidelines
..     # with overline, for parts
..     * with overline, for chapters
..     =, for sections
..     -, for subsections
..     ^, for subsubsections
..     “, for paragraphs

###########################
Simple Python API (LEAP)
###########################

A Python environment is a great way to get started because it provides maximum flexibility for trying out models, profiling models, and integration into Python applications.

We use the `ONNX TurnkeyML project <https://github.com/onnx/turnkeyml>`_ to get up and running quickly. TurnkeyML provides the ``lemonade`` "LLM aide" tools and ``lemonade.leap`` APIs you will see throughout this repo.

To get started in Python, follow these instructions.

***************************
System-level pre-requisites
***************************

You only need to do this once per computer:

#. Make sure your system has the `Ryzen AI 1.3 driver <https://ryzenai.docs.amd.com/en/latest/inst.html#install-npu-drivers>`_ installed. 
#. `Download and install miniconda for Windows <https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe>`_.
#. Launch a terminal and call `conda init`

*****************
Environment Setup
*****************

To create and set up an environment, run these commands in your terminal:

#. Create environment::

    conda create -n ryzenai-llm python=3.10

#. Activate environment::

    conda activate ryzenai-llm

#. Install ONNX TurnkeyML to get access to the LLM tools and APIs::

    pip install turnkeyml[llm-oga-hybrid]

#. Install support for Ryzen AI Hybrid LLMs::

    lemonade-install --ryzenai hybrid

****************
Validation Tools
****************

Now that you have completed installation, you can try prompting an LLM like this (where `PROMPT` is any prompt you like).

Run this command in a terminal that has your environment activated::

    lemonade -i amd/Llama-3.2-1B-Instruct-awq-g128-int4-asym-fp16-onnx-hybrid oga-load --device hybrid --dtype int4 llm-prompt --max-new-tokens 64 -p PROMPT

Each example linked in the `support models table <https://gitenterprise.xilinx.com/AIG-DAT/ryzenai-llm/blob/main/README.md#supported-llms-and-examples>`_ below also has `example commands <https://gitenterprise.xilinx.com/AIG-DAT/ryzenai-llm/blob/main/examples/hybrid/Llama_3_2_1B_Instruct.md#validation>`_ for validating the speed and accuracy of each model.

**********
Python API
**********
You can also run this code to try out our API in a Python script:

.. code-block:: python

  from lemonade import leap

  model, tokenizer = leap.from_pretrained(
      "amd/Llama-3.2-1B-Instruct-awq-g128-int4-asym-fp16-onnx-hybrid", recipe="oga-hybrid"
  )

  input_ids = tokenizer("This is my prompt", return_tensors="pt").input_ids
  response = model.generate(input_ids, max_new_tokens=30)

  print(tokenizer.decode(response[0]))

Each example linked in the `support models table <https://gitenterprise.xilinx.com/AIG-DAT/ryzenai-llm/blob/main/README.md#supported-llms-and-examples>`_ below also has an `example script <https://gitenterprise.xilinx.com/AIG-DAT/ryzenai-llm/blob/main/examples/hybrid/Llama_3_2_1B_Instruct.md#streaming>`_ for streaming the text output of the LLM.

**********
Next Steps
**********

From here, you can check out `an example <https://gitenterprise.xilinx.com/AIG-DAT/ryzenai-llm/blob/main/examples/hybrid/Llama_3_2_1B_Instruct.md>`_ or any of the other `supported models <https://gitenterprise.xilinx.com/AIG-DAT/ryzenai-llm/blob/main/README.md#supported-llms-and-examples>`_. 

The examples pages also provide code for: 

#. Additional validation tools.
#. Streaming responses with the API.
#. Integrating the API into applications
#. Using the server  interface from a variety of programming languages.




..
  ------------
  #####################################
  License
  #####################################
  
  Ryzen AI is licensed under `MIT License <https://github.com/amd/ryzen-ai-documentation/blob/main/License>`_ . Refer to the `LICENSE File <https://github.com/amd/ryzen-ai-documentation/blob/main/License>`_ for the full license text and copyright notice.
