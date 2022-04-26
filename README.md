# Release Notes:
## 0.1
- Updated helm templates to use storage classes for local storage
- Hard codded storage class for postgres temporarily
- Updated README to reflect new instructions. This version runs directly from this source, no need to pull the official charts

## 0.0
- Namespace creation with security context

# airflow-helm
Helm chart for Airflow on OpenShift

## Prerequsites for SNO
- Install the local storage operator
- Create 3 partitions on another disk device, 8GB, 100GB, and 1GB
```
sda           8:0    0 953.9G  0 disk 
├─sda1        8:1    0     8G  0 part
├─sda2        8:2    0   100G  0 part
└─sda3        8:3    0     1G  0 part
```
- Format the partitions as xfs
```
mkfs -t xfs /dev/sda1
mkfs -t xfs /dev/sda2
mkfs -t xfs /dev/sda3
```


## Installation
1. Run the create yaml for the namespace. *IMPORTANT: This sets up the UIDs for the proper security context definitions
```
oc create -f airflow-namespace.yaml
```
2. Create the pv's for airflow
```
oc create -f airflow-pv-file.yaml
```
3. Run the helm command from the airflow directory
```
helm upgrade --install airflow ./ --namespace airflow --values ./values.yaml
```


