# AI/ML API

## Requirements

- Python 3.9
- Anaconda
- PipelineAlternative_clinicaldata data
- PBPK data
- cddd data

## Installation


Clone the repository:

```sh
git clone git@github.com:ALTERNATIVE-EU/ai-ml-api.git
```

Put the `PipelineAlternative_clinicaldata` `models` directory.
Put the `cddd` directories in the `PipelineAlternative_clinicaldata` directory.
Put the `PBPK` directory in the `models` directory.

Copy the content of `patches` directory into the `models` directory.

```sh
cp -r patches/* ./models/
```

Create cddd virtual environment and install dependencies:

```sh
cd cddd
conda env create -f environment.yml
conda activate cddd
pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.10.0-cp36-cp36m-linux_x86_64.whl
pip install -e .
conda deactivate
```

Create alternative virtual environment:

```sh
conda create -n alternative python=3.9
conda activate alternative
```

Install dependencies:

```sh
pip install -f requirements.txt
```

Start the server:

```sh
python3 app.py
```

## Installation with Docker

Build the image:

```sh
docker build -t ai-ml-api .
```

Run the container:

```sh
docker run -p 5000:5000 ai-ml-api
```

## Deployment in Kubernetes

Update the deployment files in the deployment/kubernetes directory, then apply them:

```sh
kubectl apply -f deployment/kubernetes
```

For the authentication we use istio with custom envoy filter, follow the instructions in the [envoy-filter](./envoy-filter.md) documentation.

## Usage

ML:

```sh
curl -X POST -H "Content-Type: application/json" -d '{"smiles": "c1ccccc1O"}' http://localhost:5000/clinicaldata/ml/evaluate -o results.csv
```

AI:

```sh
curl -X POST -H "Content-Type: application/json" -d '{"smiles": "c1ccccc1O"}' http://localhost:5000/clinicaldata/ai/evaluate
```

AOP:

```sh
curl -X POST -H "Content-Type: application/json" -d '{"smiles": "c1ccccc1O"}' http://localhost:5000/clinicaldata/aop/evaluate
```

Doxorubicin:

```sh
curl -X POST http://127.0.0.1:5000/pbpk/doxorubicin      -H "Content-Type: application/json"      -d '{
           "dose_mg": 60,
           "age": 50,
           "weight": 70,
           "height": 190
         }'
```

HTTK:

```sh
curl -X POST http://127.0.0.1:5000/pbpk/httk -H "Content-Type: application/json"      -d '{
           "chem_name": "Bisphenol A",
           "species": "human",
           "daily_dose": 1,
           "doses_per_day": 1,
           "days": 15
         }'
```

IsAlive:

```sh
curl http://localhost:5000/isalive
```

## Testing

To run the tests:

```sh
python -m unittest app_test.py
```