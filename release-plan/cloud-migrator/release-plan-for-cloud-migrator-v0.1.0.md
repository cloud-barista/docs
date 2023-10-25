### ■ Cloud-Migrator v0.1.0 릴리스 추진 계획(초안)

- 릴리스 리드: 김윤곤
- 릴리스 목표일: **2022/12/15 (금) 오후 3시**

- 릴리스 대상 서브시스템(저장소) <--- 릴리스 대상 정리 필요
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
| 2023-11-17(수) | Cloud-Migrator platform v0.0.x pre-release | 행사 버전    |
| 2023-12-xx(xx) | Cloud-Barista platform v0.8.0 release      | pre-condition | 
| 2023-12-15(금) | Cloud-Migrator platform v0.1.0 release     |               |


### ■ Cloud-Migrator platform 행사 버전 pre-release

공개 행사를 위해 각 서브시스템의 Pre-release 및 통합 테스트 수행

#### 행사 버전 서브시스템 통합 및 테스트 일정 <---조정 예정

| 마감일정       | 대상 저장소                               | 비고           |
| -------------- | ----------------------------------------- | -------------- |
| 2023-10-25(수) | CB-Spider                                 | Pre-requisites |
| 2023-11-01(수) | CB-Tumblebug                              | Pre-requisites | 
| 2023-11-02(목) | CM-Honeybee, CM-Damselfly                 |                |
| 2023-11-06(월) | CM-Beetle, CM-Grasshopper, (CM-Centipede) |                |
| 2023-11-08(수) | CM-Cicada, CM-Ant                         |                |
| 2023-11-10(금) | CM-Mayfly                                 |                |
| 2023-11-14(화) | CM-Butterfly                              |                |
| 2023-11-16(목) | Cloud-Migrator                            |                |

#### 릴리스 포함 기능 및 테스트 범위
- 프레임워크간 연동 및 시스템 안정화 측면에서만 개선 
	- 행사에 포함된 내용 이외에 추가 기능은 릴리스 대상 아님
- 기능 시험 대상 클라우드 
	- AWS, GCP, Azure, Alibaba, Tencent, IBM-VPC, Cloudit, OpenStack
	- 그 이외의 국내 클라우드 드라이버 등, 작업 중인 드라이버는 릴리스 및 테스트에 포함하지 않음
- 각 서브시스템의 Pre-release 관련 특이 사항
	- TBA
- 서브시스템 단위의 단계별 작업
	- 단, 선행 서브시스템의 최종 릴리스를 기다리지 말고, pre-release 등을 활용하여 최대한 미리 연동 테스트 진행 필요
- 서브시스템간 통합 테스트가 빨리 처리가 되는 경우, 일정 조정 예정 
- 일정에 영향을 주는 이슈 발생시, 이메일 또는 Slack 채널(sig-release)에 전체 공유

- 문서화 (각 서브시스템에서 작업하여 Pre-release에 포함, 통합 시험 작업과 독립적으로 선행 필요)
	- API 문서 업데이트
	- Readme 업데이트 (Readme는 모든 정보를 v0.7.x으로 미리 지정)
	- 지원 클라우드 정보 업데이트 (스타일 참고: https://github.com/cloud-barista/cb-tumblebug/discussions/429)
	- ChangeLog 작성 (스타일 참고: https://github.com/cloud-barista/cb-tumblebug/blob/master/CHANGELOG.md)

- Pre-release 생성 (Tag: v0.0.x) (스타일 참고: https://github.com/cloud-barista/cb-tumblebug/releases/tag/v0.5.5)
	- 작업 완료 시, 이메일 또는 Slack sig-release 채널에 공지
		- 공유 내용: 프리릴리스 링크, Readme 링크, 프리릴리스 API 문서, 클라우드별 테스트 사항, 이슈 등


### ■ Cloud-Migrator platform v0.1.0 release

Cloud-Migrator platform v0.1.0 release를 위해 각 서브시스템의 (Pre-)release 및 통합 테스트 수행

#### 서브시스템 통합 및 테스트 일정 <--- 협의 필요

| 마감일정       | 대상 저장소                               | 비고          |
| -------------- | ----------------------------------------- | ------------- |
| 2023-11-28(화) | CB-Spider                                 | Pre-requisites |
| 2023-11-30(목) | CB-Tumblebug                              | Pre-requisites |
| 2023-12-04(월) | CM-Honeybee, CM-Damselfly                 |               |
| 2023-12-06(수) | CM-Beetle, CM-Grasshopper, (CM-Centipede) |               |
| 2023-12-08(금) | CM-Cicada, CM-Ant                         |               |
| 2023-12-12(화) | CM-Mayfly                                 |               |
| 2023-12-14(목) | CM-Butterfly                              |               |
| 2023-12-15(금) | Cloud-Migrator                            |               |

#### ■ 개별 서브시스템 릴리스, 통합 소스 구성 및 업로드

- 개별 서브시스템 릴리스 (v0.1.0)
- cloud-barista/cloud-migrator 저장소 소스 업데이트 (Git Submoduel 활용)
- cloud-barista/cloud-migrator 저장소 Readme 및 사용 방법 업데이트 

#### ■ 통합 소스 릴리스

- cloud-barista/cloud-migrator 릴리스 생성 (v0.1.0)
- 릴리스 관련 문서 업데이트
