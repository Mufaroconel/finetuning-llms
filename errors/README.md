# Error Logs Index

This is a central index to all error documentation encountered during LLM finetuning and usage.

| Error Title                             | Short Summary                                | Link                             |
|---------------------------------------|----------------------------------------------|---------------------------------|
| BitsAndBytes CUDA Setup Nightmare      | CUDA 12.4 unsupported; had to build from source | [Read More](./bitsandbytes-cuda-setup.md) |
| Transformers Import RuntimeError       | Import conflict due to bitsandbytes namespace | [Read More](./huggingface-transformers-import-error.md) |
| NotImplented Error when loading a huggingface dataset | Unsupported local caching | [Read More](./error_notimplementederror_localfilesystem_datasets.md)|
| SFTTrainer KeyError: 'text' Missing | Dataset lacked required 'text' field; fixed with formatting_func| [Readmore](./fixing-sfttrainer-text-column-issue.md)|
---
