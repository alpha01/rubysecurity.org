---
categories:
  - cloud
  - aws-cli
tags:
  - aws
  - python
layout: post
title: aws cli AuthFailure gotcha
created: 1485746900
---

It's probably been about 5-6 years since the last time I've used the AWS command line tools. The other day I signed up for the <a href="https://aws.amazon.com/free/" target="_blank">AWS Free Tier</a> to familiarize myself with the aws command-line tools once again. I created myself a test user and granted full access to my aws account. Easy and simple. Well not so fast, initially my user account failed to authenticate properly.  I tried re-generating new API keys and even created other different accounts with different types of access, but the problem persisted.

### Problem:

I was getting the following error:

```bash
[root@bashninja ~]# aws ec2 describe-regions

An error occurred (AuthFailure) when calling the DescribeRegions operation: AWS was not able to validate the provided access credentials
```

I verified my AWS API credentials stored on ~/.aws/credentials to ensure everything was correct. Yet, the fucking problem persisted.

### Fix:

It turned out that the time on the system I was trying to use the aws cli tools in, had the time completely wrong! Once the time was fixed I was able to authenticate my account and use the aws cli tool.
