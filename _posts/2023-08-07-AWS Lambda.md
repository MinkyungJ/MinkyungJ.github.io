---
title: AWS Lambda
tags: [DevOps, Lambda, Cold/Warm Start]
date: 2023-08-08 15:00 +0900
categories: [DevOps, Lambda]
toc: true
---

## ❗️ Lambda Operation

### 람다 동작 순서

람다 코드 작성 및 배포 -> 코드를 .Zip 파일로 업로드 -> 새로운 실행 환경(컨테이너) 조성 -> Bootstrap -> 핸들러 호출 및 실행
![Lambda Operation](https://github.com/MinkyungJ/MinkyungJ.github.io/blob/main/_posts/Lambda_Operation.png?raw=true)*Lambda Operation*

### 💡 Bootstrap 이란?

Lambda 함수의 진입점으로서 사용자 정의 런타임을 지정할때 사용되며 Lambda 환경으로부터 전달받은 이벤트를 처리하고, 사용자 함수를 호출하는 역할을 하는 실행 파일입니다.

> AWS Lambda는 서버리스 컴퓨팅 서비스로, 사용자 지정 런타임을 사용하여 Lambda 함수를 실행할 수 있습니다. 
> 사용자 지정 런타임은 AWS Lambda가 기본적으로 제공하는 Node.js, Python, Java 등과 같은 런타임이 아닌 사용자가 정의한 특정 런타임을 의미합니다.

---

## ❗️ 프로비저닝된 동시성(Lambda Provisioned Concurrency)

프로비저닝된 동시성은 함수에 할당하려는 사전 초기화된 실행 환경의 수입니다.
초기화 코드 실행까지 완전히 사용하기 전에 실행 환경을 생성하여 콜드 스타트 ​​추정을 최소화하며 API 호출에 소요되는 시간을 엄청나게 줄여주는 역할을 합니다.

> 💡 Lambda에서의 동시성: 함수가 동시에 처리하는 전송 중인 요청의 수

---

## ❗️ Warm Start

![Warm Start](https://github.com/MinkyungJ/MinkyungJ.github.io/blob/main/_posts/Warm_Start.png?raw=true)*Warm Start*

> 웜 스타트는 람다 함수가 이전에 호출되었고 일정 시간 이내에 다시 호출되어 초기화 과정을 생략할 수 있는 상태입니다.
> 즉, 이미 실행준비가 완료된 상태로 생성되어 있던 컨테이너의 코드를 재실행하는 것 뿐입니다.
> - 이전 호출의 성능과 상태가 영향을 미치므로, 반복 호출이 발생할 경우 람다 함수의 성능이 향상됩니다.

---

## ❗️ Cold Start

> 콜드 스타트는 람다 함수가 처음 호출되거나 일정 시간 동안 실행되지 않아 컨테이너를 시작하고 초기화 과정이 필요한 상태입니다.

![Cold Start](https://github.com/MinkyungJ/MinkyungJ.github.io/blob/main/_posts/Cold_Start.png?raw=true)*Cold Start*

1. 사용가능한 Warm 실행 환경이 없기 때문에 추가적인 단계가 필요합니다.
2. 서비스는 실행 가능한 환경을 조성하고(컨테이너) 함수 코드를 다운로드합니다.
3. 실행 환경을 시작 및 준비하고 **초기화(Initialize)** 코드를 실행합니다.
4. 핸들러를 호출하고 실행하여 호출을 완료합니다.

> 이와 같이 콜드 스타트는 웜 스타트에 비해서 많은 시간이 소요됩니다. 이를 콜드 스타트 대기 시간이라 부릅니다.

---

## ❗️ Reference

> Lambda Provisioned Concurrency
<https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/provisioned-concurrency.html>

> Lambda Operation
<https://aws.amazon.com/ko/blogs/compute/operating-lambda-performance-optimization-part-1/>
<https://quintagroup.com/blog/aws-lambda-provisioned-concurrency>

> Bootstrap
<https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/runtimes-custom.html>
