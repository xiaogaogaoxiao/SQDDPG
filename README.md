# Rethink Global Reward Game and Credit Assignment in Multi-agent Reinforcement Learning

## Dependencies
This project implements the algorithm of Shapley Q-value policy gradient (SQPG) and demonstrates the experiments in comparison with Independent DDPG, Independent Actor-critic, MADDPG and COMA mentioned in the paper:  

The code is running on Ubuntu 18.04 with Python (3.5.4) and Pytorch (1.0).

The suggestion is installing Anaconda 3 with Python (3.5.4): https://www.anaconda.com/download/.
To enable the experimantal environments, please install OpenAI Gym (0.10.5) and Numpy (1.14.5).
After installing the related dependencies mentioned above, open the terminal and execute the following bash script:
```bash
cd multi-agent-rl/environments/multiagent_particle_envs/
pip install -e .
```

Now, the dependencies for running the code are installed.

## Experiments
The experiments on Cooperative Navigation and Prey-and-Predator mentioned in the paper are based on the environments from https://github.com/openai/multiagent-particle-envs, i.e., simple_spread and simple_tag. For convenience, we merge this repository to our framework with slight modifications on the scenario simple-tag.

About the experiment on Traffic Junction, the environment is from https://github.com/IC3Net/IC3Net/tree/master/ic3net-envs/ic3net_envs. To ease the life, we also add it to our framework.

### Training
To easily run the code for training, we provide argument files for each experiment with variant methods under the directory `args` and bash script to execute the experiment with different arguments.

For example, if we would like to run the experiment of simple_tag with the algorithm SQPG, we can edit the file `simple_tag_sqddpg.py` to change the hyperparameters. Then, we can edit `train.sh` to change the variable `EXP_NAME` to `"simple_tag_sqddpg"` such that
```bash
# !/bin/bash
# sh train.sh

EXP_NAME="simple_tag_sqddpg"
ALIAS=""

cp ./args/$EXP_NAME.py arguments.py
CUDA_VISIBLE_DEVICES=0 python -u train.py > $EXP_NAME$ALIAS.out &
echo $! > $EXP_NAME$ALIAS.pid
```

If necessary, we can also edit the variable `ALIAS` to ease the experiments with different hyperparameters and ``.
Now, we only need to run the code such that
```bash
source train.sh
```

### Testing
About testing, we provide a Python function called `test.py` which includes several arguments such that
```bash
--save-model-dir # the path to save the trained model
--render # whether the visualization is needed
--episodes # the number of episodes needed to run the test
```

## Extension
This framework is easily to be extended by adding extra environments implemented in OpenAI Gym or new multi-agent algorithms implemented in Pytroch. To add extra algorithms, it just needs to inherit the base class `models/model.py` and implement the functions such that
```python
construct_model(self)
policy(self, obs, last_act=None, last_hid=None, gate=None, info={}, stat={})
value(self, obs, act)
construct_policy_net(self)
construct_value_net(self)
get_loss(self)
```
