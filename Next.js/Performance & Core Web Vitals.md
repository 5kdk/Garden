- [웹 퍼포먼스와 코어 웹 바이탈](#웹-퍼포먼스와-코어-웹-바이탈)
  - [Overview](#overview)
  - [Largest Contentful Paint (LCP)](#largest-contentful-paint-lcp)
  - [First Input Delay (FID)](#first-input-delay-fid)
  - [Cumulative Layout Shift (CLS)](#cumulative-layout-shift-cls)
  - [SEO Impact](#seo-impact)
    - [Lighthouse (v6) 바이탈 가중치](#lighthouse-v6-바이탈-가중치)
    - [UX Impact](#ux-impact)

<br />

# 웹 퍼포먼스와 코어 웹 바이탈

웹 바이탈(Web Vitals)은 웹에서 최종 사용자 페이지 경험을 측정하기 위한 통합 지침과 지표를 제공하기 위해 Google에서 만든 이니셔티브입니다.

코어 웹 바이탈은 웹 바이탈의 하위 집합으로 현재 로딩, 상호 작용 및 시각적 안정성을 측정하는 세 가지 지표로 구성되어 있습니다. 이러한 지표는 LCP(가장 많은 콘텐츠가 포함된 페인트), FID(첫 번째 입력 지연), CLS(누적 레이아웃 시프트)입니다.

이 세 가지 지표에서 높은 점수를 받으면 사용자에게 더 원활한 웹사이트 경험을 제공할 수 있습니다.

각 코어 웹 바이탈 지표에서 낮은 점수를 받은 웹사이트는 Google이 코어 웹 바이탈을 검색 알고리즘의 순위 요소로 사용하기 시작하면서 검색 엔진 순위에 영향을 미칩니다. 낮은 바이탈은 웹 트래픽과 비즈니스에 영향을 미칠 수 있습니다.

이 레슨에서는 이에 대해 알아봅니다:

- 코어 웹 바이탈에 대한 간략한 배경
- SEO 및 UX에서 코어 웹 바이탈이 갖는 의미와 웹사이트에 미치는 영향.
- 개발 프로세스에서 코어 웹 바이탈에 관심을 가져야 하는 이유와 이를 측정하는 방법.
- Next.js로 코어 웹 바이탈을 개선하고 변경 사항을 모니터링하는 방법.

<br />

## Overview

이 레슨에서는 다양한 지표, 코어 웹 바이탈이 SEO에 미칠 수 있는 영향, 사용자 경험에 미치는 중요성에 대해 살펴봅니다.

코어 웹 바이탈을 측정할 때는 세 가지 값이 있습니다: **"양호"**, **"개선 필요"**, **"미흡"** 입니다. 이러한 값은 측정 대상 바이탈에 따라 다릅니다:

![vitals-light](./assets/Performance%20&%20Core%20Web%20Vitals/vitals-light.png)

코어 웹 바이탈은 두 가지 방법으로 접근할 수 있습니다:

1. **각 지표에서 가능한 한 가장 높은 점수를 받으려고 노력하세요.** 완벽을 추구하는 것은 좋지만 종속성이 많은 대규모 웹사이트에서는 까다로울 수 있습니다.
2. **동종 업계의 경쟁업체를 벤치마킹하세요.** Google 검색에서 완벽하게 최적화된 모든 웹사이트와 경쟁하는 것이 아니라 타겟 키워드에 대해 순위를 매기는 다른 웹사이트와 경쟁하는 것입니다.

<br />

## Largest Contentful Paint (LCP)

![lcp](./assets/Performance%20&%20Core%20Web%20Vitals/lcp.avif)

가장 큰 콘텐츠가 많은 페인트(LCP) 메트릭은 페이지의 로딩 성능을 살펴봅니다. LCP는 페이지에서 가장 큰 요소를 뷰포트에 표시하는 데 걸리는 시간을 측정합니다. 이 요소는 페이지의 주요 공간을 차지하는 큰 텍스트 블록, 비디오 또는 이미지일 수 있습니다.

> **Note:** 이는 페이지 로딩이 시작될 때부터 첫 번째 요소가 화면에 렌더링될 때까지의 시간을 측정하는 FCP(First Contentful Paint)가 아닙니다.

DOM이 렌더링되면 페이지에서 가장 큰 요소가 변경될 수 있습니다. 가장 큰 콘텐츠가 많은 페인트는 가장 큰 이미지나 요소가 화면에 표시될 때까지 계산을 멈추지 않습니다.

![lcp-example](./assets/Performance%20&%20Core%20Web%20Vitals/lcp-example.avif)

> **추가 읽을거리**
>
> - Google: [Largest Contentful Paint Documentation](https://web.dev/lcp/)
> - Vercel: [Blog: Core Web Vitals - Largest Contentful Paint](https://vercel.com/blog/core-web-vitals#largest-contentful-paint)

<br />

## First Input Delay (FID)

FID(첫 번째 입력 지연) 지표는 웹 페이지와 상호 작용하는 동안 최종 사용자가 느끼는 체감 속도입니다. 입력 상자를 클릭해도 아무 일도 일어나지 않는다고 상상해 보세요. 사이트의 상호 작용 및 응답성에 대한 이러한 불만은 입력 지연이 길기 때문에 발생합니다.

![fid](./assets/Performance%20&%20Core%20Web%20Vitals/fid.png)

FID는 실제 사용자 데이터가 필요하며 실험실에서는 측정할 수 없습니다(예: Google Lighthouse). 그러나 총 차단 시간(TBT) 지표는 실험실에서 측정할 수 있으며 상호 작용에 영향을 미치는 문제를 파악할 수 있습니다.

![fid-example](./assets/Performance%20&%20Core%20Web%20Vitals/fid-example.png)

> **추가 읽을거리**
>
> - Google: [First Input Delay Documentation](https://web.dev/fid/)
> - Vercel: [Blog: Core Web Vitals - First Input Delay](https://vercel.com/blog/core-web-vitals#first-input-delay)

<br />

## Cumulative Layout Shift (CLS)

![cls](./assets/Performance%20&%20Core%20Web%20Vitals/cls.png)

누적 레이아웃 이동(CLS) 지표는 사이트의 전반적인 레이아웃 안정성을 측정하는 지표입니다. 페이지가 로드될 때 예기치 않게 레이아웃이 바뀌는 사이트는 사용자 실수나 주의 산만으로 이어질 수 있습니다.

누적 레이아웃 시프트(CLS)는 처음에 DOM에 의해 렌더링된 후 요소가 시프트된 경우에 발생합니다. 여기서는 텍스트 블록 다음에 버튼이 화면에 렌더링되어 블록이 아래쪽으로 이동했습니다. CLS를 계산할 때는 충격과 거리의 조합이 고려됩니다.

![cls-example](./assets/Performance%20&%20Core%20Web%20Vitals/cls-example.png)

각 요소의 개별 레이아웃 이동 점수는 예기치 않은 이동이 발생한 경우에만 CLS에 계산됩니다. 새 요소가 DOM에 추가되거나 기존 요소의 크기가 변경되더라도 로드된 요소가 그 위치를 유지하면 레이아웃 이동 점수에 포함되지 않습니다.

> **추가 읽을거리**
>
> - Google: [Cumulative Layout Shift Documentation](https://web.dev/cls/)
> - Vercel: [Blog: Core Web Vitals - Cumulative Layout Shift](https://vercel.com/blog/core-web-vitals#cumulative-layout-shift)

<br />

## SEO Impact

Google 검색 엔진의 주요 목표는 현지화 및 맞춤법 오류를 고려하면서 사용자에게 가능한 최상의 결과를 제공하는 것입니다. 모든 경우에 Google은 사용자에게 빠르고 원활한 웹사이트 경험을 제공하는 것을 중요하게 생각합니다.

모바일 기기에서의 웹 페이지 속도는 2018년부터 순위 결정 요소에 포함되었습니다. 하지만 지금까지 Google 검색 알고리즘이 순위를 매길 때 어떤 구체적인 성능 지표를 사용하는지는 명확하지 않았습니다.

하지만 2021년 6월에 Google이 성능을 분석하고 최적화할 수 있는 구체적인 지표와 범위를 제공하면서 상황이 달라졌습니다.

![page-experience](./assets/Performance%20&%20Core%20Web%20Vitals/page-experience.png)

### Lighthouse (v6) 바이탈 가중치

세 가지 지표의 가치가 반드시 동일하지는 않습니다. 라이트하우스에서는 각 코어 웹 바이탈에 다른 가중치가 할당됩니다:

- **가장 큰 콘텐츠가 많은 페인트**: 25%
- **총 차단 시간\***: 25%
- **첫 번째 콘텐츠가 많은 페인트**: 15%
- **속도 지수**: 15%
- **인터랙티브 시간**: 15%
- **누적 레이아웃 시프트**: 5%

\*첫 입력 지연과 유사합니다.

> **Note**: Google 순위 영향은 개별 코어 웹 바이탈 점수에 관계없이 모든 코어 웹 바이탈의 양호 범위에 있는 모든 페이지에 동일하게 적용됩니다.

### UX Impact

코어 웹 바이탈에 대한 대부분의 대화는 주로 SEO에 초점을 맞추고 있습니다.

코어 웹 바이탈이 페이지 경험과 검색 순위의 개선을 측정하고 추진하기 위해 고안된 이니셔티브인 것은 사실이지만, 궁극적으로 이러한 변화의 혜택을 받는 것은 사용자들입니다.

코어 웹 바이탈은 최상의 페이지 경험을 위해 노력하는 데 도움이 됩니다. 2012년에 Amazon에서 실시한 연구에 따르면 로딩 시간이 1초만 더 길어져도 16억 달러의 비용이 발생할 수 있다고 합니다. 이와 같은 연구는 우수한 페이지 경험과 빠른 웹사이트의 중요성을 보여주며, 코어 웹 바이탈은 이 두 가지를 모두 달성하는 데 도움을 줍니다.

> **추가 읽을거리**  
> Chromium: [The Science Behind Web Vitals](https://nextjs.org/docs/pages/building-your-application/routing)
