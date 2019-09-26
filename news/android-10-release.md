/*
Title: 구글 안드로이드 10, 무엇이 바뀌었나
Author: heartade
Date: S2019 16:11
Template: post
Tags: news
Description: (내가 보려고 정리하는) 주요 기능
Img: https://cdn.pixabay.com/photo/2016/09/25/15/11/android-1693894_1280.jpg
*/
오늘 새벽에 안드로이드 10이 릴리즈되었습니다. 디저트 이름을 떼고 ~~macOS와 Windows와 마찬가지로~~ 10이라는 숫자만 달고 출시되었는데, 이름이 바뀐 만큼 이런저런 변경점도 많습니다. 대강 한 번 살펴보도록 하겠습니다. 작성에 [Wired](https://www.wired.com/story/android-10-best-new-features/) 기사와 [Android Developers](https://developer.android.com/about/versions/10/behavior-changes-10)를 참고하였습니다.
##굵직한 변경점
###어두운 모드
<img src="https://4.bp.blogspot.com/-jRIbLZhjBkc/XW6TwYA6oAI/AAAAAAAAL4I/tOWQzT35Wm8npTAJRh0j0tchIC4pRjEygCLcBGAs/s1600/Screen%2BShot%2B2019-09-03%2Bat%2B9.08.06%2BAM.png" alt="Dark Mode" width="100%"/>


*Image Source: [Android Developers Blog](https://android-developers.googleblog.com/2019/09/welcoming-android-10.html)*

안드로이드 9에서 처음 도입된 시스템 다크 모드는 이제 더 많은 앱에서 적용됩니다. 서드 파티 앱에서는 어차피 안 되지만, 원래는 시스템 앱도 제대로 안 되었던 것에 비하면 장족의 발전이 아닐 수 없습니다!
픽셀 기기에서는 배터리 절약 모드를 사용하면 자동으로 다크 모드가 켜진다고 합니다.

###제스처 조작
<img src="https://3.bp.blogspot.com/-RWH3iXU-t_U/XW6LZr8bvfI/AAAAAAAAL38/zZtCyrxnE9U-yqM79c6oG41UPHeJWY9NgCLcBGAs/s1600/gesture.gif" alt="Gesture Navigation" width="50%"/>


*Image Source: [Android Developers Blog](https://android-developers.googleblog.com/2019/09/welcoming-android-10.html)*

안드로이드 9(Pie)에서 처음 등장한 순정 제스처 조작은 사실 다소 불편하다는 이야기를 많이 들었는데 (특히 제스처 조작인 주제에 기존 버튼 조작만큼 화면을 많이 잡아먹는 점이 마음에 안 들었습니다), 이번에는 ~~아이폰과 좀 많이 유사한~~ 하단의 얇은 막대를 밀어서 조작하는 방식으로 변경되었습니다. 뒤로가기는 드디어 가장자리에서 화면을 미는 방식으로 변경되었는데, 아무래도 동일한 동작을 이미 사용하고 있는 앱들이 많다는 문제를 한 번 밀기와 두 번 밀기를 구분하는 방법으로 해결하고 있습니다. 밀기 제스처가 이미 있는 앱의 경우 가장자리를 한 번 밀면 앱의 자체 동작이 실행되고, 두 번 밀면 뒤로가기가 됩니다. 굉장히 비직관적이긴 하지만 뭐 안드로이드는 늘 그랬으니까요.
###데스크탑 모드
삼성 DeX와 같은 기능을 제조사들이 좀 더 쉽게 구현할 수 있도록 이번 안드로이드 10에는 데스크탑 모드가 숨겨져 있습니다. 기본적으로는 아주 [허접한 비주얼](https://www.youtube.com/watch?v=Q7lW62_2Grs)을 자랑하는 데스크탑 모드의 비주얼은 런쳐로 구현되는 것 같은데, [이미 구현한 사람이 있습니다!](https://www.youtube.com/watch?v=Fj2P0omAkek)
##(개발자 관점에서) 굵직한 변경점
###USE_FULL_SCREEN_INTENT 권한 추가
이제 전체 화면 알림을 띄우는 앱은 Manifest에서 USE_FULL_SCREEN_INTENT 권한을 설정해 줘야 합니다. 이 권한이 없을 경우 전체 화면 알림 요청은 무시되고 로그에 메시지가 출력됩니다 (학교 앱이 언제쯤 대응할지 궁금해지네요). 다만, 이 권한은 자동 승인되며 유저의 별도 승인이 필요하지는 않습니다.
###멀티 윈도우 사용 시 onPause(), onResume() 동작 변경
<a href="http://www.youtube.com/watch?v=4dIULf4ma_I">

<img src="http://img.youtube.com/vi/4dIULf4ma_I/0.jpg" alt="App Continuity" width="100%"/>
</a>

*클릭하면 유튜브로 갑니다*


이전 안드로이드 버전에서는 멀티 윈도우 사용 시 하나의 앱만 Resumed 상태를 유지하고 나머지 앱은 Paused 상태로 동작했습니다. 삼성 등 일부 OEM은 다르게 구현한 것 같긴 하던데, ~~제가 마지막으로 써 본 삼성 폰이 갤럭시 S5인지라~~ 잘은 모르겠습니다. 아무튼 [갤럭시 폴드](https://m.blog.naver.com/PostView.nhn?blogId=metro1011&logNo=221637412583&proxyReferer=https%3A%2F%2Ft.co%2FtaMkAzltZ2%3Famp%3D1) 같이 멀티 윈도우를 지원하는 폴더블 스마트폰의 도래에 맞추어, 안드로이드 10에서는 멀티 윈도우에 떠 있는 모든 앱이 Resumed 상태를 유지하도록 변경되었습니다. 이 때 사용자가 앱과 상호작용하고 있는지 아니면 켜 놓고 다른 작업을 하고 있는지 알기 쉽도록 "Topmost Resumed" 라는 개념이 생겼고, 이에 대한 변경점은 onTopResumedActivity() 콜백으로 받아올 수 있습니다.
###폴더블 대응 resizableActivity, minAspectRatio 속성
폴더블 스마트폰으로 인해 액티비티 크기가 변경되는 일이 전보다 훨씬 자주 일어나게 되었는데, 이로 인해 안드로이드 10부터는 resizableActivity 속성이 false로 지정된 앱은 다른 스크린으로 이동하거나 하는 경우 호환성 모드로 동작합니다. 또한 안드로이드 10에서는 minAspectRatio 속성이 새롭게 등장했는데, 이전의 maxAspectRatio 속성과 함께 앱이 지원하는 최대/최소 화면비를 지정합니다.
##맺으며
안드로이드 10은 여러모로 역대 안드로이드 버전 중 가장 극적으로 바뀌는 버전 중 하나라고 할 수 있을 것 같습니다. 일단 겉보기에 확 달라졌다는 점이 크게 다가오네요. 물론 OEM들의 전통적으로 느려터진 업데이트를 감안하면, 여러분 중 대부분이 버전을 만져 볼 수 있는 건 내년이나 되어서의 이야기일 겁니다. 그 때까지 존버!