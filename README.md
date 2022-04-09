# airflow-helm
Helm chart for Airflow on OpenShift

## Installation
Add the Airflow helm repo
```
helm repo add apache-airflow https://airflow.apache.org
```
Get the helm charts for Airflow
```
helm pull apache-airflow/airflow
```

Unzip the resulting tgz file
```
tar xzf airflow-x.x.x.tgz
```

Copy the "values.yaml" files to their respective directories replacing the files there

├── charts
│   └── postgresql
│       └── values.yaml
└── values.yaml

Run the helm command from the airflow directory
```
helm upgrade --install airflow ./ --namespace airflow --values ./values.yaml
```


