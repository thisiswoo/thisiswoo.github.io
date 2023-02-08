- sg.ajax.js refactoring 전

```javascript
// file: "sg.ajax.js"
function processFail(jqXHR, textStatus, errorThrown) {
	// let lang = $.cookie('lang');
	// loadi18n(lang, 'messages');
	try {
		// [로딩 부모 컨테이너 삭제 실시]
		$('body').find('div#spinnerLay1000').remove();
	}
	catch (exception) {
	}
	if (typeof (console) !== 'undefined' && tㄷypeof (console.log) !== 'undefined') {
		console.log(textStatus + " - " + jqXHR.status + " (" + errorThrown + ")");
	}
	if (jqXHR.status == 500) {
		Swal.fire({
			// title: $.i18n.prop('common.alert.notice'), // 알림
			title: '알림', // 알림
			text: '내부 시스템 에러', // 내부 시스템 에러
			// text: $.i18n.prop('kg.ajax.alert.insyserror'), // 내부 시스템 에러
			icon: 'error',
			// confirmButtonText: $.i18n.prop('common.alert.confirm'), // 확인
			confirmButtonText: '확인', // 확인
			//confirmButtonClass: 'btn btn-success mt-2'
		}).then(function (result) {
			if (result.value) {
				//alertify.error($.i18n.prop('kg.ajax.alert.insyserror')); // 내부 시스템 에러
			}
		});
	} else if (jqXHR.status == 404) {
		Swal.fire({
			// title: $.i18n.prop('common.alert.notice'), // 알림
			title: '알림', // 알림
			// text: $.i18n.prop('kg.ajax.alert.wrongpath'), // 경로가 잘못 되었습니다.
			text: '경로가 잘못 되었습니다.', // 경로가 잘못 되었습니다.
			icon: 'error',
			// confirmButtonText: $.i18n.prop('common.alert.confirm'), // 확인
			confirmButtonText: '확인', // 확인
			//confirmButtonClass: 'btn btn-success mt-2'
		}).then(function (result) {
			if (result.value) {
				//alertify.error($.i18n.prop('kg.ajax.alert.wrongpath')); // 경로가 잘못 되었습니다.
			}
		});
	} else if (jqXHR.status == 408) {
		Swal.fire({
			// title: $.i18n.prop('common.alert.notice'), // 알림
			title: '알림', // 알림
			// text: $.i18n.prop('kg.ajax.alert.waitandtry'), // 잠시 후 다시 시도해 주세요.
			text: '잠시 후 다시 시도해 주세요.', // 잠시 후 다시 시도해 주세요.
			icon: 'error',
			// confirmButtonText: $.i18n.prop('common.alert.confirm'), // 확인
			confirmButtonText: '확인', // 확인
			//confirmButtonClass: 'btn btn-success mt-2'
		}).then(function (result) {
			if (result.value) {
				//alertify.error($.i18n.prop('kg.ajax.alert.waitandtry')); // 잠시 후 다시 시도해 주세요.
			}
		});
	} else if (jqXHR.status == 401) {
		// errorAlert($.i18n.prop('login.alert.unauthorized')); // 접근 권한이 없습니다.
		errorAlert('접근 권한이 없습니다.'); // 접근 권한이 없습니다.
	} else if (jqXHR.status == 403) {
		Swal.fire({
			// title: $.i18n.prop('common.alert.notice'), // 알림
			title: '알림', // 알림
			// text: $.i18n.prop('login.alert.fail'), // ID 또는 비밀번호를 확인해주세요.
			text: 'ID 또는 비밀번호를 확인해주세요.', // ID 또는 비밀번호를 확인해주세요.
			icon: 'error',
			// confirmButtonText: $.i18n.prop('common.alert.confirm'), // 확인
			confirmButtonText: '확인', // 확인
			//confirmButtonClass: 'btn btn-success mt-2'
		}).then(function (result) {
			if (result.value) {
				//alertify.error($.i18n.prop('login.alert.fail')); // ID 또는 비밀번호를 확인해주세요.
			}
		});
	}

	if (jqXHR.getResponseHeader("SESSION_EXPIRED") != null ) {
		Swal.fire({
			// title: $.i18n.prop('common.alert.notice'), // 알림
			title: '알림', // 알림
			// text: $.i18n.prop('common.alert.session.expired'), // 세션이 만료되어 로그인 페이지로 이동합니다.
			text: '세션이 만료되어 로그인 페이지로 이동합니다.', // 세션이 만료되어 로그인 페이지로 이동합니다.
			icon: 'error',
			// confirmButtonText: $.i18n.prop('common.alert.confirm'), // 확인
			confirmButtonText: '확인', // 확인
			//confirmButtonClass: 'btn btn-success mt-2'
		}).then(function (result) {
			if (result.value) {
				// location.href = `${ctx}${pageRoot}/login`;
				location.href = "/login";
			}
		});
		
	}
}
```

An optional caption for a code block
{:.figcaption}


- sg.ajax.js refactoring 후
```javascript
// file: "sg.ajax.js"
function processFail(jqXHR, textStatus, errorThrown) {
	// let lang = $.cookie('lang');
	// loadi18n(lang, 'messages');
	try {
		// [로딩 부모 컨테이너 삭제 실시]
		$('body').find('div#spinnerLay1000').remove();
	}
	catch (exception) {
	}
	if (typeof (console) !== 'undefined' && typeof (console.log) !== 'undefined') {
		console.log(textStatus + " - " + jqXHR.status + " (" + errorThrown + ")");
	}
	if (jqXHR.status == 500) {
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
	if (jqXHR.getResponseHeader("SESSION_EXPIRED") != null ) {
		notification('세션이 만료되어 로그인 페이지로 이동합니다.', 'error', () => { location.href = "/login"; });
	}
}
```