
# pyTigerDriver 



## Installation

install with pip:

```shell script
pip install pyTigerDriver
```


## Usage


> **Architecture:** You can instatiate any **Interface [GSQL or REST]** in the **pyTigerDriver** seperately.


This flowchart illustrates the Classes:

```mermaid
graph LR
A[pyTigerDriver as tgCl] -- Gsql --> B((Gsql_Client))
A -- Rest --> C((Rest_Client))
B --> D{TigerGraph Database}
C --> D
```
### Sample Code :
```python
import pyTigerDriver as tg

tgCl = tg.Client(server_ip="127.0.0.1",username="tigergraph",password="tigergraph",version="3.0.5")

print("======================== SIMPLE RESTPP Queries ==================================")
print(tgCl.Rest.get("/echo")) # tgCl.Rest.post or tgCl.Rest.delete 
print("============================== SIMPLE LS ===========================================")
print(tgCl.Gsql.execute("ls"))

```
# Code Linting And Unit Testing

## Code Linting :

Linting using PEP8 Standards

```Shell

user@box:~$ flake8 --exclude=venv* --ignore=E501 --statistics pyTigerDriver/pyDriver.py

```

## Running the tests : 

run unit tests using pytest :

```Shell

user@box:~$ pytest -v

```
## CircleCi Work flow :

1. create a folder named .circleci in the root of the repo
2. within that folder create a file called **config.yml**  containing the folloing flow :

```yaml
version: 2.1

orbs:
  python: circleci/python@0.2.1


jobs:
  build-and-test:
    executor: python/default
    steps:
      # Step 1: obtain repo from GitHub
      - checkout
      # Step 2: create virtual env and install dependencies
      - run:
          name: install dependencies
          command: |
            python3 -m venv venv
            . venv/bin/activate
            pip install -r requirements.txt
      # Step 3: run linter and tests
      - run:
          name: run tests
          command: |
            . venv/bin/activate
            flake8 --exclude=venv* --ignore=E501  --statistics pyTigerDriver/pyDriver.py
            pytest -v


workflows:
  main:
    jobs:
      - build-and-test

```
