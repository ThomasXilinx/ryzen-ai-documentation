.. Heading guidelines
..     # with overline, for parts
..     * with overline, for chapters
..     =, for sections
..     -, for subsections
..     ^, for subsubsections
..     “, for paragraphs

################
Server Interface
################

Our server interface allows your application to load an LLM on Ryzen AI hardware in a process, and then communicate with this process using standard ``REST`` APIs. This allows applications written in any language (C#, JavaScript, Python, C++, etc.) to easily integrate with Ryzen AI LLMs.

Server interfaces are used across the LLM ecosystem because they allow for no-code plug-and-play between the higher level of the application stack (GUIs, agents, RAG, etc.) with the LLM and hardware that have been abstracted by the server. For example, open source projects such as `Open WebUI <https://github.com/open-webui/open-webui>`_ and `LM Eval <https://github.com/EleutherAI/lm-evaluation-harness>`_ have out-of-box support for connecting to a variety of server interfaces, which allow users to quickly start working with LLMs in a GUI or run a variety of validation tasks, respectively.

************
Server Setup
************

We currently support launching a server process from the Python environment, although we plan to deploy a one-click installer in the future.

To set up the server, follow the :doc:`python_leap` setup instructions.

************
Server Usage
************

Now that you have completed installation, you can launch a server and interact with it like this: 

In a terminal, launch the server by running:

.. code-block:: bash

  lemonade -i amd/Llama-3.2-1B-Instruct-awq-g128-int4-asym-fp16-onnx-hybrid oga-load --device hybrid --dtype int4 serve --max-new-tokens 64

Once the process loads, you can interact with it in a simple HTML + JavaScript application by opening http://localhost:8000 in a web browser.

**********
Next Steps
**********

**NOTE**: out-of-box application support is under active development, and is discussed here for illustrative purposes.

You can get started right away with any application that already supports ``ryzenai_hybrid_dev_server``:

- Microsoft AI Dev Gallery
- LM Eval
- Open WebUI

If you want to add support for ``ryzenai_hybrid_dev_server`` to your application, you can check out the server interface code samples in `an example <https://gitenterprise.xilinx.com/AIG-DAT/ryzenai-llm/blob/main/examples/hybrid/Llama_3_2_1B_Instruct.md#server>`_ or in any of the other `supported models <https://gitenterprise.xilinx.com/AIG-DAT/ryzenai-llm/blob/main/README.md#supported-llms-and-examples>`_.



..
  ------------
  #####################################
  License
  #####################################
  
  Ryzen AI is licensed under `MIT License <https://github.com/amd/ryzen-ai-documentation/blob/main/License>`_ . Refer to the `LICENSE File <https://github.com/amd/ryzen-ai-documentation/blob/main/License>`_ for the full license text and copyright notice.
