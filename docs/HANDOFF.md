# Hand-off: 공공데이터 API 도메인 허용 (보류 중)

- 기록일: 2026-09-25
- 상태: **보류** — 나중에 이어서 진행

## 목표
클라우드 환경 네트워크 허용 도메인에 아래 두 곳이 접속되도록 하기.
- `www.data.go.kr`
- `api.odcloud.kr` (공공데이터포털 API 실제 호출 도메인)

## 지금까지 한 일
1. 저장소에는 허용 도메인 목록이 없음 → 코드 수정 대상 아님. 환경 설정(Network access)에서 추가해야 함.
2. 사용자가 환경 설정에 `api.odcloud.kr` 추가함.
3. 새 세션에서 테스트했지만 **두 도메인 모두 프록시 403**(CONNECT tunnel failed).
   - 원래 허용돼 있던 `www.data.go.kr`까지 막혀서, 도메인 목록 자체가 적용되지 않는 것으로 보임.

## 다음에 확인할 것
1. 수정한 환경이 세션 환경(`env_01QKpZXcZ4dkY7LoFWF5fCoE`)과 같은지
2. Network access 수준이 허용 도메인 목록을 쓰는 수준(Custom 등)인지, 저장됐는지
3. 그래도 막히면 조직(Organization) 정책 여부를 관리자에게 확인

## 재테스트 명령
```bash
curl -sS -o /dev/null -w "%{http_code}\n" --max-time 15 https://api.odcloud.kr/
curl -sS -o /dev/null -w "%{http_code}\n" --max-time 15 https://www.data.go.kr/
```
HTTP 코드가 나오면 성공, `CONNECT tunnel failed, response 403`이면 여전히 차단된 상태.
