---
layout: post
title: "[Linux] 서버 배포에 Linux를 사용하는 이유"
subtitle: "Why use Linux for server deployment"
category: development
tags: server linux
image:
  path: /assets/img/development/server/2024-06-25-why_use_linux_for_server_deployment/linux_cover.png
---

<span style="font-size:30px;">\#**목차**</span>
* this unordered seed list will be replaced by the toc
{:toc}

# 서버 배포에 Linux를 사용하는 이유
## 운영체제란?
![](/assets/img/development/server/2024-06-25-why_use_linux_for_server_deployment/type_of_os.png){:.centered width="80%"}
다양한 운영체제의 종류
{:.figcaption}

- 운영체제란 사용자가 컴퓨터를 쉽게 다룰 수 있게 해주는 인터페이스이며, 현재 운영체제는 굉장히 다양한 운영체제들이 존재한다.
- 일반인들에게 가장 널리 사용되고 있는 PC의 운영체제에는 Windows, MAC OS 등일 텐데, 서버 배포 관련된 OS는 아마 Linux가 가장 압도적으로 많을 것이며, 모바일의 경우 Android, iOS 두가지 운영체제가 가장 널리 사용되고 있다. 
- 해당 포스트는 그 이유에 대해 설명하려 한다.

## 전 세계 Descktop 운영 체제 시장 점유율
### Desktop 점유율
![](/assets/img/development/server/2024-06-25-why_use_linux_for_server_deployment/worldwide_os_statistics_desktop.png){:.centered width="80%"}
[전 세계 Desktop 운영체제 시장 점유율 - by statcounter](https://gs.statcounter.com/os-market-share/desktop/worldwide/#monthly-200901-202405){:target="_blank"}
{:.figcaption}

- 위 Desktop 차트를 보게 되면 Microsoft 사의 Windows 운영체제가 압도적으로 높다는 걸 알 수 있다.
- 그렇다면 왜 Microsoft 사의 Windows 운영체제가 압도적으로 높을까?

### 전 세계적으로 많이 사용되는 운영체제 Microsoft사의 Windows
#### Windows의 역사와 대중성
- 1985년도에 처음 발표된 [윈도우 1.0](https://en.wikipedia.org/wiki/Windows_1.0){:target="_blank"}은 당시 [DOS](https://en.wikipedia.org/wiki/DOS){:target="_blank"}를 많이 사용하던 [CLI(Command-line interface)](https://en.wikipedia.org/wiki/Command-line_interface){:target="_blank"}의 방식을 사용자 친화적인 멀티태스킹과 [GUI(Graphical user interface)](https://en.wikipedia.org/wiki/Graphical_user_interface){:target="_blank"} 방식으로 시도를 하였다. 이때 Apple 사의 MAC OS와 매우 유사했는데, 이유는 마이크로소프트가 매킨토시용 소프트웨어를 업그레이드해 주는 대신 맥 OS 디자인을 일부 차용할 수 있는 라이선스 계약을 애플과 맺었기 때문이다. 사실 윈도우 1.0을 개발할 때 마이크로소프트가 초기 매킨토시 소프트웨어 개발에 깊이 관여하고 있었고, 매킨토시용 소프트웨어를 업그레이드해주는 대신 맥OS 디자인을 일부 차용할 수 있는 라이선스 계약을 애플과 맺고 있었다. 당시 계약서는 애매한 표현으로 작성됐는데, 이 치명적인 실수로 인해 애플은 나중에 마이크로소프트사가 윈도우 2.0과 윈도우 3.0을 출시하자 뼈아픈 후회를 하게 되었다고 한다.
- 그렇게 1987년 말 [윈도우 2.0](https://en.wikipedia.org/wiki/Windows_2.0){:target="_blank"}을 출시하게 되었는데 이 역시 윈도우1.0과 마찬가지로 [MS-DOS](https://en.wikipedia.org/wiki/MS-DOS){:target="_blank"} 기반으로 동작하기 때문에 운영체제라기보다는 일종의 소프트웨어에 가깝다. 이 땐, 계산기, 시계 등 사용자에게 필요한 소프트웨어를 제공하였다. 또한 이 전 버전 윈도우1.0에 없던 경쟁사 MAC OS의 창 겹치기 기능을 추가하였다고 한다. 
- 1990년 [윈도우 3.0](https://en.wikipedia.org/wiki/Windows_3.0){:target="_blank"} 버전은 처음으로 널리 성공한 버전이며, 좀 더 나은 멀티테스킹 기능을 갖추고, MS-DOS의 파일 관리/프로그램 실행 역할은 <span style="color:#ff8080"><strong>아이콘</strong></span>을 기반으로 하여 사용자들에게 큰 사랑을 받게 되었다.
- 1995년 [윈도우 95](https://en.wikipedia.org/wiki/Windows_95){:target="_blank"}, 1998년 [윈도우 98](https://en.wikipedia.org/wiki/Windows_98){:target="_blank"}, 우리가 윈도우를 가장 많이 사용하게 만들어준 [윈도우 XP](https://en.wikipedia.org/wiki/Windows_XP){:target="_blank"}, 현재까지도 많이 사용하는 [윈도우 10](https://en.wikipedia.org/wiki/Windows_10) 까지 사용 친화적으로 발전해오면서 각 기업에서 서류 작업을 필기로 하던 업무를 컴퓨터로 대체했고, PC(Personal Computer - 개인용 컴퓨터)의 이름처럼 회사뿐만 아니라 각 가정에서도 사용 가능한 OS를 만들게 되면서 윈도우 OS가 전 세계 시장 점유율은 굉장히 높아졌다. 또한 게임, 관공서 프로그램, 인터넷의 발전, 애플리케이션의 발전 등 다른 프로그램과의 호환성이 굉장히 좋아 윈도우의 시장을 더욱 크게 하였다.  

이렇게 전 세계적으로 가장 많이 사용하게 되는 Desktop의 운영체젝가 되었는데, 여기엔 단점도 존재한다.

#### Windows의 단점
1. 유료 운영체제 정책을 유지하면서 회사와 가정에서는 일부 정품을 구매하는 비용을 아끼기 위해 일명 크랙 버전으로 널리 사용되기도 하였다.
2. 또한 오픈소스가 아니기 때문에 버그나 보안에 취약한 점을 찾아내어 업데이트하는 시간이 굉장히 길어지기 때문에 보안에 취약한 단점이 존재한다.
3. 윈도우 운영체제는 소스가 공개되지 않아 사용자가 운영체제를 수정하거나 커스터마이징 하는데 제한이 있을 수 있다.

사실 3번의 커스터마이징보단 1, 2번의 이유가 가장 단점일 것이다. 보통의 사용자라면 굳이 커스터마이징을 하는 사람들은 많지 않을듯하다.<br/>
위 장점들로 인해 전 세계에서 가장 사랑받는 운영체제로 자리매김해온 운영체제이다.

### 그렇다면 왜 Linux의 운영체제가 탄생하였는가?
- 그 전에 유닉스(Unix)와 미닉스(MINIX)의 등장 배경을 간략하게 알아야 한다.

#### 유닉스(Unix)의 등장
- 1960년대 말과 1970년대 초에 벨 연구소(Bell Labs)에서 개발된 Unix는 다목적 운영체제로, 특히 학계와 연구소에서 널리 사용되었는데, 그 당시 상업적으로 이용할 수 있었지만, 라이선스 비용이 비쌌고 소스 코드 접근이 제한적이었다.

#### 미닉스(MINIX)
- 미닉스(MINIX)는 유닉스 계열의 운영 체제의 하나이다. 1987년 네덜란드의 암스테르담 자유대학에 재직하던 교수 [앤드류 타넨바움](https://en.wikipedia.org/wiki/Andrew_S._Tanenbaum){:target="_blank"}이 교육 목적으로 Unix의 축소판인 Minix를 개발하였는데, Minix는 학습용으로 설계되었고, 소스 코드가 공개되어 있어 학생들이 운영체제의 내부 구조를 이해하는 데 도움이 되었고 이는 나중에 리눅스 커널의 창조에 큰 영감을 주었다고 한다.

#### 리눅스의 탄생
- 1991년, 헬싱키 대학교의 재학생이었던 [리누스 토르발스](https://en.wikipedia.org/wiki/Linus_Torvalds){:target="_blank"}는 운영 체제에 관한 호기심을 가지고 있었는데, 당시 교육용으로만 국한되었던 미닉스(MINIX) 라이선스에 실망한 토르발스는 궁극적으로 리눅스 커널이 되었던 자신만의 운영 체제 커널에 대한 작업을 시작하였다.
- 토르발스는 미닉스에서 리눅스 커널의 개발을 시작했으며, 미닉스용으로 개발된 응용 프로그램 또한 리눅스에서 사용되었고, GNU의 응용 프로그램들 또한 모든 미닉스 구성 요소를 대체하였는데, 그 이유는 미숙한 운영 체제에 [GNU](https://en.wikipedia.org/wiki/GNU){:target="_blank"} 프로젝트에 자유로이 이용 가능한 코드를 사용하는 것이 이점이 있었기 때문이다. 
- 토르발스는 상용 재배포를 금지하였던 자신의 오리지널 라이선스로부터 [GNU GPL(일바 공중 사용 허가서)](https://en.wikipedia.org/wiki/GNU){:target="_blank"}로의 전환을 시작하였는데, 이에 개발자들은 GNU 구성 요소들을 리눅스 커널로 통합하는 일을 시작함으로써 온전히 기능하는 자유 운영 체제(무료 운영체제)를 만들 수 있게 되었다.

#### 리눅스의 상업적, 대중적 활용
![](/assets/img/development/server/2024-06-25-why_use_linux_for_server_deployment/worldwide_os_statistics_mobile.png){:.centered width="80%"}
[전 세계 Mobile 운영체제 시장 점유율 - by statcounter](https://gs.statcounter.com/os-market-share/mobile/worldwide#monthly-200901-202405){:target="_blank"}
{:.figcaption}

- 1990년대 중반, 운영 환경에 리눅스를 채택하는 일이 슈퍼 컴퓨팅 커뮤니티에서 불붙기 시작했다. NASA(미 한공 우주국) 등의 단체들이 점차 운영 체계에 비용이 드는 기계들에 리눅스로 구동하는 저렴한 일상 컴퓨터 클러스터로 대체하기 시작했다. 델과 IBM, 이후 휴렛 팩커드(HP)가 데스크톱 운영 체제를 독점하던 마이크로소프트에서 자유를 얻고자, 리눅스 지원을 제공하기 시작했다. 
- 뿐만 아니라 IBM, Google, Amazon 등 수많은 IT 기업들이 리눅스를 채택하고 지원하기 시작하였고, Ubuntu와 Fedora 등 사용자 친화적인 Desktop 배포판이 등장하게 되었다.
- Google의 Android 운영체제가 리눅스 커널을 기반으로 개발되어 모바일 시장에 리눅스 기반의 운영체제인 Android가 많은 사랑을 받게 된다.
- 또한, 전 세계 개발자 커뮤니티의 기여와 오픈 소스 라이선스가 리눅스의 발전을 굉장히 촉진시키고, 클라우드와 컨테이너 기술의 부상으로 인해 AWS, GCP, Azure 등의 클라우드 서비스와 Docker, Kubernetes(K8s) 등의 컨테이너 기술이 리눅스를 기반으로 작동됨으로써 운영체제 시장에서 리눅스의 사용량은 굉장히 늘어났다.

## 결론
![](/assets/img/development/server/2024-06-25-why_use_linux_for_server_deployment/worldwide_os_statistics_total.png){:.centered width="80%"}
[전 세계 운영체제 시장 점유율 - by statcounter](https://gs.statcounter.com/os-market-share/mobile/worldwide#monthly-200901-202405){:target="_blank"}
{:.figcaption}

- Linux는 자유로운 소프트웨어 철학과 협력을 통해 만들어진 운영체제이며 이것은 오픈소스(무료)로 전향하였기 때문에 가능한 것이다. 이 결과 리눅스는 오늘날 다양한 분야에서 중요한 역할들을 수행하고 있다. 앞으로도 리눅스는 끊임없이 발전하며 서버, 클라우드 컴퓨팅, 사물인터넷, 인공지능, 우주 개발 등 다양한 분야에서 더욱 중요한 운영체제로 자리매김할 것으로 기대된다.

# Reference
- [Global Server Operating System Market Share, By Operating System, 2023](https://www.fortunebusinessinsights.com/server-operating-system-market-106601){:target="_blank"}
- [TOP500 - Operating System](https://www.top500.org/statistics/list/){:target="_blank"}
- [statcounter - Desktop Operating System Market Share Worldwide](https://gs.statcounter.com/os-market-share/desktop/worldwide/#monthly-200901-202405){:target="_blank"}
- [윈도우에 대한 거의 모든 역사](https://post.naver.com/viewer/postView.naver?volumeNo=32050554&memberNo=31588952){:target="_blank"}






