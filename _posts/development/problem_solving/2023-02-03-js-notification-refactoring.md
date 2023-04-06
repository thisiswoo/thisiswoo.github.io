---
layout: post
title: "[리팩토링] 무분별한 팝업창 코드 공통 함수로 리팩토링"
subtitle: "무분별한 팝업창 코드 공통 함수로 리팩토링"
category: development
tags: problem_solving 리팩토링
image:
    path: /assets/img/development/problem_solving/2023-02-03/refactoring_common_popup_cover.png
---

<span style="font-size:30px;">\#**목차**</span>
* this unordered seed list will be replaced by the toc
{:toc}

# 문제
- 프로젝트 이관 작업 중 유효성 및 팝업창의 코드를 무분별히게 생성하여 중복코드 발생.
- 사용자가 조회, 등록, 등 작업을 하려는 행위들을 했을 때 사용자에게 안내 팝업창과 저장했으면 저장, 취소했으면 취소하는 행동의 팝업창을 띄워주고 있는데, 그 코드들을 계속 새로 작성하여 중복코드가 상당히 발견되는 상황을 발견하였다.

## 문제의 재현 코드
```javascript
// file: "문제의 javascript파일.js"
function processDone(jqXHR, textStatus, errorThrown) {
    // ...
	if (jqXHR.status == 500) {
		Swal.fire({
			title: '알림', 
			text: '내부 시스템 에러', 
			icon: 'error',
			confirmButtonText: '확인', 
		}).then(function (result) {
			if (result.value) {
				alertify.error('내부 시스템 에러');  
			}
		});
	} else if (jqXHR.status == 404) {
		Swal.fire({
			title: '알림', 
			text: '경로가 잘못 되었습니다.',
			icon: 'error',
			confirmButtonText: '확인', 
		}).then(function (result) {
			if (result.value) {
				alertify.error('경로가 잘못 되었습니다.'); 
			}
		});
	} else if (jqXHR.status == 408) {
		Swal.fire({
			title: '알림', 
			text: '잠시 후 다시 시도해 주세요.', 
			icon: 'error',
			confirmButtonText: '확인', 
		}).then(function (result) {
			if (result.value) {
				alertify.error('잠시 후 다시 시도해 주세요.');
			}
		});
	} else if (jqXHR.status == 401) {
		errorAlert('접근 권한이 없습니다.'); 
	} else if (jqXHR.status == 403) {
		Swal.fire({
			title: '알림', 
			text: 'ID 또는 비밀번호를 확인해주세요.',
			icon: 'error',
			confirmButtonText: '확인', 
		}).then(function (result) {
			if (result.value) {
				alertify.error('ID 또는 비밀번호를 확인해주세요.'); 
			}
		});
	}

	if (jqXHR.getResponseHeader("SESSION_EXPIRED") != null ) {
		Swal.fire({
			title: '알림',
			text: '세션이 만료되어 로그인 페이지로 이동합니다.', 
			icon: 'error',
			confirmButtonText: '확인',
		}).then(function (result) {
			if (result.value) {
				location.href = "/login";
			}
		});
		
	}
}
```
문제의 재현 코드
{:.figcaption}

- 위 코드를 보게 되면 조건문 안에 `Bootstrap`의 팝업 기능인 `Swal.fire()` 부분의 코드들이 중복적으로 계속 발생한다는 것을 알 수 있다.
- 위 코드처럼 사용하게 되면 만약, 기능들이 추가되거나 사용자에게 팝업 형태의 알림을 보여줘야 할 때 계속 비슷한 코드를 사용해야 한다는 문제가 발생한다.
- 그것을 방지하고 함수로 정의하여 호출하는 형태로 바꾸어 아래 코드처럼 리팩토링하였다.

## 리팩토링 재현 코드
```javascript
// file: "sg.ajax.js"
function processFail(jqXHR, textStatus, errorThrown) {
    // ...
	if (jqXHR.stat표us == 500) {
		notification('내부 시스템 에러', 'error', null);
	} else if (jqXHR.status == 404) {
		notification('경로가 잘못 되었습니다.', 'error', null);
	} else if (jqXHR.status == 408) {
		notification('잠시 후 다시 시도해 주세요.', 'error', null);
	} else if (jqXHR.status == 401) {
		notification('접근 권한이 없습니다.', 'error', null);
	} else if (jqXHR.status == 403) {
		notification('ID 또는 비밀번호를 확인해주세요.', 'error', null);
	}
    // ...
    if (jqXHR.getResponseHeader("SESSION_EXPIRED") != null ) {
        notification('세션이 만료되어 로그인 페이지로 이동합니다.', 'error', () => { location.href = "/login"; });
    }
	// ...
}
```
리팩토링한 재현 코드
{:.figcaption}

- 위 코드처럼 리팩토링하여 약 `71%`의 코드 사용량을 줄였다.
- 물론 코드가 예제 파일 파일 한 곳에 결과를 반영한 것으로 실제와 다를 순 있다.
- 하지만, 확실한 건 코드의 사용량이 예제 코드를 통해서만 보아도 많이 줄일 수 있다는 것을 볼 수 있다.

```javascript
// file: "리팩토링.js"
/**
 * Notification (success/error/warning/question/info alert)
 *
 * @param text      팝업 문구
 * @param type      팝업 종류 : success, error, warning, info
 * @param method   실행 메서드
 * @returns Swal.fire
 */
function notification(text, type, method) {
   // When text and type are not used
   if (text === null || type === null) {
      return Swal.fire({
         title: '알림', 
         text: '시스템 오류가 발생했습니다. 관리자에게 문의해주세요.',
         icon: 'warning',
         confirmButtonText: '확인', 
      });
   }
   // Common Alert
   return Swal.fire({
      title: '알림', // 알림
      text: text,
      icon: type,
      showCancelButton: type === 'question' ? true : false,
      confirmButtonText: '확인', 
      cancelButtonText: '취소', 
   }).then((result) => {
      if (result.isConfirmed) {
         method();
      } else if (result.isDismissed) {
         notification('취소 하였습니다.', 'warning', null);
      }
   });
}
```
공통 팝업 함수 재현 코드
{:.figcaption}

- 위 공통 함수 코드로 리팩토링하여 사용자에게 필요한 팝업창을 제공하고 싶을 때 호출하여 사용하면 된다.
- 매번 사용자에게 팝업 알림 창을 보여줘야 하는 코드를 만들어서 보여주는 것보다 그곳에서 해당 공통 함수를 호출하여 제공하면 코드 사용량이 줄어들어 유지 보수에도 편하게 된다.


