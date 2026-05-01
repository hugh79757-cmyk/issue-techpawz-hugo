---
title: 구글 디스커버 노출 RSS 메타태그 설정 방법
date: '2025-05-30T18:53:19+09:00'
slug: 구글-디스커버-노출-rss
categories: [adsense]
tags: []
draft: false
description: ''
---

구글 디스커버 노출 RSS - 구글 디스커버(Google Discover)에 콘텐츠를 더 안정적으로 노출시키고 싶다면, **RSS 또는 Atom 피드와 메타태그 설정**을 반드시 확인해야 합니다. 디스커버는 사용자가 팔로우한 웹사이트의 피드를 기반으로 콘텐츠를 추천하는 구조도 함께 운영하고 있기 때문에, **피드 최적화는 노출 확률을 높이는 중요한 기술적 요소**입니다.

이 글에서는 디스커버에 노출되기 위한 **RSS 피드 설정 방법**, **메타태그 삽입법**, **서브도메인 대응 전략**까지 구체적으로 안내드립니다.

*[구글 디스커버 공식 가이드라인](https://developers.google.com/search/docs/appearance/google-discover?hl=ko)*

[구글 디스커버란? 검색 없이 콘텐츠가 노출되는 구조의 비밀](https://issue.techpawz.com/%ea%b5%ac%ea%b8%80-%eb%94%94%ec%8a%a4%ec%bb%a4%eb%b2%84/)

[구글 디스커버 노출되기 위한 정책과 자격 요건 총정리](https://issue.techpawz.com/%ea%b5%ac%ea%b8%80-%eb%94%94%ec%8a%a4%ec%bb%a4%eb%b2%84-%eb%85%b8%ec%b6%9c/)

[디스커버 최적화를 위한 콘텐츠 작성법 제목 이미지 신뢰도 포인트](https://issue.techpawz.com/%eb%94%94%ec%8a%a4%ec%bb%a4%eb%b2%84-%ec%b5%9c%ec%a0%81%ed%99%94/)

- [구글 디스커버 노출 RSS 역할](#구글-디스커버-노출-rss-역할) 
  - [구글 크롬 팔로우 기능은 다음 조건에서 작동합니다:](#구글-크롬-팔로우-기능은-다음-조건에서-작동합니다)
- [기본 메타태그 삽입 방법 ( 태그 내부)](#기본-메타태그-삽입-방법-태그-내부)
  - [주의사항:](#주의사항)
- [서브도메인 대응 방법](#서브도메인-대응-방법)
  - [예시:](#예시)
- [워드프레스, 티스토리 등에서 적용하는 방법](#워드프레스-티스토리-등에서-적용하는-방법)
  - [워드프레스](#워드프레스)
  - [티스토리](#티스토리)
- [RSS 피드 최적화를 위한 3가지 체크리스트](#rss-피드-최적화를-위한-3-가지-체크리스트)
- [마무리 – RSS 메타태그 설정은 디스커버 노출의 시작점](#마무리-rss-메타태그-설정은-디스커버-노출의-시작점)

## 구글 디스커버 노출 RSS 역할

구글 디스커버는 사용자의 관심사 기반으로 콘텐츠를 추천할 뿐만 아니라,  
**사용자가 직접 웹사이트를 ‘팔로우’했을 때 해당 사이트의 RSS 피드를 기반으로 콘텐츠를 디스커버에 노출**합니다.

### 구글 크롬 팔로우 기능은 다음 조건에서 작동합니다:

- Android 기반의 Chrome 앱
- 사이트에 RSS 또는 Atom 피드가 설정되어 있어야 함

이 기능은 사용자가 **크롬에서 팔로우 중인 사이트의 콘텐츠**를 ‘팔로우 중’ 탭에 노출시켜주며, 해당 사이트의 신선한 콘텐츠를 우선 노출하는 구조로 작동합니다.

## 기본 메타태그 삽입 방법 (`<head>` 태그 내부)

웹페이지에 다음과 같은 RSS 링크 메타태그를 추가합니다:

```
<link rel="alternate" type="application/rss+xml" href="https://example.com/feed" />
```

이 태그는 Google에게 **“이 페이지의 RSS 피드는 여기 있습니다”** 라는 정보를 제공하는 역할을 하며, 크롤러가 정확하게 콘텐츠 흐름을 추적할 수 있게 도와줍니다.

### 주의사항:

- `<head>` 태그 안에 삽입해야 하며,
- 피드 주소가 404 또는 robots.txt로 차단되면 안 됩니다.

## 서브도메인 대응 방법

Google은 서브도메인을 **독립적인 사이트로 인식**하기 때문에,  
`blog.example.com`, `news.example.com`처럼 여러 서브도메인을 운영 중이라면, **각 서브도메인마다 RSS 피드를 따로 등록**해야 합니다.

### 예시:

```
<link rel="alternate" type="application/rss+xml" href="https://blog.example.com/feed" />
<link rel="alternate" type="application/rss+xml" href="https://news.example.com/feed" />
```

이렇게 하면 크롬이나 디스커버에서 해당 피드를 통해 각각의 콘텐츠 흐름을 추적할 수 있게 됩니다.

## 워드프레스, 티스토리 등에서 적용하는 방법

### 워드프레스

- 기본적으로 `https://example.com/feed` 형태의 RSS 피드가 자동 생성됨
- functions.php에 아래 코드 추가로 `<head>`에 메타태그 자동 삽입 가능:

```
add_action('wp_head', function () {
  echo '<link rel="alternate" type="application/rss+xml" href="' . get_bloginfo('rss2_url') . '" />';
});
```

### 티스토리

- 기본 피드 주소: `https://블로그주소.tistory.com/rss`
- 스킨 HTML 편집 → `<head>` 영역에 수동으로 삽입

---

## RSS 피드 최적화를 위한 3가지 체크리스트

| 항목 | 설명 |
| --- | --- |
| 피드 접근 가능 여부 | robots.txt나 비공개 설정 금지 |
| 최신 콘텐츠 포함 여부 | 글 발행 시 자동 반영되도록 설정 유지 |
| 피드 제목/링크 구성 | 구체적인 제목, 유효한 링크 포함 필요 |

---

![구글 디스커버 노출 RSS](https://issue.techpawz.com/wp-content/uploads/2025/05/image_20250530_2-2-1024x1024.webp)

## 마무리 – RSS 메타태그 설정은 디스커버 노출의 시작점

구글 디스커버에서의 노출은 단순히 좋은 콘텐츠만으로 이루어지지 않습니다.  
**구글이 콘텐츠를 인식하고 추적할 수 있도록 돕는 RSS 메타태그 설정**은  
디스커버에서의 노출 가능성을 높여주는 중요한 기반이 됩니다.

RSS 피드를 아직 구성하지 않았거나, `<link rel="alternate">` 태그가 누락되어 있다면 지금 바로 확인해보세요. 다음 글에서는 **Search Console을 통해 디스커버 유입을 어떻게 분석할 수 있는지**를 안내드립니다. 기술적 설정이 끝났다면, 이제는 데이터를 읽고 전략을 세울 차례입니다.

[구글 디스커버란? 검색 없이 콘텐츠가 노출되는 구조의 비밀](https://issue.techpawz.com/%ea%b5%ac%ea%b8%80-%eb%94%94%ec%8a%a4%ec%bb%a4%eb%b2%84/)

[구글 디스커버 노출되기 위한 정책과 자격 요건 총정리](https://issue.techpawz.com/%ea%b5%ac%ea%b8%80-%eb%94%94%ec%8a%a4%ec%bb%a4%eb%b2%84-%eb%85%b8%ec%b6%9c/)

[디스커버 최적화를 위한 콘텐츠 작성법 제목 이미지 신뢰도 포인트](https://issue.techpawz.com/%eb%94%94%ec%8a%a4%ec%bb%a4%eb%b2%84-%ec%b5%9c%ec%a0%81%ed%99%94/)