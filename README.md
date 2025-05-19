# ric

# O-RAN RIC Installation & Setup Guide

This repository provides a comprehensive guide for installing the O-RAN Near-Real-Time RIC (RIC-PLT) from scratch, deploying the E2 interface, integrating with gNB, and onboarding xApps using the official O-RAN Software Community (OSC) and third-party tools.

---

## 📦 Table of Contents

- [RIC Installation (from scratch)](#ric-installation-from-scratch)
- [RIC Deployment (from snapshot)](#deploying-ric-from-snapshot)
- [Fixing A1 Mediator Crash](#a1-mediator-issue)
- [E2 Interface Installation](#installing-e2-interface-to-connect-ran-node)
- [Integrating RIC and gNB](#integrating-ric-and-gnb)
- [xApp Deployment](#xapp-deployment)
- [Uninstall & Retry xApp](#uninstall-and-redeploy-xapp)
- [Author](#author)

---

## RIC Installation from Scratch

```bash
git clone https://gerrit.o-ran-sc.org/r/ric-plt/ric-dep
```
```bash
# install kubernetes, kubernetes-CNI, helm and docker

 cd ric-dep/bin
./install_k8s_and_helm.sh
# install chartmuseum into helm and add ric-common templates

./install_common_templates_to_helm.sh
```
```bash
sudo vim versions.txt
# Paste the following: 
nexus3.o-ran-sc.org:10004/o-ran-sc/ric-plt-a1:3.1.1
nexus3.o-ran-sc.org:10002/o-ran-sc/ric-plt-appmgr:0.5.7
nexus3.o-ran-sc.org:10002/o-ran-sc/ric-plt-dbaas:0.6.3
nexus3.o-ran-sc.org:10002/o-ran-sc/ric-plt-e2mgr:6.0.2
nexus3.o-ran-sc.org:10002/o-ran-sc/ric-plt-e2:6.0.3
nexus3.o-ran-sc.org:10002/o-ran-sc/ric-plt-rtmgr:0.9.5
nexus3.o-ran-sc.org:10002/o-ran-sc/ric-plt-submgr:0.9.6
nexus3.o-ran-sc.org:10002/o-ran-sc/ric-plt-vespamgr:0.7.5
nexus3.o-ran-sc.org:10002/o-ran-sc/ric-plt-o1:0.6.2
nexus3.o-ran-sc.org:10002/o-ran-sc/ric-plt-alarmmanager:0.5.15
nexus3.o-ran-sc.org:10002/o-ran-sc/it-dep-init:0.0.1
docker.io/prom/prometheus:v2.18.1
docker.io/kong/kubernetes-ingress-controller:0.7.0
docker.io/kong:1.4
docker.io/prom/alertmanager:v0.20.0
```
```bash
for i in `cat versions.txt`; do echo $i; docker pull $i; done
docker image list | grep nexus
```
```bash
#Edit the recipe file:


vi ../RECIPE_EXAMPLE/example_recipe_oran_h_release.yaml
## Replace ricip and auxip with your VM's local IP.
```

## Installing the RIC
After updating the recipe you can deploy the RIC with the command below. 
```bash
## Run the install script:
./install -f ../RECIPE_EXAMPLE/example_recipe_oran_h_release.yaml
```
## Checking the Deployment Status
```bash
kubectl get pods -A
# kubectl get pods -n ricplt
NAME                                               READY   STATUS             RESTARTS   AGE
deployment-ricplt-a1mediator-69f6d68fb4-7trcl      1/1     Running            0          159m
deployment-ricplt-appmgr-845d85c989-qxd98          2/2     Running            0          160m
deployment-ricplt-dbaas-7c44fb4697-flplq           1/1     Running            0          159m
deployment-ricplt-e2mgr-569fb7588b-wrxrd           1/1     Running            0          159m
deployment-ricplt-e2term-alpha-db949d978-rnd2r     1/1     Running            0          159m
deployment-ricplt-jaegeradapter-585b4f8d69-tmx7c   1/1     Running            0          158m
deployment-ricplt-rsm-755f7c5c85-j7fgf             1/1     Running            0          158m
deployment-ricplt-rtmgr-c7cdb5b58-2tk4z            1/1     Running            0          160m
deployment-ricplt-submgr-5b4864dcd7-zwknw          1/1     Running            0          159m
deployment-ricplt-vespamgr-864f95c9c9-5wth4        1/1     Running            0          158m
r3-infrastructure-kong-68f5fd46dd-lpwvd            2/2     Running            3          160m


```

## Installing E2 interface to connect RAN Node:
Repo link : 
```bash
https://github.com/o-ran-sc/sim-e2-interface/tree/h-release/e2sim

#   sudo apt-get update
#   sudo apt-get install -y build-essential git cmake libsctp-dev lksctp-tools autoconf automake libtool bison flex libboost-all-dev
#   sudo apt-get clean
#   apt-get install cmake g++ libsctp-dev
#     git clone “https://github.com/o-ran-sc/sim-e2-interface.git” 
#     cd sim-e2-interface/e2sim
#    vi e2sm_examples/kpm_e2sm/Dockerfile     [modify the last line to sleep 100000000]
#    mkdir build
#    cd build/
#   cmake .. && make package && cmake .. -DDEV_PKG=1 && make package
#   cp *.deb ../e2sm_examples/kpm_e2sm/
#   cd ../e2sm_examples/kpm_e2sm/
#   docker build -t oransim:0.0.999 .
#   docker run -d --name oransim -it oransim:0.0.999

```

