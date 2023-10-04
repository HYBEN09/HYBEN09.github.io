---
title: Mac에서 VMware Fusion 13으로 Rocky Linux 9.2 설치
author: hyebeen
date: 2023-10-02 22:48:00 +0800
categories: [Linux, RockyLinux]
tags: [linux]
image: /assets/img/posts/rockyLinux.png
---

Mac에서 VMware Fusion 13으로 Rocky Linux 9.2 Version 설치

## 1. VMware Fusion 설치

- VMware 회원가입 후 [VMware Fusion Player – Personal Use License](https://customerconnect.vmware.com/en/evalcenter?p=fusion-player-personal-13) 통해 회원가입
- 로그인 하면 하단에 라이센스 & 다운로드에서 개인의 라이센스 키 확인합니다
  <img src="/assets/img/posts/linux/mac_linux-1.png"/>
- 하단에 다운로드를 클릭하여 다운로드를 해줍니다. <br/>
  [참고] 개인 무료 라이센스로 사용하기 때문에 따로 추가적인 비용은 들어가지 않습니다.
  <img src="/assets/img/posts/linux/mac_linux-2.png"/>
- dmg 설치파일을 다운로드를 하고 더블 클릭 후 VMware Fusion을 선택합니다.
  <img src="https://i0.wp.com/endoflinux.com/wp-content/uploads/2023/07/4.dmg-start.png?resize=768%2C495&ssl=1"/>
- VMware Fusion 13 개인 라이센스 키를 통해서 인증해줍니다.
  <img src="https://i0.wp.com/endoflinux.com/wp-content/uploads/2023/07/5.License-key-enter.png?w=722&ssl=1"/>

## 2. VMware Fusion13을 이요하여 Rocky Linux 가상머신 생성

- [Download Rocky](https://rockylinux.org/ko/download) 에 들어가서 ARM64를 다운로드 해줍니다.
- Apple Silicon 칩셋을 사용하는 기기에서는 ARM 아키텍처를 사용하므로, 일반적인 x86_64 아키텍처의 리눅스 배포판은 호환되지 않습니다.
  <img src="/assets/img/posts/linux/mac_linux-3.png"/>

- 다운로드 받은 ARM64 (aarch64)을 VMware Fusion에 적용해 줍니다.
  <img src="/assets/img/posts/linux/mac_linux-4.webp"/>

- Rocky Linux의 경우 ARM 버전으로 선택하여 설치를 진행해줍니다.
  <img src="/assets/img/posts/linux/mac_linux-5.webp"/>

- Finish를 클릭해줍니다
  <img src="/assets/img/posts/linux/mac_linux-6.webp"/>

- 'Install Rocky Linux 9.2'를 Enter
- 만약에 커서가 사라진다면 Control + command를 동시에 눌러주면 됩니다.
  <img src="/assets/img/posts/linux/mac_linux-7.webp"/>

- 한국어를 선택해줍니다.
  <img src="/assets/img/posts/linux/mac_linux-8.webp"/>

- 파티션과 비밀번호, 사용자 생성을 해줍니다.
  <img src="/assets/img/posts/linux/mac_linux-9.png"/>

- 이렇게 Rocky Linux가 설치되었습니다!
  <img src="/assets/img/posts/linux/mac_linux-10.png"/>
  <img src="/assets/img/posts/linux/mac_linux-11.png"/>

### 참고

[참고사이트:M1, M2맥(Mac) 애플실리콘 가상화를 위한 VMware Fusion 프로그램 설치방법](https://endoflinux.com/m1-m2%eb%a7%a5mac-%ec%95%a0%ed%94%8c%ec%8b%a4%eb%a6%ac%ec%bd%98-%ea%b0%80%ec%83%81%ed%99%94%eb%a5%bc-%ec%9c%84%ed%95%9c-vmware-fusion-%ed%94%84%eb%a1%9c%ea%b7%b8%eb%9e%a8-%ec%84%a4%ec%b9%98/#VMware_Fusion%EC%9D%B4%EB%9E%80)
