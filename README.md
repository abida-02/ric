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
git clone https://gerrit.o-ran-sc.org/r/ric-plt/ric-dep```
```bash
 cd ric-dep/bin
./install_k8s_and_helm.sh
./install_common_templates_to_helm.sh```
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


```bash
## Run the install script:
./install -f ../RECIPE_EXAMPLE/example_recipe_oran_h_release.yaml
```
## Checking the Deployment Status
```bash
kubectl get pods -A
```


