# STEPS:
## STEP 01: Create a empty remote repository

## STEP 02: initialize a git local repository and connect to remote repository
* open and project folder in VS code then follow the below command -
```bash
echo "# dvc-ML-demo-AIOPS" >> README.md
git init
git remote add origin https://github.com/TUCchkul/Data-Version_Control_ML_Demo.git
git branch -M main
git push origin main
```
## STEP 03:  create and activate conda environments
```bash
conda create -n dvc-ml python=3.7 -y
conda activate dvc-ml  
```
## STEP 04: Create a setup file 
```bash
touch setup.py 
```
paste the bellow content in setup file 
```
from setuptools import setup

with open("README.md", "r", encoding="utf-8") as f:
    long_description = f.read()

setup(
    name="src",
    version="0.0.1",
    author="TUCchkul",
    description="A small package for dvc ml pipeline demo",
    long_description=long_description,
    long_description_content_type="text/markdown",
    url="https://github.com/TUCchkul/Data-Version_Control_ML_Demo",
    author_email="kirticse.chakma869@gmail.com",
    packages=["src"],
    python_requires=">=3.7",
    install_requires=[
        'dvc',
        'pandas',
        'scikit-learn'
    ]
)
```

## STEP 05: Create requirement file and install dependencies
```bash
touch requirements.txt
pip install -r requirements.txt
```
## STEP 06: initialize dvc 
```bash
dvc init 
```
## STEP 07: Create the basic directory structure
```bash
mkdir -p src/utils config
```
## STEP 08: create the config file 
```bash
touch config/config.yaml 
```
content of config.yaml - 
```yaml
data_source: http://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/winequality-red.csv

artifacts:
  artifacts_dir: artifacts 
  raw_local_dir: raw_local_dir
  raw_local_file: data.csv
```
## STEP 09: create the stage 01 python file and all utils file:
```bash
touch src/stage_01_load_save.py src/utils/all_utils.py
```
## STEP 10: Create the dvc.yaml file and the stage 01:
```bash
touch dvc.yaml 
```
content of dvc.yaml file - 
```yaml
stages:
  load_data: 
    cmd: python src/stage_01_load_save.py --config=config/config.yaml  
    deps: 
      - src/stage_01_load_save.py
      - src/utils/all_utils.py 
      - config/config.yaml
    outs:
      - artifacts/raw_local_dir/data.csv
```
## STEP 11: Run the dvc repro command 
```bash
dvc repro 
```
## STEP 12: push the changes to remote repository  
```bash
git add .
git commit -m "Stage 01 added"
git push origin main 
```
