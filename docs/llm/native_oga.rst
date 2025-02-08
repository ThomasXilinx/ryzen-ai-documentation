#####################################
Native OGA Libraries
#####################################

The Native OGA Libraries are available in C++ and Python and give full customizability for deployment into native applications.

AMD provides a set of pre-optimized LLMs ready to be deployed with Ryzen AI Software and the supporting runtime for hybrid execution. These models can be found on Hugging Face in the following collection:

https://huggingface.co/collections/amd/quark-awq-g128-int4-asym-fp16-onnx-hybrid-674b307d2ffa21dd68fa41d5

The steps for deploying the pre-optimized models using Python or C++ are described in the following sections.


***********************************
Setting performance mode (Optional)
***********************************

To run the LLMs in the best performance mode, follow these steps:

#. Go to ``Windows`` → ``Settings`` → ``System`` → ``Power`` and set the power mode to Best Performance.

#. Execute the following commands in the terminal::

   cd C:\Windows\System32\AMD
   xrt-smi configure --pmode performance

.. ****************
.. Package Contents
.. ****************

.. Hybrid LLM artifacts package contains the files required to build and run applications using the ONNX Runtime generate() API (OGA) to deploy LLMs using the Hybrid execution mode. The list below describes which files are needed for the different use cases:

.. - **Python flow**

..   - onnx_utils\\bin\\onnx_custom_ops.dll
..   - onnxruntime_genai\\wheel\\onnxruntime_genai_directml-0.4.0.dev0-cp310-cp310-win_amd64.whl
..   - onnxruntime_genai\\benchmark\\DirectML.dll

.. - **C++ Runtime**

..   - onnx_utils\\bin\\onnx_custom_ops.dll
..   - onnxruntime_genai\\benchmark\\DirectML.dll
..   - onnxruntime_genai\\benchmark\\D3D13Core.dll
..   - onnxruntime_genai\\benchmark\\onnxruntime.dll
..   - onnxruntime_genai\\benchmark\\ryzenai_onnx_utils.dll

.. - **C++ Dev headers**

..   - onnx_utils
..   - onnxruntime_genai

.. - **Examples**


*************************************
Using the Native Python APIs
*************************************

Setup
=====

#. If necessary, install version 1.4 of the Ryzen AI Software according to the :doc:`/inst`

#. Activate the Ryzen AI 1.3 Conda environment:: 

    conda activate ryzen-ai-1.3.0

#. Install the wheel file included in the hybrid-llm-artifacts package::

    cd path_to\\hybrid-llm-artifacts\onnxruntime_genai\wheel
    pip install onnxruntime_genai_directml-0.4.0.dev0-cp310-cp310-win_amd64.whl

Run Models
==========

1. Clone model from the Hugging Face repository and switch to the model directory

2. Open the ``genai_config.json`` file located in the folder of the downloaded model. Update the value of the "custom_ops_library" key with the full path to the ``onnx_custom_ops.dll``, located in the ``hybrid-llm-artifacts\onnx_utils\bin`` folder::

    "session_options": {
            ...
            "custom_ops_library": "path_to\\hybrid-llm-artifacts\\onnx_utils\\bin\\onnx_custom_ops.dll",
            ...
    }

3. Copy the ``DirectML.dll`` file to the folder where the ``onnx_custom_ops.dll`` is located (note: this step is only required on some systems)::
  
    copy hybrid-llm-artifacts\onnxruntime_genai\lib\DirectML.dll hybrid-llm-artifacts\onnx_utils\bin

4. Run the LLM::

    cd hybrid-llm-artifacts\scripts\llama3
    python run_model.py --model_dir path_to\Meta-Llama-3-8B-awq-w-int4-asym-gs128-a-fp16-onnx-ryzen-strix-hybrid

**********************************
Using the Native C++ APIs
**********************************

Setup
=====

#. If necessary, install version 1.4 of the Ryzen AI Software according to the :doc:`/inst`

#. Download and unzip the hybrid LLM artifacts package.

#. Copy required library files from ``onnxruntime-genai\lib`` to ``examples\c\lib``::

    copy onnxruntime_genai\lib\*.* examples\c\lib\

#. Copy ``onnx_utils\bin\ryzenai_onnx_utils.dll`` to ``examples\c\lib``::

    copy onnx_utils\bin\ryzenai_onnx_utils.dll examples\c\lib\

#. Copy required header files from ``onnxruntime-genai\include`` to ``examples\c\include``::

     copy onnxruntime_genai\include\*.* examples\c\include\

#. Build the ``model_benchmark.exe`` application::

     cd hybrid-llm-artifacts\examples\c
     cmake -G "Visual Studio 17 2022" -A x64 -S . -B build
     cd build
     cmake --build . --config Release

**NOTE**: The ``model_benchmark.exe`` executable is generated in the ``hybrid-llm-artifacts\examples\c\build\Release`` folder

Run Models
==========

The ``model_benchmark.exe`` test application serves two purposes:

- It provides a very simple mechanism for running and evaluating Hybrid OGA models
- The source code for this application provides a reference implementation for how to integrate Hybrid OGA models in custom C++ programs

To evaluate models using the ``model_benchmark.exe`` test application::

    # To see settings info
    .\model_benchmark.exe -h

    # To run with default settings
    .\model_benchmark.exe -i $path_to_model_dir  -f $prompt_file -l $list_of_prompt_lengths

    # To show more informational output
    .\model_benchmark.exe -i $path_to_model_dir  -f $prompt_file --verbose

    # To run with given number of generated tokens
    .\model_benchmark.exe -i $path_to_model_dir  -f $prompt_file -l $list_of_prompt_lengths -g $num_tokens

    # To run with given number of warmup iterations
    .\model_benchmark.exe -i $path_to_model_dir  -f $prompt_file -l $list_of_prompt_lengths -w $num_warmup

    # To run with given number of iterations
    .\model_benchmark.exe -i $path_to_model_dir  -f $prompt_file -l $list_of_prompt_lengths -r $num_iterations

For example::

    cd hybrid-llm-artifacts\examples\c\build\Release
    .\model_benchmark.exe -i <path_to>/Llama-3.2-1B-Instruct-awq-g128-int4-asym-fp16-onnx-hybrid -f <path_to>/prompt.txt -l "128, 256, 512, 1024, 2048" --verbose


********************************
Running Customer Fine-Tuned LLMs
********************************

It is also possible to run fine-tuned versions of the ref:`supported models <supported-llms>` (for example, fine-tuned versions of Llama2 or Llama3). For instructions on how to prepare a fine-tuned OGA model for hybrid execution, refer to :doc:`oga_model_preparation`


