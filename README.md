# Task-Agnostic Continual RL: In Praise of a Simple Baseline


<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li><a href="#structure">Structure</a></li>
    <li><a href="#installation">Installation</a></li>
    <li>
      <a href="#usage">Usage</a>
      <ul>
        <li><a href="#paper-reproduction">Paper Reproduction</a></li>
      </ul>
    </li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>



## About The Project

Official codebase for the paper [Task-Agnostic Continual Reinforcement Learning: In Praise of a Simple Baseline](https://arxiv.org/abs/2205.14495). 
The code can be useful to run continual RL (or multi-task RL) experiments in Meta-World (e.g. in Continual-World). 
The baselines, including replay-based recurrent RL (3RL), are modular enough to be ported into another codebase as well.

If you have found the paper or codebase useful, consider citing the work:
```
@article{caccia2022task,
  title={Task-Agnostic Continual Reinforcement Learning: In Praise of a Simple Baseline},
  author={Caccia, Massimo and Mueller, Jonas and Kim, Taesup and Charlin, Laurent and Fakoor, Rasool},
  journal={arXiv preprint arXiv:2205.14495},
  year={2022}
}
```

<p align="right">(<a href="#top">back to top</a>)</p>



### Built With

* [Pytorch](https://pytorch.org/)
* [Sequoia](https://github.com/lebrice/Sequoia)
* [Mujoco](https://mujoco.org/)

<p align="right">(<a href="#top">back to top</a>)</p>


## Structure
    ├── code
        ├── algs/SAC
            ├── sac.py                   # training SAC   
        ├── configs
            ├── hparams                  # hyperparameter config files
            ├── methods                  # methods' config files
            ├── settings                 # settings config files
        ├── misc
            ├── buffer.py                # sampling data from buffer
            ├── runner_offpolicy.py      # agent sampling data from the env
            ├── sequoia_envs.py          # creating the environemnts
            ├── eval.py                  # evaluation script
            ├── utils.py
        ├── models
            ├── networks.py              # creates the neural networks
        ├── scripts
            ├── analyse_reps.py          # analyse the RNN representations
            ├── test_codebase_cl.py      # makes sures the repo runs correctly
        ├── main_cl.py                   # main file for CRL experiments
        ├── main_mtl.py                  # main file for MTRL experiments 
    ├── public_ck                  
        ├── ...                          # checkpoints in CW10 benchmark

<p align="right">(<a href="#top">back to top</a>)</p>

## Installation

Essentially what you need is 
- python (3.8)
- sequoia
- mujoco_py
- pytorch

It can be quite tricky to install mujoco_py, as well as running Meta-World.
For this reason, we've used [Sequoia](https://github.com/lebrice/Sequoia) to create the continual reinforcement learning environments.

Here's how you can install the dependencies in MacOS (BigSur)

1. create a env, ideally w/ [conda](https://www.anaconda.com/)
```bash
conda create -n tacrl  python=3.8
conda activate tacrl
```

2. install [Sequoia](https://github.com/lebrice/Sequoia) w/ Meta-World add-on
```bash
pip install -e Sequoia[metaworld]
```
or if you as using zsh
```bash
pip install -e "Sequoia[metaworld]"
```

3. extra requirements
```bash
pip install -r extra_requirements.txt
```

4. install mujoco + mujoco key 
You will need to install [MuJoCo](https://github.com/deepmind/mujoco/releases) (version >= 2.1)

UPDATE:
I haven't reinstalled Mujoco since DeepMind acquisition and refactoring.
Best of luck w/ the installation.

You can follow [RoboSuite installation](https://robosuite.ai/docs/installation.html) if you stumble on some `GCC` related bugs (MacOS specific)


Contact us if you have any problem!

<p align="right">(<a href="#top">back to top</a>)</p>


## Usage

example of running SAC w/ an RNN in CW10
```python
python code/main_cl.py --context_rnn True --env_name CW10 --lr 0.0003 --batch_size 1028
```

You can pass config files and reproduce the paper's results by combining a `setting`, `method` and `hyperparameters` config file in the following manner
```python
python code/main_cl.py --setting_config <setting> --method_config <method> --hparam_config <hyperparameters>
```
e.g. running `3RL` in `CW10` w/ the hyperparameter prescribed by Meta-World (for Meta-world v2):
```python
python code/main_cl.py --setting_config CW10 --method_config 3RL --hparam_config meta_world
```

For the MTRL experiments, run 
```python
python code/main_mtl.py
```

for prototyping, you can use the `ant_direction` environment:
``` python
python code/main_cl.py --env_name Ant_direction-v3
```

### Paper reproduction

For access to the [WandB](https://wandb.ai/site) project w/ the results, please contact me. 

For Figure 1, use `analyse_reps.py` (the models are in `public_ck`)

For Figure 2 
```python
python main_cl.py --setting_config <CW10, CL_MW20_500k>  --method_config <method> --hparam_config meta_world
```

For Figure 3 
```python
python main_mtl.py --setting_config MTL_MW20  --method_config <method> --hparam_config meta_world-noet
```

<p align="right">(<a href="#top">back to top</a>)</p>


## License

This project is licensed under the Apache-2.0 License.


## Contact
Please open an issue on [issues tracker](https://github.com/amazon-research/
replay-based-recurrent-rl
/issues) to report problems or to ask questions or send an email to Massimo Caccia - [@MassCaccia](https://twitter.com/MassCaccia) - massimo.p.caccia@gmail.com and  [Rasool Fakoor](https://github.com/rasoolfa).
 

<p align="right">(<a href="#top">back to top</a>)</p>
