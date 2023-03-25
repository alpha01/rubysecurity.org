---
categories:
  - books
  - kubernetes
  - kustomize
tags:
  - kubernetes
  - kustomize
layout: post
hidden: true
title: Leveraging Kustomize for Kubernetes Manifests
---

<a href="https://kustomize.io/" target="_blank">Kustomize</a> is one of tools that if I don't use it often, resulting in me forgetting how it works. The last time I used this useful Kubernetes tool was about 2.5 years ago. At the time I used it to modify (patch) Helm charts, without having to touch the original chart templates. So at a high level, I knew what Kustomize is capable of doing, and what it does not do. I finally came into a situation where I needed to use the functionality of this awesome tool, so I decided to read this short O'Reilly book (or article given its length).

This short 46 page book is practically all you need to learn <a href="https://kustomize.io/" target="_blank">Kustomize</a>. It is divided into two chapters. The first chapter is a brief overview on the problems Kustomize is trying to solve, and how it differs from <a href="https://helm.sh/" target="_blank">Helm</a>. The second chapter is where most important content is covered in. In hindsight <a href="https://kustomize.io/" target="_blank">Kustomize</a> is a simple tool. The author does an excellent job describing the tool's usage. With easy to follow, comprehensive examples. Anyone, even with basic level of Kubernetes knowledge can read this book, and start incorporating it in their application deployment workflow almost instantly. Reading this book along the official <a href="https://kubectl.docs.kubernetes.io/guides/" target="_blank">Kustomize Documentation</a>, is practically all its needed to become efficient in Kustomize. All while in less time their is to finish your cup of coffee!

### Rating: 4/5

Leveraging Kustomize for Kubernetes Manifests

<a href="https://www.oreilly.com/library/view/leveraging-kustomize-for/9781098117078/" target="_blank"><img src="/assets/books/leveraging-kustomize-for-kubernetes-manifests.jpg"></a>

* Chapter 1: Kubernetes Application Deployment and Reuse
* Chapter 2: Key Strategic Functionality Provided by Kustomize
