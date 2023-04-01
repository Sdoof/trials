# Trials

[![zh](https://img.shields.io/badge/lang-zh-red.svg)](https://github.com/chancefocus/trials/blob/main/README.zh.md)
[![en](https://img.shields.io/badge/lang-en-green.svg)](https://github.com/chancefocus/trials/blob/main/README.md)
[![python](https://img.shields.io/badge/-Python_3.8-blue?logo=python&logoColor=white)](https://github.com/pre-commit/pre-commit)
[![pytorch](https://img.shields.io/badge/PyTorch_1.8+-ee4c2c?logo=pytorch&logoColor=white)](https://pytorch.org/get-started/locally/)
[![black](https://img.shields.io/badge/Code%20Style-Black-black.svg?labelColor=gray)](https://black.readthedocs.io/en/stable/)
[![pre-commit](https://img.shields.io/badge/Pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white)](https://github.com/pre-commit/pre-commit)
[![poetry](https://img.shields.io/badge/Poetry-config-informational?logo=poetry&logoColor=white)](https://python-poetry.org)
[![license](https://img.shields.io/badge/License-MIT-green.svg?labelColor=gray)](https://github.com/lazaratan/dyn-gfn/blob/main/LICENSE)

Automatic Pair Trading


<img
src="assets/trials.jpg"
title="Trials Overview"
style="display: inline-block; margin: 0 auto; max-width: 800px">

## Description
Our codebase trials provide an implementation of the [Select and Trade](https://arxiv.org/abs/2301.10724) paper, which proposes a new paradigm for pair trading using hierarchical reinforcement learning. It includes the code for the proposed method and experimental results on real-world stock data to demonstrate its effectiveness.

## Maintainer
[Boyi Zhang](https://github.com/zbgzbgzbg)


## Prerequisite

The requirements to develop the project.

- Python ^3.8
- Git
- Poetry
- Python IDE (Visual Studio Code is recommended!)

**⚠️ Read the official manual or documentation of following projects before write any code ⚠️**:

- `git`, a version control system
- `Poetry`, a python library to manage project dependencies
- `towncrier`, a changelog generater
- `pre-commit`, a git commit hook runner
- `commitizen`, a semantic version manager

## Data Preparation

1. Download the symbols of target stocks and put the .csv file containing the symbols of the target stocks into selected_symbol_path； Download the processed data put all .csv files containing stock datas into stock_path

2. You can download the data of US stocks from the following website：[Tiingo stock marke](https://api.tiingo.com/documentation/iex)

3. Select and process eligible U.S.S&P 500 stocks by running(The processing steps of the Chinese stock dataset are the same as the following):

```bash
python trials/preprocess/U.S.SP500-selected.py selected_symbol_path stock_path store_path begin_time end_time

```

- `selected_symbol_path`, Stock symbols to be selected are stored in selected_symbol_path
- `stock_path`, All stocks are stored in stock_path
- `store_path`,  Eligible U.S.S&P 500 stocks that have been screened and processed are stored in store_path
- `begin_time`, The start time of the stocks to be screened. For example: '2000-01-01'
- `end_time`, The end time of the stocks to be screened. For example: '2020-12-31'

4. Randomly screen stocks from all stocks to form .csv of symbols, and generate as many .csv as possible with disjoint symbols. Then rolling.py can use these .csv to generate rolling datasets

```bash
python trials/preprocess/random_stocks_selected.py symbols_num_each_rolling, stock_data_path, random_symbol_path

```

- `symbols_num_each_rolling`, The number of stocks contained in each rolling
- `stock_data_path`, All U.S.S&P 500 stocks data screened by U.S.SP500-selected.py are stored in stock_data_path
- `random_symbol_path`, The stocks symbols in each rolling are stored in random_symbol_path

5. Form datasets for each period of each rolling in the form of .csv. by running:

```bash
python trials/preprocess/rolling.py stock_data_path store_path training_month validation_month testing_month random_symbol_path csv_name1 csv_name2 csv_name3

```

- `stock_data_path`, All U.S.S&P 500 stocks data screened by U.S.SP500-selected.py are stored in stock_data_path
- `store_path`, The final .csv files are stored in store_path
- `training_month`, The number of months included in the training
- `validation_month`, The number of months included in the validation
- `testing_month`,The number of months included in the testing
- `random_symbol_path`, The stock symbols included in each rolling that are randomly generated by random_stocks_selected.py in advance are stored in random_symbol_path

6. We provide a subset of the processed dataset, please check it in /trials/data/.


## How to Run?

1. Single Run

```bash
poetry run trials/scripts/train_trials.py
```

See all parameters and their illustrations by `poetry run trials/scripts/train_trials.py --help`

2. Hyperparameter Search
   We recommend to use hyperparamete search by wandb sweep.

Start with a template `sweep.yaml` to define searching parameters and values, then simply run:

```bash
wandb sweep rl_sweep.yaml
```

After showing

```bash
wandb: Run sweep agent with: wandb agent xxx/xxxxxxx/[sweep-id]
```

start the agents with `sweep-id`:

```bash
wandb agent xxx/xxxxxxx/[sweep-id]
```

Or you can start multiple agents with the script:

```bash
bash train_trials.sh [sweep-id] [num-process-per-gpu]
```


## License

[MIT](https://choosealicense.com/licenses/mit/)

## Feel free to cite our paper
```
@misc{han2023select,
      title={Select and Trade: Towards Unified Pair Trading with Hierarchical Reinforcement Learning}, 
      author={Weiguang Han and Boyi Zhang and Qianqian Xie and Min Peng and Yanzhao Lai and Jimin Huang},
      year={2023},
      eprint={2301.10724},
      archivePrefix={arXiv},
      primaryClass={q-fin.CP}
}
```
