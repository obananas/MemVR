
### Look Twice Before You Answer: Memory-Space Visual Retracing for Hallucination Mitigation in Multimodal Large Language Models


## 🎯 Overview
We propose Memory-Space Visual Retracing (MemVR), a novel hallucination mitigation paradigm without needing external knowledge retrieval or additional fine-tuning. MemVR has two significant advantages:
* First, MemVR significantly mitigates hallucination issues across various MLLMs and excels in general benchmarks, emphasizing its potential for widespread applicability.
* Second, MemVR is a plug-and-play solution without incurring added time overhead.

![MemVR](assets/compared.png)

![MemVR](assets/bigfig.png)

<div align="center">
<strong>It’s a game-changer for effectiveness and efficiency.</strong>
</div>

In contrast to previous methods, which primarily focus on eliminating biases of language priors, MemVR seeks to replenish visual clues towards more evidential responses, which signifies the other side of the coin.
Comprehensive experimental evaluations demonstrate that MemVR significantly mitigates hallucination issues across various MLLMs and excels in general benchmarks without incurring added time overhead.

## 🕹️ Usage

### Installation

1. We recommend you to use [LLaVA](https://github.com/haotian-liu/LLaVA) as the working environment. Please clone the repository from [LLaVA](https://github.com/haotian-liu/LLaVA) and set up the envirnment bu running
```
git clone https://github.com/haotian-liu/LLaVA
cd LLaVA
conda create -n memvr python==3.10
conda activate memvr
pip install --upgrade pip
pip install -e .
```
2. After setting up, clone the repository and move all contents to the main directory of LLaVA (except README.md).
```bash
LLaVA/
├── llava/
│ ├── eval/ # merge here in next step
│ ├── .../
├── eval_scripts/
│ ├── llava/
│ ├── qwen/
│ ├── glm/
├── memvr.py/
├── inference.py/
├── red_bird.jpg/
└── ...
```
Then merge the file [eval] to directory 
```
/LLaVA/llava/eval/
```

### Downloading Checkpoints
Under the main directory of LLaVA:
1. Download the checkpoint of LLaVA v1.5 [here](https://huggingface.co/liuhaotian/llava-v1.5-7b).
2. Download the checkpoint of Qwen-VL-Chat [here](https://huggingface.co/Qwen/Qwen-VL-Chat). Replace the downloaded 'modeling_qwen.py' by [modeling_qwen.py] to enable MemVR on Qwen-VL-Chat model.
3. Download the checkpoint of glm-4v-9b [here](https://huggingface.co/THUDM/glm-4v-9b). Replace the downloaded 'modeling_chatglm.py' by [modeling_chatglm.py] to enable MemVR on glm-4v-9b model.

You may check if your environment works fine by running
```
python inference.py
```

### Evaluation
Follow [Evaluation.md](https://github.com/haotian-liu/LLaVA/blob/main/docs/Evaluation.md) in [LLaVA](https://github.com/haotian-liu/LLaVA) to prepare for the benchmark materials. Additionally, we recommen you to use GPUs with no less than 40GB of VRAM.
Test with these benchmarks by running
```
bash eval_scripts/llava/mme.sh 
```
Please note that you may need to fill in your own OpenAI API-KEY for GPT-based evaluations like llavabench or MM-Vet.

Here are some tips of the parameters in the scripts:
```
    --retracing-ratio 0.12 \
    --entropy-threshold 0.75 \
    --starting-layer 5 \
    --ending-layer 16 \
```
Where 
* [retracing-ratio] refers to the percentage of visual_token to be retraced in certain layer. It has straightforward effect on the model's performance.
* [entropy-threshold] defines the minimum layer-wide entropy that triggers visual information retracing.
* [starting-layer] and [ending-layer] set the range of layers where visual information retracing is allowed.


## 🏅 Experiments
![MemVR](assets/mmbench.png)
*Figure 5. Results on MMBench. MemVR enhances comprehensive performance on diverse tasks.*

## 📌 Examples
![Case1](assets/caseA.png)
*Figure 9. Visualization of uncertainty across layers without and with MemVR. MemVR effectively reduces uncertainty after the 8th layer, contributing to hallucination mitigations.*

![MemVR](assets/cases2.png)
*Figure 13: A case study comparing the levels of hallucination among various baselines.*

![Case2](assets/longcase.png)
*Figure 10. A case study in long text generation. MemVR effectively mitigates hallucinations.*

## 📝 Related Projects
- [OPERA](https://github.com/shikiw/OPERA): OPERA: Alleviating Hallucination in Multi-Modal Large Language Models via Over-Trust Penalty and Retrospection-Allocation
- [VCD](https://github.com/XiangLi1999/ContrastiveDecoding): VCD: Mitigating Object Hallucinations in Large Vision-Language Models through Visual Contrastive Decoding
- [DoLa](https://github.com/voidism/DoLa): DoLa: Decoding by Contrasting Layers Improves Factuality in Large Language Models
- [Contrastive Decoding](https://github.com/XiangLi1999/ContrastiveDecoding): Open-ended Text Generation as Optimization
- [GLM-4V](https://github.com/THUDM/GLM-4): ChatGLM: A Family of Large Language Models from GLM-130B to GLM-4 All Tools
- [Qwen-VL](https://github.com/QwenLM/Qwen-VL): A Versatile Vision-Language Model for Understanding, Localization, Text Reading, and Beyond
- [LLaVA 1.5](https://github.com/haotian-liu/LLaVA): Improved Baselines with Visual Instruction Tuning
