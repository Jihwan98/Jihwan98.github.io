---
title: "[Infra] AWS 서버 생성!"
author: JIHWAN PARK
categories: [Backend, Infra]
tag: [Backend, AWS]
math: true
date: 2023-06-08 15:55:34 +0900
last_modified_at: 2023-06-08 15:55:34 +0900
image: "https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/bb10f126-575d-4a3c-be54-66566a6b91d9"
---
> AWS 서버 생성을 해보자!
{: .prompt-info}

[[AWS] 가장쉽게 VPC 개념잡기](https://medium.com/harrythegreat/aws-%EA%B0%80%EC%9E%A5%EC%89%BD%EA%B2%8C-vpc-%EA%B0%9C%EB%85%90%EC%9E%A1%EA%B8%B0-71eef95a7098)

AWS 세팅하기 전에 위의 글을 보면 무슨 작업을 하는지 이해하기 수월한 것 같다.

전체적으로 VPC 생성 -> 서브넷 생성 -> 인터넷 게이트웨이 생성 -> EC2 인스턴스 생성 순서로 진행한다.

## ✅ AWS 로그인
[https://aws.amazon.com/](https://aws.amazon.com/) 로 가서 로그인을 하자.
계정이 없다면 무료 계정을 생성하면 됨!

## ✅ VPC 생성
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/75c325d1-1093-4002-8f79-9c0c94ed998c)
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/8275df1e-4c40-4d1e-a139-6ffc31f6c3f6)
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/6d4162c6-f0ad-473e-986b-38331e72e4ee)

## ✅ 서브넷 생성
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/41f8ec27-fb89-4851-a978-7c72ecf2262f)

![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/72e82721-b0fd-4c6e-9afd-c63da9d22877)
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/b8b0500d-2fa3-4b21-b293-ce4fc9fc0c95)
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/1b9ccfe0-1837-4281-98c0-a9e8efe2964c)
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/fb506726-f999-4cda-8a29-e199e1ac0c23)
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/9d4ab9bc-6cc1-48f1-99f0-9b280aa8ddef)
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/43b68a3a-3cfa-492a-b1d4-f73c3306531b)

## ✅ 인터넷 게이트웨이 생성
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/fde9d878-798d-49ef-ae22-71c52d8adab1)
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/7e9a00a7-f7d2-408c-aafb-6ee3face0155)
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/8c7a29a1-fd87-4e57-95a9-d7ba65112bd4)
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/4361f538-a754-44ab-a35e-d44687af95b8)

![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/6d03780e-4fe9-449e-8a4b-5aaedd42c78b)

![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/394628f5-d843-4dbf-b8a0-8e72eef5b442)

![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/8ffc0649-a44d-4307-8e75-68b6559ba862)

![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/4e8a8a94-acd9-413a-b689-1ed60f612105)

![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/c4b63e4e-a20e-49be-9867-b638f31e1e28)

![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/b3759ed8-4871-40bf-a3d2-a5ce34467439)

![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/100e2344-c55d-4447-90fa-09ef6c4af854)
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/61e5c854-bff4-43ed-85c3-9f8e064ac0ef)
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/0e1236e2-c5a5-4941-bc52-e7b3aec931c4)
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/2cecca3d-e65e-4042-9394-7cef66183c99)

## ✅ EC2 인스턴스 생성
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/418ac97f-73b3-41a8-9fa8-288ad593b6dc)
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/8fcb5803-10dd-4ae4-abe9-c81aa92e93c4)
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/ce689bd4-92ae-4d23-a6d9-8a85c4bfe3d5)
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/30ba10db-af4d-4d1f-b407-334b15ae0dc8)

### ✔️ 키페어

- ppk로 생성

![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/6e1676dd-4f35-4884-a6c6-8b4742639199)

### ✔️ 네트워크설정
![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/0cb80333-b350-495e-92c0-0ff7d717e46a)

- 본인이 생성한 vpc와 서브넷 설정에 주의하고
- 퍼블릭 ID 자동할등은 반드시 활성화 해야함

![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/55488ea2-3471-4372-9d9b-73e27fa25dfc)

- SSH를 위치 무관하게 설정
- 실무에서는 보안상 이렇게 사용하지는 않음
- 팀 과제로 다같이 들어가기 위해 준비

![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/9babf742-01d3-4c83-9bf8-36112d7602d7)

- HTTP를 위치무관으로 추가
- 브라우저에서 웹 접속을 허용하기 위해 80포트를 추가

![image](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/8d2784b2-c6d4-4557-a275-498579f32ba1)

이후 <mark>인스턴스 시작</mark>!