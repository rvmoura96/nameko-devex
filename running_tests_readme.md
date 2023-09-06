In case your are using a higher version of python it suggested to install [pyenv](https://github.com/pyenv/pyenv]) a tool to easily switch python current version.


On mac OS
```
brew update
brew install pyenv
```

After the instalation you should add the following lines to your shell controller. I'm currently using zsh you can do with the following commands:

```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
```

To install the required version suggested on project you should run those commands:
```
pyenv install 3.7.2
```

Afther the installation you may need to add this on your shell controller you can edit in your code editor: 
```
if command -v pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init -)"
fi
```

After this run the command to force your main version to 3.7.2:
```
pyenv global 3.7.2
```

restart your terminal and come back to the directory of this repo.


You can check your python version running:
```
python --version
```

Install conda a required toll to run the project:
```
brew install --cask anaconda
```

After the installation run another dependency from the project:
```
brew install jq
```

Create an virtual environment with conda:
```
conda env create -f environment_dev.yml
```

To activate you need to use: 
```
conda activate nameko-rv
```

A good thing is check if our dependencies are installed open a python instance and run this code to test if you are available to import nameko:

```
python

import nameko
```

If you could import successfully you can procede.


Start backing services as docker containers:
```
./dev_run_backingsvcs.sh
```

Start nameko services in one process (gateway, orders and products)
```
./dev_run.sh gateway.service orders.service products.service
```

After this open another instance of terminal and go back to repository directory.

Activate virtual environment
```
conda activate nameko-rv
```

To run the performance test run the command:
```
./test/nex-bzt.sh local 100 1h 3m
```

After the number of users reaches to 100 the application will shut down, if you want to run more tests you will need to start services again.

```
./dev_run.sh gateway.service orders.service products.service
```

To run unit tests:
```
./dev_pytest.sh
```

To run smoke tests you can run with this command:
```
./test/nex-smoketest.sh local
```

You can also starts the services using an integration with FastAPI, using this command:
```
FASTAPI=X ./dev_run.sh orders.service products.service
```

With this integration the application can support 100 users simultaneously and dont shut down with just 100 users.

You can run the integration tests again after the services up:
```
./test/nex-bzt.sh local 1000 1h 3m
```

With this version the application can handle nearly 500 users simultaneously and dont shutdown but all reaquests made after the users reach near to 500 will receive errors and on terminal running the server will be showed an error message about too many files open simultaneously.