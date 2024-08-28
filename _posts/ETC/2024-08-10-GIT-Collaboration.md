---
title: '[Git] 깃을 활용하여 협업하기'

categories:
  - Etc
tags:
  - Git

last_modified_at: 2024-08-10T08:06:00-05:00

classes: wide
---

깃을 활용하여 협업하는 환경을 구축하는 과정에 대한 포스팅입니다. 깃허브에 관한 기본적인 내용은 [깃허브 개념 및 깃 명령어]({{site.url}}/etc/Git_Basic/)를 참고하시길 바랍니다.

1. 상대방의 깃 저장소를 자신의 깃 저장소로 가져오기
2. 자신의 깃 저장소를 자신의 PC로 가져오기
3. 상대방의 깃 저장소와 자신의 깃 저장소 연결하기
4. 자신의 PC의 변경 사항을 자신의 깃 저장소에 적용하기
5. 자신의 깃 저장소의 변경 사항을 상대방의 깃 저장소에도 적용하기
6. 상대방의 깃 저장소의 변경 사항을 자신의 깃 저장소에도 적용하기

## 1. 상대방의 깃 저장소를 자신의 깃 저장소로 복제하기 : `Fork`

먼저 깃허브 웹페이지에서 가져오고 싶은 저장소로 이동합니다. 저의 경우 YonseiESC 계정의 ESC-20SUMMER 저장소를 Fork 하려고 합니다. 웹페이지 우측 상단에 보이는 Fork 버튼을 클릭합니다.

![]({{site.url}}/assets/images/git_collabo_1.png)

Fork 가 완료되면 다음과 같은 화면이 나타납니다.

![]({{site.url}}/assets/images/git_collabo_2.png)

화면 왼쪽 상단을 보면, 상대방의 저장소가 자신의 저장소로 추가된 것을 볼 수 있습니다. 저의 경우 Hoya2021을 Fork 하여 dongho1994 이 생긴 것을 확인할 수 있습니다.

이처럼 `Fork`는 다른 사람의 깃허브 저장소를 자신의 깃허브 저장소로 그대로 복제하는 것을 말합니다.

## 2. 자신의 깃 저장소를 자신의 PC로 복제하기 : `git clone`

먼저 자신의 저장소의 URL을 복사합니다. 이때 주의할 점은 깃허브 웹페이지의 URL이 아니라 깃허브 웹페이지에서 Clone 버튼을 클릭했을 때 나오는 URL을 복사해야 한다는 것입니다. URL 옆의 아이콘을 클릭하면 자동으로 복사가 됩니다.

![]({{site.url}}/assets/images/git_collabo_3.png)

그러고 나서 터미널에 다음과 같이 명령어를 입력합니다. 이때 주의해야 할 점은 `git clone` 뒤에 상대방의 저장소의 URL이 아닌 방금 복사한 자신의 저장소의 URL을 적어주어야 한다는 점입니다.

```bash
git clone [자신의 저장소의 HTTP]
# git clone https://github.com/KIM-1994.git
```

`git clone` 명령어는 깃허브 웹페이지에 있는 기존 저장소를 복제하여 PC로 가져오는 명령어입니다. 다른 프로젝트에 참여하거나 깃 저장소를 복사하고 싶을 때 주로 사용합니다.

이렇게 하게 되면 이제 깃허브 웹페이지의 저장소에 올라와있는 모든 파일들이 내 컴퓨터로 들어와 PC에서 여러 작업들을 수행할 수 있습니다.

## 3. 상대방의 깃 저장소와 자신의 깃 저장소 연결하기 : `git remote add`

먼저 터미널을 켜고 다음과 같은 명령어를 실행해봅니다.

```bash
git remote -v
```

`git remote -v` 명령어는 저장소 목록을 보여주는 명령어로, `git remote` 명령어는 저장소 이름을, `git remote -v` 명령어는 저장소 이름과 저장소 URL을 함께 보여줍니다. 실행 결과 저의 경우 origin이라는 이름을 가진 저장소 하나만 존재함을 볼 수 있습니다. 이는 이전에 clone 했던 자신의 저장소, 즉 저의 경우 cheris8/ESC20-SUMMER을 가리킵니다.

다음과 같은 명령어를 실행합니다. 이때 [상대방의 저장소의 별칭]은 사용자가 자신이 원하는대로 설정할 수 있습니다.

```bash
git remote add [상대방의 저장소의 별칭] [상대방의 저장소의 HTTP]
# git remote add esc https://github.com/Hoya2021.git
```

이렇게 하면 자신의 저장소와 함께 상대방의 저장소도 등록되게 됩니다. 다시 한번 `git remote` 명령어를 실행해봅시다.

```bash
git remote -v
```

실행 결과 저의 경우 origin 과 esc 두개의 저장소가 존재하는 것을 확인할 수 있습니다. 이렇게 상대방의 깃 저장소와 자신의 깃 저장소를 연결하여 좀 더 편리하게 프로젝트를 수행할 수 있습니다.

## 4. 자신의 PC의 변경 사항을 자신의 깃 저장소에 올리기 : `git push`

내려받은 폴더에서 여러 작업을 수행한 후, 변경 사항을 자신의 깃 저장소에 올리는 방법에 대해 알아보겠습니다.

먼저 터미널을 켜고 해당 폴더로 이동합니다. 그런 다음 다음과 같이 명령어를 입력합니다.

```bash
git add .
git add [파일경로/파일명]
```

`git add` 명령어는 깃이 파일을 추적하도록 하는 명령어로, `git add .` 명령어는 모든 파일을, `git add [파일경로/파일명]` 명령어는 특정 파일을 추적하도록 합니다.

다음으로 아래와 같이 명령어를 실행합니다. [코멘트] 자리에는 변경 사항에 대한 내용을 입력합니다.

```bash
git commit -m '[코멘트]'
```

`git commit` 명령어는 자신의 PC에서 변경 사항을 저장하는 명령어입니다. commit 은 PC에서 이루어지는 것으로, `git commit` 명령어만으로는 깃허브 웹페이지의 자신의 저장소에 변경 사항이 반영되지 않습니다.

이를 위해서는 아래와 같은 명령어를 실행해야 합니다.

```bash
git push origin master
```

`git push` 명령어는 변경 사항을 자신의 저장소로 발행하여 자신의 저장소에 변경 사항이 반영될 수 있도록 합니다. push를 하게 되면 이제 깃허브 웹페이지의 저장소도 자신의 PC와 동일한 상태가 되는 것을 볼 수 있습니다.

## 5. 자신의 깃 저장소의 변경 사항을 상대방의 깃 저장소에도 적용하기 : `Pull Request`

이렇게 자신의 PC에서의 변경 사항을 자신의 깃 저장소에 적용한다 하더라도, 상대방의 저장소에는 이러한 변경 사항이 적용되지 않습니다. 이번에는 자신의 깃 저장소의 변경 사항을 상대방의 깃 저장소에도 적용하는 방법에 대해 알아보도록 하겠습니다.

먼저 깃허브 웹페이지에 접속하여 자신의 저장소로 이동한 다음 화면 상단의 Pull Request 탭을 클릭합니다.

![]({{site.url}}/assets/images/git_collabo_4.png)

그러면 위와 같은 화면이 나오는데, 화면 오른쪽 상단의 New Pull Request 버튼을 클릭합니다.

## 6. 상대방의 깃 저장소의 변경 사항을 자신의 깃 저장소에도 적용하기 : 'git fetch & git merge'

먼저 터미널을 켜고 해당 폴더로 이동한 다음 다음과 같은 명령어를 실행해봅니다.

```bash
git remote -v
```

실행 결과 저의 경우 origin(자신의 저장소)과 esc(상대방의 저장소) 두개의 저장소가 존재하는 것을 확인할 수 있습니다.

그러고 나서 다음 명령어들을 실행합니다.

```bash
git fetch [상대방의 저장소의 별칭]
git merge [상대방의 저장소의 별칭]/master
# git fetch esc
# git merge esc/master
```

그러면 자신의 PC가 상대방의 저장소와 같은 최신 상태로 업데이트 됩니다. 하지만 상대방의 깃 저장소의 변경 사항이 자신의 PC로 적용된 것일 뿐, 자신의 깃 저장소에는 적용되지 않습니다. 이를 위해서는 다음과 같은 명령어를 실행해야 합니다.

```bash
git push origin master
```

이렇게 되면 깃허브 웹페이지의 자신의 저장소도 상대방의 저장소와 동일한 상태로 업데이트 됩니다.

-------------------
* 이 포스트는 최주호 강사님과 https://cheris8.github.io/etc/GIT-Basic/에서 참고하였다.

#패스트캠퍼스 #패스트캠퍼스AI부트캠프 #업스테이지패스트캠퍼스 #UpstageAILab #국비지원 #패스트캠퍼스업스테이지에이아이랩 #패스트캠퍼스업스테이지부트캠프