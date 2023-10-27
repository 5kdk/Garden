- [코어 웹 바이탈 개선](#코어-웹-바이탈-개선)
  - [Lighthouse 소개](#lighthouse-소개)
    - [최적화된 예시](#최적화된-예시)
    - [최적화되지 않은 예](#최적화되지-않은-예)
      - [프로젝트 셋업](#프로젝트-셋업)
      - [다운로드 및 빌드, 스타트](#다운로드-및-빌드-스타트)
  - [이미지 구성 요소 및 자동 이미지 최적화](#이미지-구성-요소-및-자동-이미지-최적화)
    - [최적화되지 않은 이미지](#최적화되지-않은-이미지)
    - [이미지 컴포넌트](#이미지-컴포넌트)
    - [자동 이미지 최적화는 어떻게 작동하나요?](#자동-이미지-최적화는-어떻게-작동하나요)
      - [온디멘드 최적화(On-demand Optimization)](#온디멘드-최적화on-demand-optimization)
      - [지연 로드된 이미지(Lazy Loaded Images)](#지연-로드된-이미지lazy-loaded-images)
      - [CLS 방지(Avoids CLS)](#cls-방지avoids-cls)
    - [이미지 컴포넌트 사용](#이미지-컴포넌트-사용)
  - [다이나믹 임포트](#다이나믹-임포트)
  - [컴포넌트 다이나믹 임포트](#컴포넌트-다이나믹-임포트)
  - [폰트 최적화](#폰트-최적화)
  - [서드파티 스크립트 최적화](#서드파티-스크립트-최적화)
    - [스크립트 컴포넌트 사용](#스크립트-컴포넌트-사용)

<br />

# 코어 웹 바이탈 개선

Next.js 기능을 사용하여 예제의 코어 웹 바이탈을 개선하는 방법을 살펴보겠습니다.

이 단원에서는 배우게 됩니다:

- 라이트하우스란 무엇이며 어떻게 사용하는지 알아보세요.
- 다음/이미지를 사용하여 이미지를 자동으로 최적화하는 방법.
- 라이브러리와 컴포넌트를 동적으로 임포트하여 초기 JS 번들을 줄이는 방법.
- 타사 스크립트에 사전 연결하는 방법.
- Next.js가 기본적으로 웹 글꼴 로딩을 최적화하는 방법.
- 타사 스크립트의 로딩을 최적화하는 방법.

<br />

## Lighthouse 소개

이전 섹션에서 살펴본 것처럼 코어 웹 바이탈은 로딩 성능(최대 콘텐츠가 많은 페인트, LCP), 상호 작용(첫 번째 입력 지연, FID), 시각적 안정성(누적 레이아웃 이동, CLS)을 통해 사용자 경험의 측면에 중점을 둡니다.

이 단원에서는 라이트하우스라는 시뮬레이션 환경을 사용하여 코어 웹 바이탈을 측정하는 방법을 중점적으로 살펴보겠습니다.

> **Note**: 이 레슨에서는 Chrome 개발 도구를 사용합니다. 하지만 Lighthouse를 실행하는 방법에는 여러 가지가 있습니다.

Lighthouse는 제공된 URL에 대해 일련의 감사를 실행하는 방식으로 작동합니다. 이러한 감사를 기반으로 페이지가 얼마나 잘 수행되었는지에 대한 보고서를 생성합니다. 개선이 필요한 영역이 있는 경우 보고서에서 개선 방법에 대한 인사이트를 제공합니다.

<br />

### 최적화된 예시

라이트하우스의 작동 방식에 대한 예시를 보려면 당사 홈페이지(https://nextjs.org)를 이용하세요.

1. Chrome을 엽니다.
2. 시크릿 창에서 https://nextjs.org 으로 이동합니다.
3. 개발자 도구를 열고 **Lighthouse** 탭을 클릭합니다.
4. **Generate Report**을 클릭합니다.
5. 이제 60초 이내에 보고서가 실행됩니다.

이제 60초 이내에 보고서가 실행됩니다.

> **Note**: 타사 플러그인이 보고서에 영향을 미칠 수 있으므로 시크릿 창에서 보고서를 실행하는 것이 중요합니다.

또한 광고 차단기는 스크립트 로딩을 차단하여 불완전한 감사를 제공할 수 있습니다. 클린 퍼소나를 설정하여 성과를 측정하는 것을 고려하세요.

다음은 보고서 예시입니다:

![lighthouse](./assets/Improving%20your%20Core%20Web%20Vitals/lighthouse.png)

<br />

### 최적화되지 않은 예

이 튜토리얼에서는 최적화하지 않고 애플리케이션을 만들었습니다.

#### 프로젝트 셋업

이 애플리케이션은 방문자가 국가를 검색하여 인구를 검색하고 버튼을 클릭하여 팝업 모달을 읽는 두 가지 작업을 수행할 수 있는 최적화되지 않은 기본 애플리케이션입니다. 이 애플리케이션은 타사 라이브러리 사용을 피할 수 없는 대규모 애플리케이션 작업의 현실을 모방하기 위한 것입니다.

#### 다운로드 및 빌드, 스타트

```zsh
npx create-next-app@latest nextjs-lighthouse --use-npm --example "https://github.com/vercel/next-learn/tree/main/seo"
```

```zsh
cd nextjs-lighthouse
```

```zsh
npm run build && npm run start
```

> **추가 읽을거리**  
> Chromium: [Lighthouse Scoring Calculator](https://googlechrome.github.io/lighthouse/scorecalc/)

<br />

## 이미지 구성 요소 및 자동 이미지 최적화

### 최적화되지 않은 이미지

일반 HTML을 사용하여 Hero 이미지를 다음과 같이 추가했습니다:

```html
<img src="large-image.jpg" alt="Large Image" />
```

그러나 이는 다음과 같은 몇 가지 사항을 수동으로 최적화해야 함을 의미합니다:

- 다양한 화면 크기에서 이미지가 반응하는지 확인.
- 타사 도구 또는 라이브러리를 사용하여 이미지 최적화.
- 뷰포트에 이미지가 들어올 때 Lazy-loading.

<br />

### 이미지 컴포넌트

Next는 이러한 최적화를 즉시 처리할 수 있는 [이미지 컴포넌트](https://nextjs.org/docs/pages/api-reference/components/image)를 제공합니다.

`next/image`는 최신 웹에 맞게 진화한 HTML 이미지 요소의 확장입니다.

즉, `next/image`를 사용하면 WebP와 같은 최신 형식(브라우저에서 지원하는 경우)의 이미지 크기 조정, 최적화 및 제공을 자동으로 수행할 수 있습니다.

이 컴포넌트는 뷰포트가 작은 기기에 큰 이미지를 전송하는 것을 방지하고 Next.js가 향후 이미지 형식을 채택하여 이를 지원하는 브라우저에 해당 이미지를 제공할 수 있도록 합니다.

자동 이미지 최적화는 모든 이미지 소스에서 작동합니다. 이미지가 CMS와 같은 외부 데이터 소스에서 호스팅되는 경우에도 이미지를 최적화할 수 있습니다.

<br />

### 자동 이미지 최적화는 어떻게 작동하나요?

#### 온디멘드 최적화(On-demand Optimization)

Next.js는 빌드 시 이미지를 최적화하는 대신 사용자가 요청할 때 온디맨드 방식으로 이미지를 최적화합니다. 정적 사이트 생성기 및 정적 전용 솔루션과 달리 10개의 이미지를 제공하든 천만 개의 이미지를 제공하든 빌드 시간이 늘어나지 않습니다.

#### 지연 로드된 이미지(Lazy Loaded Images)

이미지는 기본적으로 지연 로드됩니다. 뷰포트 외부에 있는 이미지의 경우 페이지 속도에 불이익이 없습니다. 이미지는 뷰포트에 들어올 때만 로드됩니다.

#### CLS 방지(Avoids CLS)

누적 레이아웃 시프트(CLS)를 피하기 위해 이미지는 항상 렌더링됩니다.

<br />

### 이미지 컴포넌트 사용

`next/image`를 사용하여 Hero 이미지를 표시하도록 앱을 업데이트해 보겠습니다. 높이와 너비 소품은 원하는 렌더링 크기여야 하며, 가로 세로 비율은 소스 이미지와 동일해야 합니다.

`pages/index.js` 파일을 열고 파일 시작 부분에 다음/이미지에서 이미지 가져오기를 추가합니다:

```jsx
import Image from 'next/image';
```

그런 다음 Hero 이미지를 이미지 구성 요소로 바꿉니다:

```jsx
// Before
<img src="large-image.jpg" alt="Large Image" />

// After
<Image src="/large-image.jpg" alt="Large Image" width={3048} height={2024} />
```

Footer에도 이미지가 있습니다. 이것도 교체해 보겠습니다:

```jsx
// Before
<img src="/vercel.svg" alt="Vercel Logo" />

// After
<Image src="/vercel.svg" alt="Vercel Logo" width={72} height={16} />
```

마지막으로 Chrome 개발자 도구에서 다른 라이트하우스 보고서를 실행하고 결과를 비교하세요.

> **추가 읽을거리**  
> Next.js: [Automatic Image Optimization Documentation](https://nextjs.org/docs/basic-features/image-optimization)
> Next.js: [API Reference for next/image](https://nextjs.org/docs/api-reference/next/image)

<br />

## 다이나믹 임포트

이 단원에서는 초기 페이지 로드 시 타사 라이브러리에서 로드되는 자바스크립트의 양을 줄이는 방법을 알아봅니다.

Next.js는 자바스크립트에 대한 ES2020 [동적 `import()`](https://nextjs.org/docs/pages/building-your-application/optimizing/lazy-loading)를 지원합니다. 이를 통해 자바스크립트 모듈을 동적으로 가져와서 작업할 수 있습니다. 또한 서버 측 렌더링(SSR)과 함께 작동합니다.

동적 임포트를 코드를 관리 가능한 청크로 분할하는 또 다른 방법이라고 생각하면 됩니다.

pages/index.js 파일을 열고 파일의 맨 아래에서 동적으로 가져오기 때문에 파일 시작 부분에서 이 두 가져오기 파일을 제거합니다.

```jsx
import Fuse from 'fuse.js';
import _ from 'lodash';
```

지금은 제거하려고 합니다:

```jsx
const fuse = new Fuse(countries, {
  keys: ['name'],
  threshold: 0.3,
});
```

이제 이 코드를 제거했으므로 동적으로 가져온 라이브러리를 사용하도록 검색 `input`을 설정해 보겠습니다.

`input`을 다음 코드로 대체할 수 있습니다:

```jsx
<input
  type="text"
  placeholder="Country search..."
  className={styles.input}
  onChange={async e => {
    const { value } = e.currentTarget;
    // Dynamically load libraries
    const Fuse = (await import('fuse.js')).default;
    const _ = (await import('lodash')).default;

    const fuse = new Fuse(countries, {
      keys: ['name'],
      threshold: 0.3,
    });

    const searchResult = fuse.search(value).map(result => result.item);

    const updatedResults = searchResult.length ? searchResult : countries;
    setResults(updatedResults);

    // Fake analytics hit
    console.info({
      searchedAt: _.now(),
    });
  }}
/>
```

동적 가져오기를 사용하면 페이지 로드 시 '사용하지 않는 자바스크립트 제거' 문제가 해결됩니다. 또한 인터랙티브 타임 투 인터랙티브(TTI)가 개선되어 첫 입력 지연(FID)을 개선하는 데 도움이 됩니다.

Chrome 개발자 도구에서 또 다른 라이트하우스 보고서를 실행하여 진행 상황을 확인해 보겠습니다.

> **추가 읽을거리**  
> Next.js: [Dynamic Imports Documentation](https://nextjs.org/docs/advanced-features/dynamic-import)

<br />

## 컴포넌트 다이나믹 임포트

다음으로, 초기 페이지 로딩에 필요하지 않은 React 컴포넌트로 시선을 돌려보겠습니다.

React 컴포넌트는 동적 임포트를 사용해 가져올 수도 있지만, 여기서는 다른 React 컴포넌트처럼 작동하도록 하기 위해 next/dynamic과 함께 사용합니다.

이 방법을 사용해 `Hello World` 코드 샘플로 모달의 로딩을 지연시키겠습니다. 이렇게 하면 초기 페이지 로드에서 서드파티 라이브러리 두 개를 추가로 제거할 수 있습니다.

`pages/index.js` 파일을 열고 파일 시작 부분에 `next/dynamic`에서 `dynamic`를 추가합니다:

```jsx
import dynamic from 'next/dynamic';
```

이 줄도 제거해야 합니다:

```jsx
import CodeSampleModal from '../components/CodeSampleModal';
```

이제 파일 시작 부분에 다음을 추가하여 동적 컴포넌트로 임포트할 수 있습니다:

```jsx
const CodeSampleModal = dynamic(() => import('../components/CodeSampleModal'), {
  ssr: false,
});
```

`CodeSampleModal`이 반환하는 기본 컴포넌트는`../components/CodeSampleModal`입니다. 일반 React 컴포넌트처럼 작동하며, 평소와 같이 프로퍼티를 전달할 수 있습니다.

서버에서는 이 컴포넌트가 필요하지 않으므로 `ssr: false`를 사용해 비활성화했습니다.

다음으로, 사용자가 필요로 할 때까지 이 컴포넌트의 로딩을 지연시키고 싶습니다. 이를 위해 모달이 열려 있어야 하는지 확인하는 조건부로 컴포넌트를 감싸고, 열려 있으면 로드할 수 있습니다.

`CodeSampleModal` 컴포넌트를 다음과 같이 래핑합니다:

```jsx
{
  isModalOpen && (
    <CodeSampleModal
      isOpen={isModalOpen}
      closeModal={() => setIsModalOpen(false)}
    />
  );
}
```

이제 `isModalOpen`이 처음으로 `true`로 전환되면 필요한 JavaScript가 요청됩니다.

이러한 최적화를 통해 이제 바이탈이 더 건강하게 보일 것입니다. Chrome 개발자 도구에서 다른 라이트하우스 보고서를 실행하여 확인해 보겠습니다.

이제 두 가지 최적화 제안이 남았습니다:

- **HTTP2 사용**: 이 문제를 해결하기 위해 HTTP2를 지원하는 곳(예: Vercel)에 배포할 수 있습니다.
- **이미지 요소에는 명시적인 너비와 높이가 없습니다**: 이는 실제로 lighthouse의 [버그](https://github.com/GoogleChrome/lighthouse/issues/11631)이며 사이트 성능에는 영향을 미치지 않습니다.

> **추가 읽을거리**  
> Next.js: [Dynamic Imports Documentation](https://nextjs.org/docs/advanced-features/dynamic-import)

<br />

## 폰트 최적화

[데스크톱용 웹 페이지의 82%](https://almanac.httparchive.org/en/2020/fonts)가 웹 글꼴을 사용합니다. 사용자 정의 글꼴은 사이트의 브랜딩, 디자인, 브라우저/기기 간 일관성을 위해 중요합니다. 하지만 웹 글꼴을 사용한다고 해서 성능이 저하되어서는 안 됩니다.

Next.js에는 [자동 웹폰트 최적화 기능](https://nextjs.org/docs/pages/building-your-application/optimizing/fonts)이 내장되어 있습니다. 기본적으로 Next.js는 빌드 시 글꼴 CSS를 자동으로 인라인 처리하여 글꼴 선언을 가져오기 위한 추가 왕복 작업을 제거합니다. 그 결과 첫 번째 콘텐츠가 많은 페인트(FCP)와 가장 큰 콘텐츠가 많은 페인트(LCP)가 개선됩니다.

예를 들어 다음은 글꼴을 최적화한 Next.js의 전후 모습입니다.

최적화하기 전에 추가 네트워크 요청이 필요합니다:

```html
<link href="https://fonts.googleapis.com/css2?family=Inter" rel="stylesheet" />
```

최적화 후 Next.js가 글꼴 CSS를 인라인 처리합니다:

```html
<style data-href="https://fonts.googleapis.com/css2?family=Inter">
  @font-face{font-family:'Inter';font-style:normal.....
</style>
```

<br />

## 서드파티 스크립트 최적화

많은 애플리케이션은 분석, 광고, 고객 지원 위젯 등 다양한 유형의 기능을 포함하기 위해 타사 JavaScript를 사용합니다. 그러나 타사에서 작성한 코드를 삽입하면 페이지 콘텐츠가 너무 일찍 로드되면 렌더링이 지연되고 사용자 성능에 영향을 미칠 수 있습니다.

Next.js는 모든 타사 스크립트의 로딩을 최적화하는 기본 제공 스크립트 컴포넌트를 제공하며, 개발자가 스크립트를 가져와 실행할 시기를 결정할 수 있는 옵션을 제공합니다.

### 스크립트 컴포넌트 사용

일반 HTML을 사용하면 외부 스크립트를 `next/head`에 수동으로 추가해야 합니다:

```jsx
import Head from 'next/head';

function IndexPage() {
  return (
    <div>
      <Head>
        <script src="https://www.googletagmanager.com/gtag/js?id=123" />
      </Head>
    </div>
  );
}
```

Next.js 스크립트 컴포넌트를 사용하면 `next/head`를 사용하지 않고도 컴포넌트의 어느 곳에나 추가할 수 있습니다.

```jsx
import Script from 'next/script';

function IndexPage() {
  return (
    <div>
      <Script
        strategy="afterInteractive"
        src="https://www.googletagmanager.com/gtag/js?id=123"
      />
    </div>
  );
}
```

스크립트 컴포넌트에는 최적의 로딩을 위해 스크립트를 가져와 실행할 시기를 결정할 수 있는 전략 속성이 도입되었습니다. LCP(가장 콘텐츠가 많은 페인트)에 부정적인 영향을 주지 않으려면 대부분의 타사 스크립트는 페이지의 모든 콘텐츠 로딩이 완료된 후 페이지가 인터랙티브해진 직후(`strategy="afterInteractive"`) 또는 브라우저 유휴 시간 동안 느리게(`strategy="lazyOnload"`) 로드되도록 지연시켜야 합니다.

> **추가 읽을거리**  
> Next.js: [Script Component](https://nextjs.org/docs/pages/building-your-application/optimizing/scripts)  
> Next.js: [API Reference for next/script](https://nextjs.org/docs/pages/api-reference/components/script)
