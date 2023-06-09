﻿![Build Status](https://github.com/beautiful-store/gosiren/actions/workflows/build.yml/badge.svg)
![GitHub go.mod Go version](https://img.shields.io/github/go-mod/go-version/beautiful-store/gosiren)
![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/beautiful-store/gosiren)

# goSiren
여러 서비스가 추가되면서 각각 장애 알림이 필요했고 장애 알림 모듈을 github 에서 관리합니다.
# install
`go get -u github.com/beautiful-store/gosiren`

# 사용법
```
type UserClaim struct {
	Id          int64    `json:"id"`
	Name        string   `json:"name"`
}

...


var projectNo, projectMemberGroupId, title, content string
	siren := gosiren.GoSiren{
		TargetCodes:        []int{http.StatusBadRequest, http.StatusInternalServerError},
		TargetEnvironments: []string{gosiren.EnvironmentProduction},
		TargetHost:         []string{"www.test.com"}
		}
	alarm := gosiren.Alarm{
			Request: req, 
			Code: code, 
			StackTrace: stackTrace, 
			UserName: userClaim.Name,
			UserID: int(userClaim.Id)
	}

//code : 에러 상태 코드
//environment : 운영/개발 등..
//host  ex)"www.test.com"
if siren.IsSending(code, environment,host) {
			title, content, err = siren.Setting(&alarm)
			if err != nil {
				logrus.Error(err)
			}
}

//알림을 외부로 전송 
go adapter.DoorayAdapter{}.SendTask(title,content)
```
