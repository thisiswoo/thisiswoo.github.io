---
layout: post
title: "[마이그레이션] 내가 회사 프로젝트 마이그레이션을 vue2가 아닌 vue3로 선택한 이유"
subtitle: "내가 회사 프로젝트 마이그레이션을 vue2가 아닌 vue3로 선택한 이유"
category: development
tags: problem_solving 마이그레이션 vue3
image:
    path: /assets/img/development/problem_solving/2023-02-23/reason_why_i_chose_vue3_cover.png
---

<span style="font-size:30px;">\#**목차**</span>
* this unordered seed list will be replaced by the toc
{:toc}

# 내가 회사 프로젝트 마이그레이션 작업 중 vue2가 아닌 vue3를 채택한 이유
- 작년 12월 말쯤, 회사에서 기존 프로젝트를 앞으로 크게 리뉴얼 할 예정이라는 이야기를 들었다.
- 내가 왜 `vue3`를 택했는지 블로그에 포스팅해야겠다고 생각하고 있었다.
- 조만간.. 곧.. 포스팅.. 해야지... 하지만... 마이그레이션과 나의 부족한 코딩 실력...이 발목을 잡았다.
- 머리를 쥐어짜며 마이그레이션 작업을 하던 중 거의 마무리 단계에 이르러서 드디어! 블로그에 포스팅하게 되었다.

## 발단
- 위에서 얘기했듯이 작년 12월 말쯤, 회사에서 기존 프로젝트를 앞으로 크게 리뉴얼 할 예정이라는 이야기를 들었다.
- 회사의 2~3개의 프로젝트가 `vue2.x` 버전 대로 회사 내부 시스템을 `vue2.x`로 개발하기도 하였다.
- 그러던 중 기존 프로젝트를 `vue`로 마이그레이션 한다고 하여 `vue`에 대해 비교 분석하여 제출하라고 하였다.
- 그래서 나와 신입 한 분과 함께 `vue`에 대해 조사를 하였다.
- 기존에 `vue3`에 대해 개인적으로 [코지 코더님의 인강]{:target="_blank"}과 [책]{:target="_blank"}을 보면서 공부를 했고 [사이드 프로젝트]{:target="_blank"}를 해본 경험이 있어 낯설진 않았다.
- 하지만 `vue2`에 대해서는 유지 보수하는 정로로만 하는 실력이었다.

## 전개
- 그렇게 신입 사원분과 함께 vue2와 vue3의 차이점, 장/단점, 그리고 테마, `framework` 등을 비교하여 분석 후 ppt 자료에 정리하여 보고하였다.
- 간략한 정리 내용은 아래와 같다.

### vue3.2 성능 개선(비교 vue2.x)
- `Vue.js`의 창시자 `Evan You`의 [공식 블로그]{:target="_blank"}에서 그 내용을 알 수 있다.
> 1. More efficient ref implementation (~260% faster read / ~50% faster write)
> 2. ~40% faster dependency tracking
> 3. ~17% less memory usage

- 구글 번역기를 그대로 돌려보면 아래와 같이 성능 개선을 하였다고 한다.
1. 보다 효율적인 `ref` 구현(최대 `260%` 더 빠른 읽기/ 최대 `50%` 더 빠른 쓰기)
2. 최대 `40%` 더 빠른 종속성 추적
3. 메모리 사용량 최대 `17%` 감소

- 또한, 다른 `framework(react, angular 등)`들 중에서 성능 부분이 빠르다는 것을 볼 수 있다.

### vue2.xx vs vue3.2의 lifecycle hook 차이
- 나의 포스팅 [Vue.js 라이플사이클 이해하기]를 참고하면 좋을듯하다.
- 결국 `Option API`보다 `Composition API`의 사용으로 동일한 코드를 [리팩토링]{:target="_blank"} 한 결과 코드의 사용량이 많이 줄 수 있게 되었다.
- 또한, 동일한 논리적 문제와 관련된 코드가 어떻게 그룹화 되었는지 아래 그림을 통해 확인할 수 있다.

![](/assets/img/development/problem_solving/2023-02-23/options-vs-composition-api.png){:.centered width="80%"}
Options API를 Composition API로 리팩토링한 결과
{:.figcaption}

### vue2.xx vs vue3.2 npm 다운로드 수
- 내가 처음 `vue`를 공부했을 때(2022년 12월 말)쯤 [캡틴 판교]{:target="_blank"}  님의 [Can I use ... Vue 3?]{:target="_blank"} 영상을 보게 되었다.
- 캡틴 판교 님의 영상을 보기 전 이미 [코지 코더] 님의 `vue3` 인강을 결제해 놓은 상태라 어쩔 수 없이 vue3 버전을 공부하였지만 그래도 나는 vue3가 조금 더 쉽게 다가왔다.(개인적으로 설명도 좋으셨다고 생각한다.)
- 내가 인강을 들을 때만 해도 `vue3`의 다운로드 수는 그렇게 높지가 않았던 걸로 기억한다.

![](/assets/img/development/problem_solving/2023-02-23/npm-vue_before.png){:.centered width="80%"}
캡틴 판교 님의 vuejs 2.x vs 3.x 비교 영상 중 npm 다운로드 수
{:.figcaption}

![](/assets/img/development/problem_solving/2023-02-23/npm-vue.png){:.centered width="80%"}
(2023년 4월 3일 기준)
{:.figcaption}

- 2021년 9월 5일 기준 : 약 `17배`가량의 다운로드 수
- 2023년 4월 3일 기준 : 약 `3.8배`가량의 다운로드 수
- `vue3` 초창기 `3.x` 대 라이브러리가 지원하지 않는 것이 굉장히 많아 아무래도 `3.x` 대보단 안전하고 풍부한 라이브러리를 제공하는 `2.x` 대를 많은 회사와 사람들이 선호하는 것을 확인할 수 있다.
- 하지만 지금은 공식 문서의 주소나, `vue3`를 기준으로 한 `vue-router`, `vuex` 라이브러리 버전이 설치된다.

### 회사의 장기 프로젝트
- 내가 `vue3.x` 대를 선택한 이유는 회사의 장기적 계획도 한몫하였다.
- 지금 신규 프로젝트를 계획하고 진행함에 있어 대표님께서 앞으로의 계획을 말씀해 주셨는데, 그 계획 중 개발자 시선으로 보자면, 다량의 트래픽이 발생한다는 것이다.
- 물론, 해당 프로젝트가 잘 안될 수도 있다. 혹은 기획에서 망가질 수도 있다. 하지만, 나는 그래도 위 성능 개선에 초점을 두고 조금 더 나은, 조금 더 빠른 성능의 퍼포먼스를 적용하고 싶었다.

### 나의 욕심과 선례
- 또 하나의 이유는 나의 욕심이다.
- 내가 `vue3`로 하고 싶었다. 물론 나의 경력이(2023년 2월 기준 - 1년 3개월)이라는 짧은 경력에도 도전하고 싶었다.
- 왜냐하면 회사에는 기존에 2~3개의 프로젝트가 `2.x` 대의 `vue` 프로젝트이다.
- 나는 좋던 나쁘던 선례를 남기고 싶었다.
- 좋은 선례는 `2.x` 대 뿐만 아니라 `3.x`의 퍼포먼스와 코드 작성 방법 들...
- 나쁜 선례는 내가 작성하고 이끌어 나간 진행 방향의 잘못된 코드와 선례들을 통해 다른 직원들이 이렇게 짜면 안 되겠다는 그나마 희망적인 선례를 남기고 싶었다.

## 결말
- 결국 나의 욕심?으로 인해 `vue3` 프로젝트로 확정되어 마이그레이션 작업 중이다.
- 마이그레이션 작업하면서 많이 실패하고, 선배 개발자분들에게 많이 여쭈어보고, 많이 검색해 보고, 많이 공부한 거 같다. 물론 그래도 부족한 건 사실이다.!
- 그래도 내가 프로젝트를 주도적으로 이끌어 나갈 수 있는 기회는 사실 많이 없는 거 같다. 특히나 연차가 얼마 안 되는 나에게 이런 기회는 흔치 않는 거 같다.
- 이렇게 기회를 준 차장님께 고마운 마음이 든다.
- 물론 아직 프로젝트가 끝나지 않았고, 차장님도 망할 수 있다는 생각을 하셨는지 안 하셨는지 모르지만 그래도 날 믿고 맡겨주신 차장님께 감사하다.
- 이 프로젝트가 망하면 선배 개발자분들이 계시니까 보험이 있으니 맡겨주신 거 일 수도 있다...^^;;
- 망하지 않게 내가 잘 이끌어 나가야지...!!!


# References
- [Vue 3.2 Released!](https://blog.vuejs.org/posts/vue-3.2){:target="_blank"}
- [Can I use ... Vue 3?](https://youtu.be/Z0OG00YQeMg){:target="_blank"}
- [Vuejs 공식문서 - KR](https://v3-docs.vuejs-korea.org/){:target="_blank"}


<!-- Links -->
[코지코더님의 인강]: https://kossiecoder.thinkific.com/
[책]: https://product.kyobobook.co.kr/detail/S000001916802
[사이드 프로젝트]: https://thisiswoo.vercel.app/projects
[공식 블로그]: https://blog.vuejs.org/posts/vue-3.2
[Vue.js 라이플사이클 이해하기]: ../client/2022-12-22-vue-lifecycle.md
[리팩토링]: https://v3-docs.vuejs-korea.org/guide/extras/composition-api-faq.html#more-flexible-code-organization
[캡틴 판교]: https://joshua1988.github.io/
[Can I use ... Vue 3?]: https://youtu.be/Z0OG00YQeMg
[코지코더]: https://kossiecoder.thinkific.com/
