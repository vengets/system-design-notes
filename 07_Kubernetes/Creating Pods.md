---
created: 2026-06-22 23:52
updated:
tags:
  - kubernetes
status: active
source:
related:
---


> [!note]
> Kubernetes should be studied as a distributed orchestration system, not just kubectl commands.

## Purpose

Set the label, image with version (latest), give container name(for this we have to edit yaml). 

thor@jump-host ~$ kubectl run pod-httpd --image=httpd:latest --labels="app=httpd_app" --dry-run=client -o yaml > pod.yaml
thor@jump-host ~$ vi pod.yaml 
thor@jump-host ~$ kubectl apply -f pod.yaml 


