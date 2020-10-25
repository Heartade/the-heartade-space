---
title: Nginx와 PicoCMS로 5분만에 블로그 만들기
excerpt: 제가 겁나게 고생했으니 여러분은 하지 마세요
date: 2019-09-02
image: https://www.elektrollart.de/wp-content/uploads/20161227_0013.jpg
tags: work
---

##들어가며
제가 [블로그를 만들며](%base_url%/first_post) 깨달은 것은 저 같은 웹 초심자들에게는 간단한 블로그도 만들기 굉장히 어렵다는 것이었습니다. 그래서 저처럼 웹은 해 본 적 없는데 왠지 집에 남아도는 서버가 있는(?) 여러분을 위해 아주 쉽지만 아무래도 허술한 블로그 만들기 가이드를 준비했습니다!


제 셋업에 사용된 시스템은 우분투 16.04입니다.


##설치하기

###Nginx 설치하기
```
sudo apt update
sudo apt install nginx
```
nginx 서비스는 인스톨 이후 자동으로 실행됩니다.


###PHP 설치하기
```
sudo apt install php php-fpm
```


현재(9-3-2019) 이 커맨드를 실행했을 때 설치되는 버전은 php 7.0입니다. 혹시 다른 버전이 설치되었고 설정 과정에서 어려움을 겪고 있다면 다음 커맨드로 대체해 주세요.
```
sudo apt install php7.0 php7.0-fpm
```


###PicoCMS 설치하기
[GitHub](https://github.com/picocms/Pico/releases/tag/v2.0.4)에서 PicoCMS를 다운로드한 후 `/var/www/html/`에 압축 해제합니다.


##설정하기

###Nginx 설정하기
`/etc/nginx/sites-available`에 있는 `default` 파일을 열고, 파일의 내용을 다음 텍스트로 바꿔 주세요.


```
server {
  listen 80;
  server_name _;
  root /var/www/html/pico;
  index index.php;
location ~ ^/((config|content|vendor|composer\.(json|lock|phar))(/|$)|(.+/)?\.(?!well-known(/|$))) {
    try_files /index.php$is_args$args =404;
  }
  location ~ \.php$ {
    try_files $uri =404;
    include fastcgi_params;
    fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    fastcgi_index index.php;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param PICO_URL_REWRITING 1;
  }
  location / {
    try_files $uri $uri/ /index.php$is_args$args;
  }
}
```


파일을 저장하고, `cd /etc/nginx/sites-enabled && sudo ln -s ../sites-available/default ./default && sudo nginx -s reload`를 실행해 주세요.


###PicoCMS 설정하기
개인적으로는 [Pico_Edit](https://github.com/chiotis/pico_edit), [Pico-Pagination](https://github.com/rewdy/Pico-Pagination), [Pico-Tags](https://github.com/PontusHorn/Pico-Tags) 플러그인을 사용하고 있습니다. 이 세 가지를 세팅하는 법을 봅시다.


####Pico_Edit
```
cd /var/www/html/pico/plugins && https://github.com/chiotis/pico_edit.git && cd pico_edit
```


pico_edit 폴더의 config.php 폴더를 열고
```$backend_password = '';``` 부분의 따옴표 사이에 관리에 쓸 비밀번호의 SHA-1 해시를 넣어 주세요. SHA-1 해시를 만들어 주는 [사이트](http://www.sha1-online.com)가 있긴 한데, 개인적으로는 SHA-1이 무엇인지 아신다면 오프라인으로 만드는 것을 추천하고 싶긴 합니다. SHA-1 해시가 그렇게까지 안전한 것은 아니다 보니, 보안에 신경을 쓰신다면 이 버전의 Pico_Edit은 안 쓰는 게 좋을 수도 있겠네요.


####Pico-Pagination
```
cd /var/www/html/pico/plugins && wget https://github.com/rewdy/Pico-Pagination/blob/master/10-Pagination.php
```


####Pico-Tags
```
cd /var/www/html/pico/plugins && wget https://github.com/PontusHorn/Pico-Tags/blob/master/PicoTags.php
```


###PicoCMS에 테마 입히기
여기서는 [제 테마](%base_url%/new-pico-theme)를 사용하는 걸로 하죠!
```
cd /var/www/html/pico/themes && mv default default_original && git clone https://github.com/Heartade/clean-blog && mv clean-blog/clean-blog/* ./clean-blog/ && mv clean-blog/content-samples/* ../content/
```


###블로그 정보 설정하기
```var/www/html/pico/config/config.yml``` 을 열고, 설정을 본인 블로그에 맞게 바꿔 주세요.


* `site_title`: 본인 사이트의 제목입니다.
* `theme`: 테마 이름입니다. 여기서는 `clean-blog`를 사용하고 있죠!
* `author`: 작가 이름입니다. 여러분의 이름이나 닉네임을 써 주세요.


작가 이름 아래에 다음 세 항목을 추가해 주세요.
* `facebook`: 여러분의 페이스북 프로필 링크를 적어 주세요. clean-blog 테마에서 사용합니다.
* `twitter`: 여러분의 트위터 링크를 적어 주세요. clean-blog 테마에서 사용합니다.
* `pagination_limit`: 인덱스 한 페이지에 몇 개의 문서가 들어가는지입니다. 일단 `5`로 넣어 주세요. clean-blog 테마에서 사용합니다.


##첫 번째 포스트
여러분의 블로그 주소에 `/pico_edit`을 추가하면 아까 설치한 Pico_Edit 화면으로 들어갑니다. Pico_Edit을 설치하지 않으셨다면 그냥 content/ 폴더에 새로운 .md 문서를 추가하면 끝입니다!
문서는 [마크다운](https://en.wikipedia.org/wiki/Markdown) 형식이고, 문서 첫머리에 다음과 같은 형식으로 메타데이터를 지정해 줘야 합니다.
```
---
title: 포스트 제목
excerpt: 설명
Date: DD-MM-YYYY HH:MM 형식 날짜
Author: 작가명
Profile: 작가 프로필 링크
Img: 헤더 이미지 (일단 https://www.elektrollart.de/wp-content/uploads/20161227_0013.jpg 같은 CC0 이미지로 때워 보세요!)
Tags: 태그 (콤마[,]로 구분해 주세요)
Template: post
---
```
마지막으로, 태그를 추가할 때는 다음과 같은 형식으로 content 폴더에 태그 문서를 만들어 주세요. 태그 문서 이름은 `태그이름.md` 형식입니다.
```
---
title: 태그 제목 (태그 이름과 같을 필요는 없습니다)
Tagline: 태그 부제목
excerpt: 태그 설명
Filter: 태그 이름
Tags: tag
Social:
  트위터_프로필_링크: twitter
  인스타_프로필_링크: instagram
Img: https://unsplash.it/1900/994?image=1075
Template: tag
---
```
index.md와 태그 문서들의 Social 메타데이터에는 트위터, 인스타, 깃헙, 링크드인, 페이스북 등을 넣을 수 있고, 하나도 넣지 않아도 됩니다!
##끝
와, 블로그 세팅이 금방 끝났네요! 이제 끝이면 좋겠지만, 하다 보면 마음에 안 드는 부분이 분명히 생길 것이고 그 때부터는 직접 세팅을 배워 나가야 합니다. 저는 실제로 블로그를 하는 시간의 두 배 이상을 세팅에 쓰고 있는 것 같네요. 아마 이 글을 보고 블로그를 설정하시는 분들은 저와 마찬가지로 대부분 웹 경험이 없는 분들일텐데, 블로그를 운영해 보는 김에 웹 경험도 쌓으며 ~~환상의 동물인~~ 풀스택 개발자가 되면 좋겠습니다. 화이팅!


* 여러 번 말했지만 제가 사실 웹이 처음이라, 여러 실수가 있을 수 있습니다. "이러면 안 된다!" 싶은 부분이 있다면 제 [트위터](https://twitter.com/heartade_)로 알려 주세요!
