# Release Notes:
# OCP SNO branch
## 0.3
- Branched off into SNO specific branch
- Updated templates to use LVM volume names instead of device names. Follow the convention ``/dev/vg01/postgres-data`` etc...

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
- Use LVM to create 3 partitions on another disk device, 8GB, 100GB, and 1GB
  - pvcreate pv01 /dev/sda
  - vgcreate vg01 /dev/sda
  - lvcreate -L 8G -n postgres-data vg01
  - lvcreate -L 100G -n workers-data vg01
  - lvcreate -L 1G -n redis-data vg01
```
  LV            VG   Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  postgres-data vg01 -wi-a-----   8.00g                                                    
  redis-data    vg01 -wi-a-----   1.00g                                                    
  workers-data  vg01 -wi-a----- 100.00g 
```
- Format the partitions as xfs
```
mkfs -t xfs /dev/vg01/postgres-data
mkfs -t xfs /dev/vg01/workers-data
mkfs -t xfs /dev/vg01/redis-data
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


