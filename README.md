```
gcloud compute --project=data-protection-01 networks create gke-network-01 --subnet-mode=custom

gcloud compute --project=data-protection-01 networks subnets create master-1 --network=gke-network-01 --region=us-east1 --range=10.1.0.0/24 --secondary-range=RANGE_NAME=172.2.0.0/24

gcloud iam service-accounts create kubernetes-sa-admin --display-name kubernetes-sa-admin
gcloud projects add-iam-policy-binding "data-protection-01" \
  --member serviceAccount:kubernetes-sa-admin@data-protection-01.iam.gserviceaccount.com \
  --role roles/compute.networkUser


gcloud  container clusters create "gke-dmilan-app-dev-useast-1" \
--project "data-protection-01" \
--zone "us-east1-b" \
--cluster-version "1.14.10-gke.27" \
--machine-type "n1-standard-1" \
--disk-type "pd-standard" \
--disk-size "20" \
--num-nodes "3" \
--enable-private-nodes \
--enable-private-endpoint \
--master-ipv4-cidr "10.1.0.0/28" \
--enable-ip-alias \
--network "projects/data-protection-01/global/networks/gke-network-01" \
--subnetwork "projects/data-protection-01/regions/us-east1/subnetworks/master-1" \
--cluster-secondary-range-name "pod-1" \
--services-secondary-range-name "services-1" \
--enable-master-authorized-networks \
--service-account "kubernetes-sa-admin@data-protection-01.iam.gserviceaccount.com"

```
