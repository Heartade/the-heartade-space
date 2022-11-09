# 미루고 미루다 블로그를

> 아이고 이게 인생이지

블로그를 만드는 건 피곤한 일입니다. 그래서 트위터만 했죠. [트위터가 망하기 전까지는,](https://edition.cnn.com/2022/11/09/tech/elon-musk-twitter-layoffs-india-africa-hnk-intl/index.html) [그리고 그 덕에 사람들이 갑자기 탈중앙화를 외치기 전까지는.](https://edition.cnn.com/2022/11/05/tech/mastodon/index.html)

## 지금 상태

재작년이었나 언젠가 PicoCMS로 운영하던 블로그를 Docsify로 옮기고 있습니다. 하지만 Docsify는 그냥 마크다운을 뿌려 주기만 할 뿐이라서 한계가 많죠. 날짜나 태그 별로 정리하는 게 안 되는 건 정말 치명적이에요.

PicoCMS에는 그게 있었는데, [PicoCMS에 태그 기능 넣으려고 테마 포크까지 했는데](/diary/2019/09/03/new-pico-theme.md) 왜 안 쓰기로 했냐면 - 장기적으로는 직접 블로그를 구현하고, 무엇보다 ActivityPub 표준과 RSS를 구현해서 사람들이 제 헛소리를 팔로우하고 공유할 수 있게 하는 게 목표이기 때문입니다. PicoCMS는 PHP 기반이라 도저히 제 실력으로 뜯어볼 수가 없었고, 일단 마크다운을 정적 서빙하는 뭔가를 만들어서 올려 놓고 차차 백엔드를 만들고 싶었어요.

지금 상태에서도 공유 기능만큼은 반드시 넣고 싶어서 [Docsify용 공유 버튼 플러그인](https://github.com/Heartade/docsify-share)을 만들었습니다. 근데 워낙 대충 만든 거라 아마 영원히 패키지로 릴리즈할 일은 없을 것 같아요.

## 할 일

* [x] [Markdown을 읽어와서 보여주는 블로그 같은 거 구현해 보기](https://github.com/Heartade/heartade-blog)
* [ ] 태그 같은 거 구현해 보기
* [ ] 뭔가 아무튼 구현해 보기