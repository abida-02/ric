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
cd ric-dep/bin
./install_k8s_and_helm.sh
./install_common_templates_to_helm.sh
sudo vim versions.txt
