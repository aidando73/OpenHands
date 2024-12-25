```bash
# New Lambda VM setup
git clone git@github.com:aidando73/OpenHands.git

git checkout aidand-open-hands

curl -sSL https://install.python-poetry.org | python3 -
# installs nvm (Node Version Manager)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
source ~/.bashrc
# download and install Node.js (you may need to restart the terminal)
nvm install 22
# verifies the right Node.js version is in the environment
node -v # should print `v22.12.0`
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

# Llama 405B
# Costs about ~$60 to run the full evaluation
screen -S swe_bench
track ./evaluation/benchmarks/swe_bench/scripts/run_infer.sh llm.llama3_1_405B HEAD CodeActAgent 300 30 12 princeton-nlp/SWE-bench_Lite test


# If you run out of space
docker system prune

# Not helpful no
# Clear containers
docker rm -vf $(docker ps -aq)
# Clear images
docker rmi -f $(docker images -aq)

# Check how many tests have been completed
watch -c wc -l evaluation/evaluation_outputs/outputs/princeton-nlp__SWE-bench_Lite-test/CodeActAgent/llama-3.1-405b-instruct_maxiter_30_N_v0.16.1-no-hint-run_1/output.jsonl
```


config.toml file:

```toml
[llm]
# IMPORTANT: add your API key here, and set the model to the one you want to evaluate

[llm.claude]
model = "claude-3-5-sonnet-20241022"
api_key = ""
num_retries = 8
retry_min_wait = 60
retry_max_wait = 120
retry_multiplier = 1

[llm.llama3_1_405B]
model = "openrouter/meta-llama/llama-3.1-405b-instruct"
api_key = ""
custom_llm_provider = "openrouter"
```
