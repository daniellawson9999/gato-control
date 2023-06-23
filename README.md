(In Progress)

Goal: working and trainable [Gato](https://arxiv.org/abs/2205.06175) implementation for control tasks to be used for [Manifold's Neko](https://github.com/ManifoldRG), or other projects.


# Setup
```bash
conda env create -f env.yml 
```

## Minari
We rely on [Minari](https://minari.farama.org/) to provide a standard for datasets. However, we are currently testing with MuJoCo locomotion and Atari tasks, which are not included in Minari by default. These datasets are generated by: https://github.com/daniellawson9999/data-tests/
and can be downloaded by: 



```bash
git clone https://github.com/Farama-Foundation/Minari.git
cd Minari
pip install -e .

cd ..
python ./gato/data/download_custom_datasets.py
```

Where datasets can be any string in download_custom_datasets.py or dataset in https://minari.farama.org/ with Box or Discrete observation or action spaces, although these environments have not been tested yet. 

# Training
Below are some example training commands. These have been at least tested for a few updates and evaluation to verify logical correctness, but full training runs may not have been performed. 


```bash
python train.py --datasets d4rl_halfcheetah-expert-v2 d4rl_hopper-expert-v2 d4rl_walker2d-expert-v2
```

test, with toy size
```bash
python train.py --embed_dim=128 --layers=2 --heads=4 --training_steps=1000 --log_eval_freq=10 --warmup_steps=100 --batch_size=4 -k=256
```

```bash
python train.py --embed_dim=128 --layers=3 --heads=1 --training_steps=10000 --log_eval_freq=1 --warmup_steps=100 --batch_size=4 -k=512 --eval_episodes=1 --device=cuda --datasets Breakout-expert_s0-v0
```

## Examples

```python
# This computes logits and (cross-entropy) loss over a batch of size three, where each diciontary is an episode in the batch
logits, loss = model([
    {
        'images': torch.randn(20, 3, 80, 64),
        'discrete_actions': torch.randint(0, 55, (20, 1)),
    },
    {
        'continuous_obs': torch.randn(15, 8),
        'continuous_actions': torch.randn(15, 4),
    },
    {
        'images': torch.randn(100, 3, 224, 224),
        'continuous_actions': torch.randn(100, 11),
    }
], compute_loss=True)

```

# Credits

This implementation is influenced or uses components from:
- https://github.com/OrigamiDream/gato/tree/main
- https://github.com/LAS1520/Gato-A-Generalist-Agent
- [VIMA: General Robot Manipulation with Multimodal Prompts](https://arxiv.org/abs/2210.03094): https://github.com/vimalabs/VIMA
- [Decision Transformer: Reinforcement Learning via Sequence Modeling
](https://arxiv.org/abs/2106.01345) https://github.com/kzl/decision-transformer
