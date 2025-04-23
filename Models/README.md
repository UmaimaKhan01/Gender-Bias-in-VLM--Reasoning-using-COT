# Model Weights

This directory is for storing model weights for the gender bias detection project. Due to their large size, model weights are not included in this repository, but this README provides instructions on how to obtain them.

## LlamaV-o1 Base Model

The project uses [LlamaV-o1](https://github.com/mbzuai-oryx/LlamaV-o1) as the base model, which has been released under the Apache 2.0 license. To obtain the model:

### Option 1: Download from Hugging Face Hub

```bash
# Install git-lfs if you don't have it
apt-get install git-lfs
git lfs install

You can load the model directly in your scripts using the Hugging Face transformers library:

```python
from transformers import MllamaForConditionalGeneration, AutoProcessor

model_id = "omkarthawakar/LlamaV-o1"

model = MllamaForConditionalGeneration.from_pretrained(
    model_id,
    torch_dtype=torch.bfloat16,
    device_map="auto",
)
processor = AutoProcessor.from_pretrained(model_id)
```

## Fine-tuned Model Weights

After running the training script (`train.py`), the fine-tuned model weights will be saved in this directory. The expected structure is:

```
models/
  ├── llamav_o1_bias_detector/
  │   ├── config.json
  │   ├── pytorch_model.bin
  │   ├── tokenizer_config.json
  │   └── ...
  └── ...
```

## Baseline Model Weights

For the baseline approaches, the following models are used:

### CLIP

The CLIP model is automatically downloaded when running the `clip_baseline.py` script. The trained classifier will be saved as:

```
models/clip_baseline_classifier.pkl
```

### LLaVA-CoT

The LLaVA model with chain-of-thought capabilities can be used directly from Hugging Face:

```python
from transformers import AutoModelForCausalLM, AutoProcessor

model_name = "llava-hf/llava-1.5-13b-hf"
processor = AutoProcessor.from_pretrained(model_name)
model = AutoModelForCausalLM.from_pretrained(model_name)
```

## License Information

The LlamaV-o1 model and code are released under the Apache 2.0 license. Please refer to the original repository and the LICENSE file in the `docs/` directory for more details.

For more information about the LlamaV-o1 model, please visit:
- GitHub repository: [https://github.com/mbzuai-oryx/LlamaV-o1](https://github.com/mbzuai-oryx/LlamaV-o1)
- Paper: "LlamaV-o1: Rethinking Step-by-step Visual Reasoning in LLMs"
