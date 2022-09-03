---
categories:
  - books
  - terraform
tags:
  - terraform
  - azure
layout: post
hidden: true
title: Deep-Dive Terraform on Azure
---

Deep-Dive Terraform on Azure: Automated Delivery and Deployment of Azure Solutions is a good book that will help you incorporate the power of Terraform into the Azure cloud. While it does cover some of Azure basics and of Terraform, this book is aimed towards cloud engineers already familiar with Azure, and some knowledge of infrastructure as code concepts. While I'm not new to Azure or Terraform, I definitely learned important concepts on how to properly implement Terraform in Azure.

The book covers core Terraform concepts like resources, data sources, providers, modules, versioning, and state management.
As well as more advance topics like security, testing, and CI/CD pipelines. It goes in depth into the best practices of implementing Terraform efficiently in Azure. One section I will definitely will come back to re-read, is the section on testing Terraform using <a href="https://terratest.gruntwork.io/" target="_blank">Terratest</a>, given that it requires having knowledge of Golang. (and the fact that I've had a long interest in learning Go, and now I have the perfect excuse to do so!)

My only negative on this book is on the CI/CD portion, it's using Azure Pipelines as the CI/CD platform. It's quite clear that the community has chosen GitHub Actions as the defacto CI/CD platform, and also the fact that it's a Microsoft Product, I would've liked this chapter used GitHub Actions instead of Azure Pipelines. Thus said, it's quite easy to follow along using GitHub Actions without much effort, and the principles can be applied regardless of the CI/CD platform.

### Rating: 3/5

Deep-Dive Terraform on Azure: Automated Delivery and Deployment of Azure Solutions

<a href="https://link.springer.com/book/10.1007/978-1-4842-7328-9" target="_blank"><img src="/assets/books/deep-dive-terraform-on-azure.jpg"></a>

* Chapter 1: Infrastructure as Code
* Chapter 2: Azure and Terraform
* Chapter 3: Getting Started with Terraform
* Chapter 4: Deep-Dive into Terraform
* Chapter 5: Modules
* Chapter 6: Writing Secure Scripts with Terraform
* Chapter 7: CI/CD with Terraform
* Chapter 8: Terraform Unit Testing
* Chapter 9: Terraform Best Practices
