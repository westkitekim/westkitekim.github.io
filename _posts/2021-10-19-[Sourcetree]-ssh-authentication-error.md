---
title: "[Git] 소스트리(Sourcetree) SSH 인증 실패 오류,SSH 연결하기"

categories:
  - Git
tags:
  [Web, java, Javascript]

toc: true
toc_sticky: true

date: 2021-10-20
last_modified_at: 2021-10-20
---

<br>
{% include feature_row id="intro" type="center" %}

그동안 GitBash만 사용하다가 Sourcetree를 시작했다.
그런데 SSH key 인증 실패 팝업창이 계속 나오고 옵션에서 SSH key를 등록하려고 했지만 이상하게 되지 않았다.

이쯤되면 삽질의 Intro 단계로 진입한다..

아 참, 그리고
이전에 Git Bash로 push하려고 할 때마다 

```html
Enter passphrase for key '~/.ssh/id_rsa':
```

이렇게 이전에 SSH 생성시 설정했던 암호를 push를 할 때마다 매번 입력해야 했던 것을 기억하고 
Sourcetree도 인증을 매번 해줘야 하는 것인가.. 라는 아주 무지하고도 대담한 생각으로 ssh-agent 키 등록을 했었다..

GitBash 에 등록한 것으로 소스트리 인증도 다 되는 줄 알았는데 그게 아니였다.

```html
ssh-add ~/.ssh/id_rsa
```

```html
Could not open a connection to your authentication agent.
```

```html
eval $(ssh-agent)
```

```java
Indentity added: ~/.ssh/id_rsa
```

> "무지는 지식보다 더 확신을 갖게 한다" (Ignorance more frequently begets confidence than does knowlege.) - 찰스 다윈('The Descent of Man', 1871)



당연히 될 리가 없다...

1. 난 애초에 ssh key 암호를 생성했기 때문에 ssh agent에 등록해도 어차피 매번 암호를 입력해줘야 했었고,
2. Sourcetree에서 등록이 안되는 문제인데 Git Bash에서 이러고 있었으니.. 

이렇게 ssh-agent를 등록하는 방법은 ssh 생성시에 따로 암호를 설정하지 않은 경우에만 해당한다. 매번 push할 때마다 암호를 입력하지 않기 위해 등록하는 방법이기 때문이다.. 

혹시라도  push를 할 때마다 암호를 매번 입력하는게 번거로우신 분들 중, ssh key 암호를 설정하지 않았다면 [https://gentlesark.tistory.com/86](https://gentlesark.tistory.com/86) 이 분의 블로그를 참고하시기 바랍니다. 

서론이 길다. 

문제상황은 아래와 같다.





































