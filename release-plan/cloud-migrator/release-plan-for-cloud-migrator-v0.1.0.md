### ■ Cloud-Migrator v0.1.0 release 추진 계획

- 릴리스 리드: 김윤곤
- 릴리스 목표일: **2022/12/15 (금) 오후 3시**

- 릴리스 대상 서브시스템(저장소)
	- CM-Honeybee
	- CM-Beetle
	- CM-Grasshopper
	- CM-Damselfly
	- CM-Cicada
	- CM-Ant
	- CM-Butterfly
	- CM-Mayfly
- 비 대상
	- CM-Centipede

- 릴리스 계획: 

| 목표일         | 내용                                       | 비고          |
| -------------- | ------------------------------------------ | ------------- |
| 2023-11-01(수) | Cloud-Barista platform v0.7.x pre-release  | pre-condition |
| 2023-11-17(금) | Cloud-Migrator platform v0.0.x pre-release | 행사 버전    |
| 2023-12-06(수) | Cloud-Barista platform v0.8.0 release      | pre-condition | 
| 2023-12-15(금) | Cloud-Migrator platform v0.1.0 release     |               |


### ■ Cloud-Migrator platform 행사 버전 pre-release

공개 행사를 위한 각 서브시스템의 개발, Pre-release 및 통합 테스트 추진 계획 입니다.

#### 행사 버전 서브시스템 개발, 통합 및 테스트 일정

"행사 버전 서브시스템 개발, 통합 및 테스트 일정"과 관련하여 간단히 안내드립니다. 가능하신 범위 내에서 진행해 주시길 바랍니다.

목표: 
1. 행사 전 최소 1회 Pre-release
2. 공개SW 개발 프로세스에 따른 지속적인 업로드(commits, PRs)를 통한 개선

주의 - 최종본을 업로드 하는 방식은 가급적 지양해 주시길 바랍니다.

행사 전 수행하실 사항:
1. README, script, documentation 등을 GitHub에 업데이트
2. 소스 코드 GitHub 업데이트

일정: 

| 마감일정       | 대상 저장소                               | 비고           |
| -------------- | ----------------------------------------- | -------------- |
| 2023-10-25(수) | CB-Spider                                 | Pre-requisites |
| 2023-11-01(수) | CB-Tumblebug                              | Pre-requisites | 
| 2023-11-16(목) | CM-Honeybee, <br/>CM-Damselfly, <br/>CM-Beetle, <br/>CM-Grasshopper, <br/>CM-Cicada, <br/>CM-Ant, <br/>CM-Mayfly, <br/>CM-Butterfly | (가능한 범위 내에서) <br/>Pre-release, 문서화, 등 수행


### ■ Cloud-Migrator platform v0.1.0 release 준비

Cloud-Migrator platform v0.1.0 release를 위한 각 서브시스템의 개발 및 테스트, 문서화, Pre-release 및 통합 테스트 수행 계획입니다.

#### 안내 사항

각 서브시스템의 Pre-release 관련 이슈 또는 특이사항이 있으신 경우:
- 이메일/Slack 채널(sig-release)에 상시 공유 바랍니다.
- 특히, 일정에 영향을 주는 이슈 발생 시에는 필히 이메일/Slack 채널(sig-release)에 공유하여 주시기 바랍니다.

#### 일정

| 마감일정       | 대상 저장소                                               | 비고           |
| -------------- | --------------------------------------------------------- | -------------- |
| 2023-12-01(금) | CB-Spider                                                 | Pre-requisites |
| 2023-12-05(화) | CB-Tumblebug                                              | Pre-requisites |
| 2023-12-06(수) | Cloud-Barista platform v0.8.0 release                     | Pre-requisites |
| 2023-12-11(월) | CM-Honeybee, CM-Damselfly, <br/>CM-Beetle, CM-Grasshopper | Pre-releases   |
| 2023-12-14(목) | CM-Cicada, CM-Ant, <br/>CM-Mayfly, CM-Butterfly           | Pre-releases   | 
| 2023-12-15(금) | Cloud-Migrator platform v0.1.0 release                    |                |

#### Pre-release 수행 사항

##### 서브시스템 개발 및 테스트
- 서브시스템 기능 개발
- 서브시스템 기능 테스트
- 연관 서브시스템간 통합 테스트 - 트러블슈팅 위주로 진행 (기능 추가/개선 지양)

##### 서브시스템 문서화
- Wiki 업데이트: 설치/기능/API/Design/활용/How-to 등에 대한 문서 업데이트
- README 업데이트 - **README의 모든 정보를 v0.1.0으로 미리 지정**
- ChangeLog 작성 (스타일 참고: https://github.com/cloud-barista/cb-tumblebug/blob/master/CHANGELOG.md)
- 지원 클라우드 정보 업데이트 (스타일 참고: https://github.com/cloud-barista/cb-tumblebug/discussions/429)

##### 서브시스템 Pre-release 생성 
- Pre-release tag 예: v0.0.x
- Pre-release 내용/스타일 참고: https://github.com/cloud-barista/cb-tumblebug/releases/tag/v0.5.5)
- 작업 완료 시, 이메일 또는 Slack sig-release 채널에 공지
	- 공유 내용: Pre-release 링크, Readme 링크, pre-release API 문서, docs, 클라우드별 테스트 사항, 이슈 등

##### 플랫폼 문서화
- Base repository: [cloud-barista/docs](https://github.com/cloud-barista/docs)
- 공개 행사 자료 업데이트: cloud-barista/docs/openseminar
- 기술 문서 업데이트: cloud-barista/docs/technical_docs

#### ■ Cloud-Migrator platform v0.1.0 release 추진

Pre-release 및 통합 테스트 수행사항을 바탕으로 개별 서브시스템 릴리스 및 플랫폼 릴리스를 추진 합니다. 

- 개별 서브시스템 릴리스 (v0.1.0)
- cloud-barista/cloud-migrator 저장소 소스 업데이트 (Git Submodule 활용)
- cloud-barista/cloud-migrator 저장소 Readme 및 사용 방법 업데이트 
- cloud-barista/cloud-migrator 릴리스 관련 문서 업데이트 및 릴리스 생성 (v0.1.0)
