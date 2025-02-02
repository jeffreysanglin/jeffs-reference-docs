### After creating a fresh notebook on Gradient, I tried to download a new model from 🤗
However, I got an error saying that I needed to install flash-attn. `pip install flash-attn` did not work.

### Reinstalling Pytorch
I uninstalled pytorch to start fresh:
`pip uninstall torch torchvision torchaudio`
>uninstall successful!

Then I reinstalled pytorch to use the right version (v2.4).
`pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124`
>I got the code from pytorch website: https://pytorch.org/get-started/locally/

>The install was successful!


### Re-attempt flash-attn install. It's struggling to build the wheel from an unsupported tag.
I installed flash-attn using a specified wheel: `pip install "flash-attn>=2.7.3" --find-links "https://github.com/Dao-AILab/flash-attention/releases"`
>This was successful - Thanks ChatGPT!

1/30/25 - When I reran this code, it failed at this poing. ChatGPT suggested: `pip install --no-binary=flash-attn flash-attn`
>This worked!

> I think the above only worked because I also did: `pip install --upgrade pip setuptools wheel`. Let's do that before installing with --no-binary.

2/2/25 - Apparently I also need to install a package called accelerate: `'pip install 'accelerate>=0.26.0'`

### Trouble with transformers "utils" functions
uninstalled transformers and fell back to transformers==4.44.2

### In this instance I'm trying to testrun the DeepSeek R1 weights. Can we get the DeepSeek weights to download and run?
_adapted from ChatGPT_
```
#Define the model name from Hugging Face
model_name = "deepseek-ai/DeepSeek-R1"

# Load the tokenizer
tokenizer = AutoTokenizer.from_pretrained(model_name)

# Load the model weights
model = AutoModelForCausalLM.from_pretrained(
    model_name, torch_dtype=torch.float16, device_map="auto"
)

#Move to GPU if available
device = "cuda" if torch.cuda.is_available() else "cpu"
model.to(device)
```
It seems like I'm getting an error from Transformers Utils, so it's likely a versioning error.

ChatGPT confirms and suggests specifying the version for R1. Sounds good to me!

`pip install transformers==4.46.3`
>from DeepSeek Readme: https://github.com/deepseek-ai/DeepSeek-V3?tab=readme-ov-file

I'll install the other dependencies while I'm at it:
```
torch==2.4.1
triton==3.0.0
transformers==4.46.3
safetensors==0.4.5
```
>I restarted the kernel and reran.
### Now I'm getting an import error ""Using low_cpu_mem_usage=True or a device_map requires Accelerate: pip install 'accelerate>=0.26.0'"

>I ran the install command. Let's see what happens.

### The AutoModelForCausalLM.from_pretrained() function hits an error that seems related to the quantization. From what I'm reading this error is more related to data configuration rather than the environment. Success!