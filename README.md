# Fine-Tuning with Divergent Chains of Thought Boosts Reasoning Through Self-Correction in Language Models

This repository includes the code and prompts we used in our 2024 arXiv paper "Fine-Tuning with Divergent Chains of Thought Boosts Reasoning Through Self-Correction in Language Models."



> **Abstract**:
We present a novel method of further improving performance by requiring models to compare multiple reasoning chains before generating a solution in a single inference step. We call this method Divergent CoT (DCoT).
We generate a DCoT dataset where a question is answered by a series of alternative (and correct) chains of thought. Importantly, all these CoTs are part of the same label, thus, forcing the LLM to learn how to generate multiple CoTs in a single inference step.
We find that instruction tuning on DCoT datasets boosts the performance of LLMs of all sizes (from 1.3B to 70B). These performance gains stem from models generating multiple divergent reasoning chains in a single inference step, indicative of the enabling of self-correction in language models.


![divergent CoT description](./assets/intro.png)


> The output data is available at https://tudatalib.ulb.tu-darmstadt.de/handle/tudatalib/4266

- This data includes prompts, model responses, post-processes responses, and results.

## Getting Started
1. Prepare a virtual environment:
```bash
conda create -n dcot python=3.10
conda activate dcot
pip install -r requirements.txt
```

2. Download the training data
```bash
sh download_eva_data.sh
```
## Usage


### Training


```bash
$ python training_script.py \
    --train \
    --base_model_path YOUR_MODEL_PATH/models--llama-2-hf/7B \
    --lora_path YOUR_LORA_OUTPUT_PATH/ \
    --train_path data/dcot_collection/cot9_dataset.json \
    --epochs 3\
    --training_batch_size 4 \
    --dcot \
    --seed 0
```

### Evaluation

```bash
$ python evaluation.py \
    --base_model_path YOUR_MODEL_PATH/models--llama-2-hf/7B \
    --lora_path YOUR_LORA_OUTPUT_PATH/ \
    --min_cots 1 \
    --max_cots 4 \
```

### Remarks on Phi 1.5 and 2
vLLM is not compatible with Phi 1.5 and Phi 2 with lora. Therefore, to evaluate with lora you need to merge the lora weights on the base model first. To do this, we provide `merge_weights.py`. You can run the evaluation as follows

```bash
$ python merge_weights.py \
    --base_model_path YOUR_MODEL_PATH \
    --lora_path YOUR_LORA_PATH \
```

Running this scirpt will generate a folder `merged_model` in YOUR_LORA_PATH. Then, you can run the evaluation on the merged model.

```bash
$ python evaluation.py \
    --base_model_path YOUR_PATH_TO_PHI_MERGED_MODEL \
    --postprocess_responses \
    --min_cots 1 \
    --max_cots 4 \
```

# Model Checkpoints
You can download the model checkpoints for CoT and DCoT in the following links:
- [Phi 1.5](https://tudatalib.ulb.tu-darmstadt.de/handle/tudatalib/4270)
- [Phi 2](https://tudatalib.ulb.tu-darmstadt.de/handle/tudatalib/4269)
- [LLaMA 7B](https://tudatalib.ulb.tu-darmstadt.de/handle/tudatalib/4268)
- [LLaMA 13B](https://tudatalib.ulb.tu-darmstadt.de/handle/tudatalib/4267)
- [LLaMA 13B-Chat](https://tudatalib.ulb.tu-darmstadt.de/handle/tudatalib/4272)
- [LLaMA 70B](https://tudatalib.ulb.tu-darmstadt.de/handle/tudatalib/4271)

# Output Inspection
You can see some output samples manually analyzed in `dcot_samples.json`



## Cite

If you find our work useful, please it using the following citation:

```
@InProceedings{smith:20xx:CONFERENCE_TITLE,
  author    = {Smith, John},
  title     = {My Paper Title},
  booktitle = {Proceedings of the 20XX Conference on XXXX},
  month     = mmm,
  year      = {20xx},
  address   = {Gotham City, USA},
  publisher = {Association for XXX},
  pages     = {XXXX--XXXX},
  url       = {http://xxxx.xxx}
}
```

## Disclaimer

> This repository contains experimental software and is published for the sole purpose of giving additional background details on the respective publication. 