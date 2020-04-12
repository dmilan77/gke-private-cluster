```
export NETWORK_NAME=gke-network-02
gcloud compute --project=data-protection-01 networks create ${NETWORK_NAME} --subnet-mode=custom
gcloud compute networks subnets create my-subnet-2 \
    --network ${NETWORK_NAME}\
    --region us-east1 \
    --range 192.168.0.0/20 \
    --secondary-range my-pods-2=10.4.0.0/14,my-services-2=10.0.32.0/20 \
    --enable-private-ip-google-access

gcloud compute firewall-rules create ${NETWORK_NAME}-allow-external \
	  --allow tcp:22,tcp:6443,icmp \
	  --network ${NETWORK_NAME} \
	  --source-ranges 0.0.0.0/0

gcloud container clusters create private-cluster-1 \
    --zone us-east1-b \
    --enable-master-authorized-networks \
    --network ${NETWORK_NAME} \
    --subnetwork my-subnet-2 \
    --machine-type "n1-standard-1" \
    --cluster-secondary-range-name my-pods-2 \
    --services-secondary-range-name my-services-2 \
    --enable-private-nodes \
    --enable-private-endpoint \
    --enable-ip-alias \
    --master-ipv4-cidr 172.16.0.16/28 \
    --no-enable-basic-auth \
    --no-issue-client-certificate




gcloud container clusters update private-cluster-1 \
    --zone us-east1-b \
    --enable-master-authorized-networks \
    --master-authorized-networks 192.168.0.6/32,192.168.0.7/32

```
