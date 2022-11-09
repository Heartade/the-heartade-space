# About

> 글쇠를 두드리다가, 파란 막대를 바라보다가, 빨간 글씨를 읽는 일을 합니다.

소프트웨어 엔지니어 박수한입니다.

!> 옛날에 Pico로 작성하던 블로그와 노션에 작성한 블로그를 합치는 작업 중입니다. 그 동안은 뭐가 안 보이거나 이상해 보일 가능성이 높습니다.

# 무슨 일을 하나요?

- 2019년부터 2020년까지 한국관광공사 창업경진대회 스타트업인 DearUs 소속으로 활동했습니다.
- 잠시 블록체인 스타트업 SCVSoft에서 프론트엔드 엔지니어로 일하기도 했습니다.
- 2020년부터 2022년까지 인터랙티브 미디어 제작사 Swink에서 프론트엔드 엔지니어로 근무했습니다.
- 현재 SCVSoft로 돌아와 풀스택(강제) 엔지니어로 일하고 있습니다.

## 주로 하는 일

정해진 스택은 없지만 그냥 시키는 일을 합니다!

- React(TypeScript)를 사용하여 PWA를 만듭니다.
    - 요즘은 회사에서 SSR을 하라길래 NextJS도 씁니다 (원래는 우리 서버보다 사용자들 스마트폰 사양이 더 좋아서 굳이 안 썼어요)
    - 원래는 emotion/styled랑 ariakit 위주로 썼는데 회사에서 Tailwind CSS랑 DaisyUI를 쓰라길래 쓰고 있습니다.
    - Data Visualization 같은 건 좀 했는데 [이지통계-M (ebsmath.co.kr)](https://ebsmath.co.kr/innovativelrms/web_lrms/content/resource/easyTong2/index.html)
- Cocos Creator(TypeScript)를 사용하여 웹 브라우저에서 구동되는 앱이나 게임을 만듭니다.
    - [2020정부혁신박람회 (innoexpo.kr)](https://2020.innoexpo.kr/) 혼돈
- Unity3D(C#)를 사용하여 모바일 앱이나 게임을 만듭니다.
    - [아이캔두 누리키즈｜공부습관을 만드는 내 아이 맞춤 Ai 학습 (kyowonedu.com)](https://www.kyowonedu.com/KEP/aicandonuri22.jsp) 고통

## 해본 적 있는 일 (사이드 프로젝트 정도로!!)

- Java로 Spring 서버 건드리기
    - Spring에서 POST 리퀘스트 페이로드로 들어온 이미지 데이터를 OpenCV로 처리하기
- 웹앱을 Cordova로 패키징하기
- Java로 Cordova용 Android 네이티브 코드 플러그인 작성하기
- Kotlin으로 Android 앱 작성하기
- React Native, Electron 같은 잡다한 크로스플랫폼 유사 웹앱 프레임워크 쓰기

## 포트폴리오 (WIP)

### cordova-plugin-android-mediastore ([⤴️npmjs](https://www.npmjs.com/package/cordova-plugin-android-mediastore))

안드로이드 11의 Scoped Storage에 대응하여 base64 인코딩된 PNG 이미지를 안드로이드 갤러리에 저장하는 Cordova 플러그인

### korean-gotong (⤴️[npmjs](https://www.npmjs.com/package/korean-gotong))

한글과 한국어를 아주 조금 덜 고통스럽게 만들어주는 JS 도구 모음

```jsx
import { getKoreanHanjaNumeral } from "korean-gotong/dist/hanjaNumberTools";
import {
  getTopicParticle,
  getLinkingParticle,
  을를,
} from "korean-gotong/dist/particleTools";

console.log(getTopicParticle("사과")); // '는'
console.log(을를("참외")); // '참외를'
console.log(getKoreanHanjaNumeral(12345)); // '만 이천삼백사십오'

let a = 24;
let b = 36;
console.log(`${a}${getLinkingParticle(getKoreanHanjaNumeral(a))} ${b}`); // '24와 36'
```

### **2020 정부혁신박람회** (⤴️[전자신문](https://m.etnews.com/20201215000122), [⤴️링크](https://2020.innoexpo.kr))

비대면 인터랙티브 박람회 공간

### **EBSMATH 이지통계** (링크 준비중)

학습용 통계 계산 및 데이터 시각화 어플리케이션 

### **EBSMATH 중등 수학 인터랙티브 《별들에게 물어봐》 ([⤴️EBSMATH](https://www.ebsmath.co.kr/resource/rscView?cate=10095&cate2=10107&cate3=10133&rscTpDscd=RTP01&grdCd=MGRD02&sno=29728&type=S&historyYn=study))**

중고등학생을 위한 지수법칙 학습 게임