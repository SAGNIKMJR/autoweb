# Autonomous web navigation with RL
 This is the code for the final project in CS394R: Reinforcement Learning Theory and Practice at UT Austin.

**Authors:** Sagnik Majumder, Chinmoy Samant 

This code is heavily inspired from the [**source code**](https://github.com/stanfordnlp/wge) accompanying the ICLR 2018 paper:  
[**Reinforcement Learning on Web Interfaces using Workflow-Guided Exploration**](https://arxiv.org/abs/1802.08802)


## Setup

### General setup

- Python dependencies
  ```
  pip install -r requirements.txt
  ```
  - If this gives you problems, try again and add pip's ```--ignore-installed```
  flag.

- Node and npm
  - Make sure Node and npm are installed via ```brew install node```. If they 
  are, ```node -v``` and ```npm -v``` should print version numbers.

- PyTorch
  - Install PyTorch v1.2.0

- Selenium
  - Outside this repository, download
    [ChromeDriver](https://sites.google.com/a/chromium.org/chromedriver/downloads).
    Unzip it and then add the directory containing the `chromedriver` executable
    to the `PATH` environment variable
    ```
    export PATH=$PATH:/path/to/chromedriver
    ```
  - If instead you're using Anaconda,
    use ```conda install -c conda-forge selenium```.

### Data directory setup

- This code depends on the environmental variable ```$RL_DATA``` being set,
  pointing to a configured data directory.

- Create a data directory ```mkdir -p /path/to/data``` and set ```export
  $RL_DATA=/path/to/data```. In order for the code to run, ```$RL_DATA```
  will need to be set to point at this directory.

- Next, set up the data directory:
  ```
  cd $RL_DATA
  # Download glove from https://nlp.stanford.edu/data/glove.6B.zip and place
  # in current directory however you want
  # Suggested: wget https://nlp.stanford.edu/data/glove.6B.zip
  unzip glove.6B.zip
  mv glove.6B glove
  ```

### Demonstration directory setup

```
# Where $REPO_DIR is the path to the root of this Git repository.
git clone https://github.com/stanfordnlp/miniwob-plusplus-demos.git $REPO_DIR/third-party/miniwob-demos
export RL_DEMO_DIR=$REPO_DIR/third-party/miniwob-demos/
```

### MiniWoB setup

- There are 2 ways to access MiniWoB tasks:
  1. **Use the `file://` protocol (Recommended):**
    Open `miniwob-sandbox/html/` in the browser,
    and then export the URL to the `MINIWOB_BASE_URL` environment variable:
    ```
    export MINIWOB_BASE_URL='file:///path/to/miniwob-sandbox/html/'
    ```
  2. **Run a simple server:** go to `miniwob-sandbox/html/` and run the supplied `http-serve`.
    - The tasks should now be accessible at `http://localhost:8080/miniwob/`
    - To use a different port (say 8765), run `http-serve 8765`, and then
    export the following to the `MINIWOB_BASE_URL` environment variable:
    ```
    export MINIWOB_BASE_URL='http://localhost:8765/'
    ```
- Once you've followed one of the steps above, test `MiniWoBEnvironment` by running
  ```
  pytest wge/tests/miniwob/test_environment.py -s
  ```

### MiniWoB versions of FormWoB

Follow the "Run a simple server" instruction in the MiniWoB setup section above.

## Training models

1) WGE + RL on email-inbox with 10 demonstrations and natural language queries:
```
python3 main.py -t email-inbox-nl-turk -s "demonstrations.max_to_use = 10" configs/default-base.txt
```
2) WGE + RL on email-inbox with 10 demonstrations and hard reward:
```
python3 main.py -t email-inbox -s "demonstrations.max_to_use = 10, env.reward_processor.type=hard" configs/default-base.txt
```

3) WGE + RL on email-inbox with 10 demonstrations and augmented reward:
```
python3 main.py -t email-inbox -s "demonstrations.max_to_use = 10, env.reward_processor.type=augmented" configs/default-base.txt
```

4) BC + RL on email-inbox with 10 demonstrations and hard reward:
```
python3 main.py -t email-inbox -s "demonstrations.max_to_use = 10, env.reward_processor.type=hard" configs/default-base.txt configs/config-mixins/bc-rl.txt
```

5) BC + RL on email-inbox with 10 demonstrations and augmented reward:
```
python3 main.py -t email-inbox -s "demonstrations.max_to_use = 10, env.reward_processor.type=augmented" configs/default-base.txt configs/config-mixins/bc-rl.txt
```

6) WGE + RL on enter-time with 10 demonstrations:
```
python3 main.py -t enter-time -s "demonstrations.max_to_use = 10" configs/default-base.txt
```

7) WGE + RL on enter-time with 100 demonstrations:
```
python3 main.py -t enter-time -s "demonstrations.max_to_use = 100" configs/default-base.txt
```

8) BC + RL on enter-time with 10 demonstrations:
```
python3 main.py -t enter-time -s "demonstrations.max_to_use = 10" -s "demonstrations.base_dir = 2017-11-05_fourth-turk" -r 0 configs/default-base.txt configs/config-mixins/bc-rl.txt configs/config-mixins/no-demo-filter.txt
```

9) BC + RL on enter-time with 100 demonstrations:
```
python3 main.py -t enter-time -s "demonstrations.max_to_use = 100" -s "demonstrations.base_dir = 2017-11-05_fourth-turk" -r 0 configs/default-base.txt configs/config-mixins/bc-rl.txt configs/config-mixins/no-demo-filter.txt
```