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

./evaluation/benchmarks/swe_bench/scripts/run_infer.sh llm.eval_test
```


config.toml file:

```toml
[llm]
# IMPORTANT: add your API key here, and set the model to the one you want to evaluate
model = "claude-3-5-sonnet-20241022"
api_key = ""

[llm.eval_test]
```
