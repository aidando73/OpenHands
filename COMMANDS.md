```bash
# New Lambda VM setup
curl -sSL https://install.python-poetry.org | python3 -
# installs nvm (Node Version Manager)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
source ~/.bashrc
# download and install Node.js (you may need to restart the terminal)
nvm install 22
# verifies the right Node.js version is in the environment
node -v # should print `v22.12.0`
# verifies the right npm version is in the environment
npm -v # should print `10.9.0`
sudo apt-get install build-essential
# Sudoless docker (otherwise we run into errors)
sudo usermod -aG docker $USER
newgrp docker


source ~/miniconda3/bin/activate && conda create --prefix ./env python=3.12

source ~/miniconda3/bin/activate && conda activate ./env

make build

make run

screen -S swe_bench
# ./evaluation/benchmarks/swe_bench/scripts/run_infer.sh [model_config] [git-version] [agent] [eval_limit] [max_iter] [num_workers] [dataset] [dataset_split]
track ./evaluation/benchmarks/swe_bench/scripts/run_infer.sh llm.eval_test HEAD CodeActAgent 300 30 1 princeton-nlp/SWE-bench_Lite test
# At 20 workers we start hitting Anthropic rate limit
# At 12 workers we start hitting Anthropic rate limit
# At 6 workers we start hitting Anthropic rate limit
# At 3 workers we start hitting Anthropic rate limit
# At 2 workers we start hitting Anthropic rate limit



screen -ls
screen -r swe_bench
# ctrl-a + d to detach from screen
# ctrl-a + esc to scroll up

htop
```


config.toml file:

```toml
[llm]
# IMPORTANT: add your API key here, and set the model to the one you want to evaluate
model = "claude-3-5-sonnet-20241022"

[llm.eval_test]
api_key = ""
num_retries = 8
retry_min_wait = 60
retry_max_wait = 120
retry_multiplier = 1
```
