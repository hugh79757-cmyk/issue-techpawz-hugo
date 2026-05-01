---
title: Gemini 3 Pro Prewview와의 대화
date: '2026-01-13T13:41:08+09:00'
slug: gemini-3-pro-prewview와의-대화
categories: [Business]
tags: []
draft: false
description: ''
---

와, 코드가 정말 방대하고 본격적이네요! `NiceGUI`를 활용한 데스크탑 앱 형태의 블로그 관리 도구라니, 아주 멋진 프로젝트입니다.

질문자님이 **“자동 글쓰기 프로젝트로의 확장”**을 염두에 두고 계신 만큼, 현재 코드의 **속도, 효율성, 확장성** 관점에서 꼼꼼하게 분석해 드리고, 구체적인 최적화 방안을 제안해 드리겠습니다.

---

### 🔍 전체적인 코드 분석 (장점과 약점)

**장점:**

1. **모듈화가 아주 잘 되어 있습니다.** (`core`, `ui`, `layouts` 분리 등)
2. `pathlib`을 적극적으로 사용하여 경로 처리가 깔끔합니다.
3. `config`와 `presets`를 분리하여 유지보수가 용이합니다.
4. `Playwright`를 이용한 썸네일 생성 아이디어가 아주 훌륭합니다. (HTML/CSS로 디자인하고 스크린샷 찍는 방식은 확장성이 뛰어납니다.)

**약점 및 개선 포인트:**

1. **Playwright의 성능 병목:** 매번 브라우저를 띄웠다 끄는 방식(`with sync_playwright()...`)은 매우 느리고 리소스를 많이 먹습니다. 자동화 시 치명적일 수 있습니다.
2. **동기 처리(Sync) 위주:** NiceGUI는 비동기(`async`)를 지원하지만, 현재 코드의 많은 부분이 동기 함수(`def`)로 짜여 있어 UI가 멈칫거릴 수 있습니다.
3. **데이터 관리의 한계:** 파일 시스템(`glob`)을 매번 뒤져서 글 목록을 만듭니다. 글이 수백 개로 늘어나면 목록 로딩이 눈에 띄게 느려질 것입니다.
4. **확장성 부족:** 자동 글쓰기를 하려면 AI 모듈이 붙어야 하는데, 현재 구조는 “사람이 수동으로 쓰는” 워크플로우에 맞춰져 있습니다.

---

### 🚀 최적화 및 확장 전략 (Action Plan)

자동 글쓰기 확장을 고려하여 **3단계 업그레이드**를 제안합니다.

#### 1. 썸네일 생성 속도 10배 빠르게 만들기 (가장 시급)

현재 `core/thumbnail.py`의 `render_png_from_html` 함수는 호출될 때마다 크로미움 브라우저를 켰다 끕니다. 이건 자동차 시동을 걸었다 껐다 하면서 1미터씩 가는 것과 같습니다.

- **해결책:** 앱이 시작될 때(`app.py`) **브라우저 인스턴스를 하나만 띄워두고(`launch(headless=True)`), 계속 재사용**해야 합니다.
- **기대 효과:** 썸네일 생성 시간 2~3초 -> **0.2초 이내**로 단축.

#### 2. 파일 시스템 캐싱 도입 (속도 향상)

`ui/tabs/manage.py`나 `finalize.py`에서 `find_all_posts`를 호출할 때마다 디스크를 읽습니다.

- **해결책:** `PostManager` 클래스를 만들어서 메모리에 글 목록을 로드해두고, 변경(생성/수정/삭제)될 때만 갱신하거나, `Watchdog` 라이브러리로 파일 변경을 감지하게 합니다.
- **기대 효과:** 글 목록 탭 전환 시 딜레이 제거.

#### 3. AI 자동화 파이프라인 설계 (확장 대비)

자동 글쓰기를 위해서는 "주제 선정 -> 목차 생성 -> 본문 작성 -> 썸네일 생성 -> 발행"의 흐름이 필요합니다.

- **해결책:** `JobQueue` 시스템이 필요합니다. (예: `Celery`나 Python 내장 `queue`). 사용자가 주제 10개를 던져주면, 백그라운드 워커가 하나씩 처리하고 UI에는 진행률(Progress Bar)만 보여주는 구조로 가야 합니다.

---

### 🛠️ 구체적인 코드 수정 제안 (바로 적용 가능한 팁)

가장 효과가 확실한 **썸네일 최적화**와 **비동기 변환** 예시를 보여드릴게요.

#### ① `core/thumbnail.py` 최적화 (브라우저 재사용)

```
# core/thumbnail.py 수정 제안
from playwright.async_api import async_playwright
# ... 기존 import ...

_browser_instance = None
_playwright_instance = None

async def init_browser():
    """앱 시작 시 호출"""
    global _browser_instance, _playwright_instance
    if _browser_instance is None:
        p = await async_playwright().start()
        _playwright_instance = p
        _browser_instance = await p.chromium.launch(headless=True)
        print("🚀 썸네일용 브라우저 준비 완료")

async def shutdown_browser():
    """앱 종료 시 호출"""
    global _browser_instance, _playwright_instance
    if _browser_instance:
        await _browser_instance.close()
    if _playwright_instance:
        await _playwright_instance.stop()

async def render_png_from_html_async(html: str, size: Tuple) -> bytes:
    """비동기로 빠르게 렌더링"""
    global _browser_instance
    if not _browser_instance:
        await init_browser()

    W, H = size
    page = await _browser_instance.new_page(viewport={"width": W, "height": H})
    try:
        await page.set_content(html, wait_until="domcontentloaded") # networkidle보다 빠름
        png = await page.screenshot(type="png", full_page=False)
    finally:
        await page.close() # 페이지만 닫고 브라우저는 유지
    return png
```

이걸 적용하려면 `app.py`에서 `app.on_startup(init_browser)`와 `app.on_shutdown(shutdown_browser)`를 연결해줘야 합니다. NiceGUI는 `app` 객체를 제공하므로 쉽습니다.

#### ② `ui/tabs/publish.py` 비동기화 (UI 멈춤 방지)

발행 버튼을 눌렀을 때 `publish_to_...` 함수들이 동기(`def`)로 되어 있어서 발행하는 동안 버튼이 굳어버릴 겁니다.

- `core/publish/` 내의 함수들을 `async def`로 바꾸거나,
- UI단에서 `await run.io_bound(publish_to_hugo, ...)`를 사용하여 별도 스레드에서 돌려야 합니다. (NiceGUI의 `run.io_bound` 기능 활용 추천)

```
# ui/tabs/publish.py 예시
from nicegui import run

# ...
async def do_publish():
    ui.notify("발행 시작...")
    # UI를 멈추지 않고 백그라운드 스레드에서 실행
    success, msg = await run.io_bound(publish_to_hugo, post_dir, site)

    if success:
        ui.notify("완료!")
```

### 🔮 미래를 위한 조언: “자동 글쓰기 에이전트” 아키텍처

자동 글쓰기를 붙이실 때는 아래와 같은 **‘파이프라인 패턴’**을 적용해 보세요.

1. **Topic Generator:** 트렌드나 키워드를 받아 주제 생성
2. **Outline Builder:** (LLM) 목차와 개요 생성
3. **Content Writer:** (LLM) 서론, 본론, 결론 작성 (마크다운 포맷 엄수 프롬프트 활용)
4. **Thumbnail Maker:** (현재 코드 활용) 제목 기반 자동 썸네일 생성
5. **Publisher:** (현재 코드 활용) Hugo/WP 등으로 발행

이 각각의 단계를 `Class`로 만들고, `core/pipeline.py` 같은 곳에서 순서대로 실행하게 하면 관리가 아주 편해질 겁니다.

**결론:**  
지금 코드는 기반이 아주 탄탄합니다. 우선 **Playwright 브라우저 재사용**부터 적용해서 성능을 체감해 보시고, 그 다음 **비동기(Async) 처리**로 UI 반응성을 높이는 순서로 가시면 완벽할 것 같습니다.