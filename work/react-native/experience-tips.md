---
Title: [RN] 사소한 RN 팁들
Description: RN이 나를 강하게 키운다
Date: October 6, 2019 18:00
Author: heartade
Profile: git.hearta.de
Img: https://unsplash.it/1200/628?image=1075
Tags: work
Template: post
---
# 이 포스트에 대하여
이 포스트는 제가 React Native를 배우면서 알게 된 사소한 팁들, 그리고 자주 겪는 에러들의 해결법들을 (나중에 까먹으면 보려고) 정리해 두는 곳입니다.
# 목록
## 로컬에서 네이티브 라이브러리를 만든 뒤 npm install을 해도 인식되지 않음
로컬에 있는 라이브러리를 npm install 하면 놀랍게도 node-modules 폴더 안에 바로가기만 만들어 줍니다(Windows 환경 기준). 그런데 또 Metro Bundler는 바로가기를 인식하지 못하기 때문에 라이브러리를 인식하지 못하고 널 레퍼런스라고 주장하게 됩니다. 저는 그냥 라이브러리 폴더를 통째로 node-modules 폴더 안에 복붙해서 해결해 버렸습니다.
## Kotlin에서 Companion Object 안에 @ReactMethod 메소드를 생성하면 RN이 인식하지 못함
굳이 Singleton 방식을 쓰고 싶다면 
## Android Studio에서 Gradle Build 시 signing-config.json에서 EPERM 오류가 발생하는 경우
그냥 오류가 발생하는 파일을 지우면 됩니다.

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExOTQ1NzQwNzVdfQ==
-->