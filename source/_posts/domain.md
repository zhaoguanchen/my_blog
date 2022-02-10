---
title: AWS Route 53（域名）
date:
updated:
tags: AWS
categories: AWS
keywords: domain
description:
top_img:
comments: true
cover:
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:
---

# Domain

## TransferDomainToAnotherAwsAccount

aws间域名迁移

采用`aws cli`的方式：

- 原账号下发起域名迁移

参数为域名和账户ID

```shell
aws route53domains transfer-domain-to-another-aws-account --region us-east-1 --domain-name <name> --account-id <11111>
```

- 返回操作ID和password（保存备用）

```json
{
    "OperationId": "xxxxxxx",
    "Password": "xxxxx"
}
```

- 新账号下域名接收

参数为域名和返回的password

```bash
aws route53domains  accept-domain-transfer-from-another-aws-account --region us-east-1 --domain-name <name> --password "xxxxxx"
```

- 操作结束，此时域名迁移至新aws账号下。

```json
{
    "OperationId": "e922c85c-ce2a-45b2-9f11-a32d38ac5079"
}
```



 

Reference:

https://stackoverflow.com/questions/62785304/route53-transfer-domain-to-another-aws-account