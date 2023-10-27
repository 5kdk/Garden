- [코어 웹 바이탈 모니터링](#코어-웹-바이탈-모니터링)
  - [Next.js Speed Insights](#nextjs-speed-insights)
  - [커스텀 레포팅](#커스텀-레포팅)
  - [Data Studio (Chrome User Experience Report)](#data-studio-chrome-user-experience-report)
  - [기타 도구들](#기타-도구들)

<br />

# 코어 웹 바이탈 모니터링

사이트를 최적화한 후에는 계속 반복할 수 있도록 운영 중에 모니터링하는 것이 중요합니다. 코어 웹 바이탈을 모니터링할 때는 일회성 실험실 테스트에 의존하기보다는 장기간에 걸쳐 추적하는 것이 중요합니다.

이 단원에서는 다음에 대해 자세히 알아봅니다:

- Next.js 속도 인사이트
- Next.js 사용자 지정 보고
- CrUX 보고서
- 기타 측정 도구

<br />

## Next.js Speed Insights

[Next.js Speed Insights](https://nextjs.org/analytics) 를 사용하면 코어 웹 바이탈을 사용하여 페이지의 성능을 분석하고 측정할 수 있습니다.

![analytics](./assets/Monitoring%20yout%20Core%20Web%20Vitals/analytics.png)

[Vercel 배포](<(https://vercel.com/docs/concepts/speed-insights?utm_campaign=no-campaign#metrics?utm_source=next-site&utm_medium=learnpages&utm_campaign=no-campaign)>)에서 아무것도 구성하지 않은 상태에서 [실제 경험 점수](https://vercel.com/docs/concepts/speed-insights?utm_campaign=no-campaign#metrics?utm_source=next-site&utm_medium=learnpages&utm_campaign=no-campaign)를 수집할 수 있습니다.

<br />

## 커스텀 레포팅

또한 Next.js 속도 인사이트에서 사용하는 기본 제공 릴레이를 사용하여 데이터를 자체 서비스로 전송하거나 Google 애널리틱스로 푸시할 수도 있습니다.

이제 이를 어떻게 추가할 수 있는지 살펴보겠습니다. 최적화 중인 데모 앱에 추가하면 됩니다.

콘솔 로그를 사용하여 실시간으로 메트릭을 살펴보겠습니다.

이를 위해 Next.js에서 제공하는 `reportWebVitals` 함수를 활용할 수 있습니다.

> **Note**: 이전 레슨을 방금 완료한 경우에는 이 과정이 필요하지 않습니다.

```zsh
npx create-next-app@latest nextjs-lighthouse --use-npm --example "https://github.com/vercel/next-learn/tree/main/seo"
```

`pages/_app.js`를 열고 다음 함수를 내보냅니다:

```jsx
export function reportWebVitals(metric) {
  console.log(metric);
}
```

그런 다음 애플리케이션을 빌드하고 시작하세요:

```zsh
npm run build && npm run start
```

Chrome을 열면 개발자 도구 콘솔에 메트릭이 표시됩니다. 또한 페이지와 상호 작용할 때만 **FID**가 트리거된다는 것을 알 수 있습니다.

> **추가 읽을거리**  
> Next.js: [Measuring Performance](https://nextjs.org/docs/pages/building-your-application/optimizing/analytics)

<br />

## Data Studio (Chrome User Experience Report)

성능을 측정하는 또 다른 훌륭한 무료 오픈소스 방법은 [Chrome 사용자 경험 보고서](https://developers.google.com/web/tools/chrome-user-experience-report)데이터세트를 사용하는 것입니다.

Chrome 사용자 경험 보고서는 실제 Chrome 사용자가 웹에서 인기 있는 목적지를 경험하는 방식에 대한 사용자 경험 지표를 제공합니다.

이 데이터 세트는 [BigQuery](https://console.cloud.google.com/bigquery?project=chrome-ux-report&pli=1)에서 공개적으로 사용할 수 있으며, [Google 데이터 스튜디오](https://lookerstudio.google.com/overview?)에서 완전히 무료로 사용할 수도 있습니다.

다행히도 웹사이트의 성능을 추적하기 위한 템플릿으로 사용할 수 있는 [오픈소스 대시보드](https://lookerstudio.google.com/u/0/datasources/create?connectorId=AKfycbxk7u2UtsqzgaA7I0bvkaJbBPannEx0_zmeCsGh9bBZy7wFMLrQ8x24WxpBzk_ln2i7)가 있습니다.

이 데이터 세트의 유일한 단점은 웹 사이트가 CrUX 보고서에 포함되려면 의미 있는 방문 횟수가 있어야 하며, 그렇지 않으면 데이터 부족으로 인해 보고서에 포함되지 않는다는 것입니다. 따라서 작업 중이거나 최근에 만든 웹사이트에는 적합하지 않을 수 있습니다.

또한 데이터는 월 단위로 업데이트됩니다. 보통 한 달이 끝나고 약 15일 후에 데이터를 확인할 수 있으므로 코어 웹 바이탈 점수를 개선하려는 경우 가장 실용적이지 않을 수 있습니다.

![data-studio](./assets/Monitoring%20yout%20Core%20Web%20Vitals/data-studio.png)

> **추가 읽을거리**  
> Google: [Example Dashboard (copy for free)](https://web.dev/chrome-ux-report-data-studio-dashboard/)

<br />

## 기타 도구들

기타 도구
다음 도구에서 현장 데이터 수집을 찾을 수 있습니다:

- [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/): Google의 페이지 속도 측정 도구.
- [Chrome User Experience Report](https://developers.google.com/web/tools/chrome-user-experience-report): 필드 데이터 오픈소스 데이터 세트.
- [Search Console](https://support.google.com/webmasters/answer/9205520): Google 검색, 코어 웹 바이탈 보고서.

실험실 데이터를 찾고 있다면 Google에서 여러 측정 도구도 제공합니다:

- [Lighthouse](https://developers.google.com/web/tools/lighthouse?hl%253Den): 웹 페이지의 품질을 개선하기 위한 Google의 오픈소스 자동 도구.
- [Chrome 개발자 도구](https://developer.chrome.com/docs/devtools/): Google 크롬 브라우저에 직접 내장된 웹 개발자 도구 세트.

두 도구 모두 FID는 필드 데이터를 통해서만 측정할 수 있으므로 첫 번째 입력 지연(FID)의 대안으로 [총 차단 시간(TBT)](https://web.dev/articles/tbt)을 사용해야 합니다.
