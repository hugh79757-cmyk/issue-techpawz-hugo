---
title: 워드프레스 블로그 ads.txt 파일 오류, Ad Inserter로 해결하는 방법
date: '2025-06-03T16:08:40+09:00'
slug: 워드프레스-블로그-ads-txt-파일
categories: [Business]
tags: []
draft: false
description: ''
---

워드프레스 블로그 ads.txt 파일 - 애드센스를 처음 연동한 많은 워드프레스 사용자들이 한 번쯤은 경험하게 되는 오류가 있습니다. 바로 "ads.txt 파일을 찾을 수 없음" 이라는 경고 메시지입니다. 이는 블로그 수익화에 필수적인 설정 중 하나로, 광고주의 신뢰를 높이고 광고 송출 오류를 줄이기 위해 꼭 해결해야 할 문제입니다. 오늘은 워드프레스 인기 광고 플러그인인 **Ad Inserter**를 이용해 ads.txt 파일 오류를 간단하게 해결하는 방법을 안내드립니다.

[애드 인서터 광고위치 미세조정하기](https://issue.techpawz.com/%ec%9b%8c%eb%93%9c%ed%94%84%eb%a0%88%ec%8a%a4-%eb%b8%94%eb%a1%9c%ea%b7%b8-%ec%95%a0%eb%93%9c%ec%9d%b8%ec%84%9c%ed%84%b0-%ed%94%8c%eb%9f%ac%ea%b7%b8%ec%9d%b8/)

[애드 인서터 소개](https://ko.wordpress.org/plugins/ad-inserter/)

## 글의 구성

- [워드프레스 블로그 ads.txt 파일](#워드프레스-ads-txt-파일-오류-해결방법)
  - [ads.txt 파일이란?](#ads-txt-파일이란)
  - [Ad Inserter를 이용한 ads.txt 등록 방법](#ad-inserter를-이용한-ads-txt-등록-방법)
  - [자주 묻는 질문](#자주-묻는-질문)
- [마무리하며](#마무리하며)

## 워드프레스 블로그 ads.txt 파일

### **ads.txt 파일이란?**

ads.txt(Authorized Digital Sellers)는 사이트 소유자가 자신의 광고 공간을 판매할 수 있는 플랫폼과 계정을 명시하는 파일입니다. 이 파일이 없으면 구글 애드센스가 광고를 제대로 송출하지 않거나 수익이 제한될 수 있습니다.

### **Ad Inserter를 이용한 ads.txt 등록 방법**

**1. Ad Inserter 플러그인 설치 및 활성화**

- 워드프레스 관리자 페이지로 접속합니다.
- 좌측 메뉴에서 **플러그인 > 새로 추가**로 이동해 `Ad Inserter`를 검색합니다.
- 플러그인을 설치하고 **활성화** 버튼을 클릭합니다.

**2. ads.txt 파일 생성 및 입력 준비**

- 애드센스 관리자 페이지에서 다음 형식의 코드를 복사합니다.

```
google.com, pub-xxxxxxxxxxxxxxxx, DIRECT, f08c47fec0942fa0
```

- 이때 `pub-xxxxxxxxxxxxxxxx` 부분은 본인의 애드센스 퍼블리셔 ID로 대체되어야 합니다.

**3. Ad Inserter로 ads.txt 내용 입력하기**

- 워드프레스 관리자 화면에서 **설정 > Ad Inserter**로 이동합니다.
- 상단 탭에서 **"Ads.txt"** 메뉴를 클릭합니다.
- 해당 입력란에 복사해둔 ads.txt 내용을 그대로 붙여넣습니다.
- 예시:

```
google.com, pub-1234567890123456, DIRECT, f08c47fec0942fa0
```

- 하단의 **Save Settings(설정 저장)** 버튼을 클릭합니다.

![워드프레스 블로그 ads.txt 파일](https://issue.techpawz.com/wp-content/uploads/2025/06/img_d61e8b1c-1024x272.webp)

애드센스 홈페이지에서 사이트 메뉴에서 문제가 되는 사이트를 클릭하면 위와 같은 화면을 볼 수 있습니다. 지금 해결하기를 누르면 아래의 페이지로 이동할 수 있습니다.

![워드프레스 블로그 ads.txt 파일](https://issue.techpawz.com/wp-content/uploads/2025/06/img_be93bfed-1024x665.webp)

이곳에서 복사하기를 누르고 AD Inserter 플러그 인에서 위의 코드를 입력하고 Ads.txt 파일을 편집, 업로드까지 할 수 있습니다. 업로드 후 '업데이트 확인' 버튼을 눌러주면 아래와 같은 결과를 얻을 수 있습니다.

![워드프레스 블로그 ads.txt 파일](https://issue.techpawz.com/wp-content/uploads/2025/06/img_4ece3502-1-1024x846.webp)

**4. 정상 적용 확인하기**

- 브라우저 주소창에 `https://내도메인/ads.txt`를 입력해 저장한 파일 내용이 정상 출력되는지 확인합니다.
- 입력한 퍼블리셔 ID가 그대로 보이면 설정이 완료된 것입니다.
- 구글 애드센스 홈페이지에서 확인할 수 있습니다.

### **자주 묻는 질문**

- **ads.txt를 워드프레스 루트에 직접 업로드해도 되나요?**  
  가능하지만 FTP 접근이 필요하며, Ad Inserter를 사용하면 훨씬 간편하게 관리할 수 있습니다.
- **여러 광고 네트워크를 사용하는 경우는요?**  
  각 네트워크에서 제공하는 ads.txt 정보를 줄마다 추가해주면 됩니다.
- **설정 후 애드센스 오류 메시지가 바로 사라지지 않아요.**  
  보통 구글 측에서 반영되기까지 24~72시간 정도 걸릴 수 있습니다. 기다려 보세요.

## **마무리하며**

ads.txt 파일은 단순한 텍스트 한 줄이지만, 블로그 수익화의 신뢰 기반을 다지는 중요한 요소입니다. 워드프레스를 사용 중이라면 Ad Inserter 플러그인을 통해 누구나 손쉽게 설정할 수 있으며, 관리도 편리합니다. 아직 설정하지 않으셨다면, 지금 바로 설정하시고 애드센스 광고가 안정적으로 송출되도록 준비해보시기 바랍니다.

[워드프레스 블로그 애드인서터 플러그인 광고 위치 조정 방법 (+썸네일과 광고 겹침 해결)](https://issue.techpawz.com/%ec%9b%8c%eb%93%9c%ed%94%84%eb%a0%88%ec%8a%a4-%eb%b8%94%eb%a1%9c%ea%b7%b8-%ec%95%a0%eb%93%9c%ec%9d%b8%ec%84%9c%ed%84%b0-%ed%94%8c%eb%9f%ac%ea%b7%b8%ec%9d%b8/)

[워드프레스 블로그 속도가 느릴 때 확인해 볼 것 5가지](https://issue.techpawz.com/%ec%9b%8c%eb%93%9c%ed%94%84%eb%a0%88%ec%8a%a4-%eb%b8%94%eb%a1%9c%ea%b7%b8-%ec%86%8d%eb%8f%84/)

[ChatGPT 챗GPT 사업자 세금 혜택 부가세 면제 받기](https://issue.techpawz.com/%ec%b1%97gpt-%ec%82%ac%ec%97%85%ec%9e%90-%ec%84%b8%ea%b8%88-%ed%98%9c%ed%83%9d/)