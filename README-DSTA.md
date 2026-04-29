# HYPIR Image Super-Resolution

## Setup

Create a conda environment and install dependencies:

```bash
conda create -n hypir-dsta python=3.10 -y
conda activate hypir-dsta
pip install -r requirements.txt
```

Download the pretrained HYPIR LoRA weights from HuggingFace:

```bash
huggingface-cli download lxq007/HYPIR HYPIR_sd2.pth --local-dir /path/to/weights
export HYPIR_WEIGHTS=/path/to/weights/HYPIR_sd2.pth
```

## Run

Replace `<lq_dir>` with your input image folder, `<output_dir>` with the desired output path.

### With prompt

Provide a fixed caption describing the image content. Example prompt template for the infrared ship dataset: `"infrared image of a <class-name> on sea water"`.

```bash
LORA_MODULES="to_k,to_q,to_v,to_out.0,conv,conv1,conv2,conv_shortcut,conv_out,proj_in,proj_out,ff.net.2,ff.net.0.proj"

python test.py \
  --base_model_type sd2 \
  --base_model_path Manojb/stable-diffusion-2-1-base \
  --model_t 200 \
  --coeff_t 200 \
  --lora_rank 256 \
  --lora_modules $LORA_MODULES \
  --weight_path "$HYPIR_WEIGHTS" \
  --patch_size 512 --stride 256 \
  --lq_dir <lq_dir> \
  --captioner fixed \
  --fixed_caption "your text prompt here" \
  --scale_by factor \
  --upscale 1 \
  --output_dir <output_dir> \
  --device cuda
```

### Without prompt

```bash
LORA_MODULES="to_k,to_q,to_v,to_out.0,conv,conv1,conv2,conv_shortcut,conv_out,proj_in,proj_out,ff.net.2,ff.net.0.proj"

python test.py \
  --base_model_type sd2 \
  --base_model_path Manojb/stable-diffusion-2-1-base \
  --model_t 200 \
  --coeff_t 200 \
  --lora_rank 256 \
  --lora_modules $LORA_MODULES \
  --weight_path "$HYPIR_WEIGHTS" \
  --patch_size 512 \
  --stride 256 \
  --lq_dir <lq_dir> \
  --captioner empty \
  --scale_by factor \
  --upscale 1 \
  --output_dir <output_dir> \
  --device cuda
```
