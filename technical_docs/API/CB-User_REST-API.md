# Cloud-Barista User REST API 
[v0.3.0-espresso](#v0.3.0-espresso) <BR>
[v0.2.0-cappuccino](#v0.2.0-cappuccino) <BR>
[v0.1.0-americano](#v0.1.0-americano)

# v0.3.0-espresso
### [클라우드 인프라 연동 정보 통합 관리 (CB-Spider)] 
  * 클라우드 인프라 연결을 위한 정보 등록 및 관리
  * API 규격/샘플 : https://cloud-barista.github.io/rest-api/v0.3.0/spider/ccim/
    * 관리대상: Cloud Driver/Credential/Region:Zone

  * 클라우드 인프라 공통 제어 관리
  * API 규격/샘플 : https://cloud-barista.github.io/rest-api/v0.3.0/spider/cctm/
    * 제어대상: Image/Spec/VPC/Subnet/SecurityGroup/KeyPair/VM

  * Leader: [@powerkimhub](https://github.com/powerkimhub "https://github.com/powerkimhub")

---

### [멀티 클라우드 인프라 서비스 통합 관리 (CB-Tumblebug)]

#### [멀티 클라우드 네임스페이스 관리]
  * 작업 공간 분리를 위한 네임스페이스 생성 및 관리
  * API 규격/샘플 : [v0.3.0 Namespce API](https://cloud-barista.github.io/cb-tumblebug-api-web/?url=https://raw.githubusercontent.com/cloud-barista/cb-tumblebug/master/src/docs/tbapi-0.3.0.yaml#/Namespace)
  * Leader: [@seokho-son](https://github.com/seokho-son "https://github.com/seokho-son") [@jihoon-seo](https://github.com/jihoon-seo "https://github.com/jihoon-seo")
 
#### [멀티 클라우드 인프라 자원(MCIR) 관리]
  * MCIR 관련 등록/생성 및 관리
  * API 규격/샘플
    * Image : [v0.3.0 MCIR API-Image](https://cloud-barista.github.io/cb-tumblebug-api-web/?url=https://raw.githubusercontent.com/cloud-barista/cb-tumblebug/master/src/docs/tbapi-0.3.0.yaml#/Image)
    *	Spec : [v0.3.0 MCIR API-Spec](https://cloud-barista.github.io/cb-tumblebug-api-web/?url=https://raw.githubusercontent.com/cloud-barista/cb-tumblebug/master/src/docs/tbapi-0.3.0.yaml#/Spec)
    *	VNet : [v0.3.0 MCIR API-VNet](https://cloud-barista.github.io/cb-tumblebug-api-web/?url=https://raw.githubusercontent.com/cloud-barista/cb-tumblebug/master/src/docs/tbapi-0.3.0.yaml#/VNet)
    *	SecurityGroup : [v0.3.0 MCIR API-SecurityGroup](https://cloud-barista.github.io/cb-tumblebug-api-web/?url=https://raw.githubusercontent.com/cloud-barista/cb-tumblebug/master/src/docs/tbapi-0.3.0.yaml#/Security%20Group)
    *	AccessKey : [v0.3.0 MCIR API-AccessKey](https://cloud-barista.github.io/cb-tumblebug-api-web/?url=https://raw.githubusercontent.com/cloud-barista/cb-tumblebug/master/src/docs/tbapi-0.3.0.yaml#/SSH%20Key)
  * Leader: [@jihoon-seo](https://github.com/jihoon-seo "https://github.com/jihoon-seo") [@seokho-son](https://github.com/seokho-son "https://github.com/seokho-son")
 
#### [멀티 클라우드 인프라 서비스(MCIS) 관리]
  * MCIS 생성/제어 및 관리
  * API 규격/샘플
    * 생성 및 기본 제어(1) : [v0.3.0 MCIS API(1)](https://cloud-barista.github.io/cb-tumblebug-api-web/?url=https://raw.githubusercontent.com/cloud-barista/cb-tumblebug/master/src/docs/tbapi-0.3.0.yaml#/MCIS)
    * 생성 및 기본 제어(2) : [v0.3.0 MCIS API(2)](https://cloud-barista.github.io/cb-tumblebug-api-web/?url=https://raw.githubusercontent.com/cloud-barista/cb-tumblebug/master/src/docs/tbapi-0.3.0.yaml#/MCIS%20Policy)

  * Leader: [@seokho-son](https://github.com/seokho-son "https://github.com/seokho-son") [@jihoon-seo](https://github.com/jihoon-seo "https://github.com/jihoon-seo")

---
 
### [멀티 클라우드 애플리케이션 운용 관리 (CB-Ladybug)]
  * 클러스터 생성/삭제/정보 조회, 노드 추가, 삭제, 정보 조회
  * API 규격/샘플 : https://cloud-barista.github.io/rest-api/v0.3.0/ladybug/
  * Leader: [@sykim-etri](https://github.com/sykim-etri "https://github.com/sykim-etri")
   
---
 
### [멀티 클라우드 인프라 서비스 통합 모니터링 (CB-Dragonfly)]
  * MCIS 및 VM 모니터링(온디멘드 모니터링 조회, MCIS 모니터링 조회, 알람 이벤트 핸들러 관리 및 알람 관리)
  * API 규격/샘플 : https://cloud-barista.github.io/rest-api/v0.3.0/dragonfly/
  * Leader: [@kyongminkwon](https://github.com/kyongminkwon "https://github.com/kyongminkwon") [@hyokyungk](https://github.com/hyokyungk "https://github.com/hyokyungk")

<P>
<BR>

# v0.2.0-cappuccino

### [클라우드 인프라 연동 정보 통합 관리 (CB-Spider)] 
  * 클라우드 인프라 연결을 위한 정보 등록 및 관리
  * API 규격/샘플 : https://cloud-barista.github.io/rest-api/v0.2.0/spider/ccim/
  * Leader: [@powerkimhub](https://github.com/powerkimhub "https://github.com/powerkimhub")

---

### [멀티 클라우드 인프라 서비스 통합 관리 (CB-Tumblebug)]

#### [멀티 클라우드 네임스페이스 관리]
  * 작업 공간 분리를 위한 네임스페이스 생성 및 관리
  * API 규격/샘플 : https://cloud-barista.github.io/rest-api/v0.2.0/tumblebug/namespace/
  * Leader: [@seokho-son](https://github.com/seokho-son "https://github.com/seokho-son") [@jihoon-seo](https://github.com/jihoon-seo "https://github.com/jihoon-seo")
 
#### [멀티 클라우드 인프라 자원(MCIR) 관리]
  * MCIR 관련 등록/생성 및 관리
  * API 규격/샘플 : https://cloud-barista.github.io/rest-api/v0.2.0/tumblebug/mcir/
  * Leader: [@jihoon-seo](https://github.com/jihoon-seo "https://github.com/jihoon-seo") [@seokho-son](https://github.com/seokho-son "https://github.com/seokho-son")
 
#### [멀티 클라우드 인프라 서비스(MCIS) 관리]
  * MCIS 생성/제어 및 관리
  * API 규격/샘플 : https://cloud-barista.github.io/rest-api/v0.2.0/tumblebug/mcis/
  * Leader: [@seokho-son](https://github.com/seokho-son "https://github.com/seokho-son") [@jihoon-seo](https://github.com/jihoon-seo "https://github.com/jihoon-seo")

---
 
### [멀티 클라우드 인프라 서비스 통합 모니터링 (CB-Dragonfly)]
  * MCIS 및 VM 모니터링(모니터링 조회 시 UTC 시간 기준으로 time 값 전달)
  * API 규격/샘플 : https://cloud-barista.github.io/rest-api/v0.2.0/dragonfly/
  * Leader: [@kyongminkwon](https://github.com/kyongminkwon "https://github.com/kyongminkwon") [@hyokyungk](https://github.com/hyokyungk "https://github.com/hyokyungk")


<P>
<BR>

# v0.1.0-americano
### [클라우드 인프라 연동 정보 통합 관리 (CB-Spider)] 
  * 클라우드 인프라 연결을 위한 정보 등록 및 관리
  * API 규격/샘플 : https://cloud-barista.github.io/rest-api/v0.1.0/spider/ccim/
  * Leader: [@powerkimhub](https://github.com/powerkimhub "https://github.com/powerkimhub")

---

### [멀티 클라우드 인프라 서비스 통합 관리 (CB-Tumblebug)]

#### [멀티 클라우드 네임스페이스 관리]
  * 작업 공간 분리를 위한 네임스페이스 생성 및 관리
  * API 규격/샘플 : https://cloud-barista.github.io/rest-api/v0.1.0/tumblebug/namespace/
  * Leader: [@seokho-son](https://github.com/seokho-son "https://github.com/seokho-son") [@jihoon-seo](https://github.com/jihoon-seo "https://github.com/jihoon-seo")
 
#### [멀티 클라우드 인프라 자원(MCIR) 관리]
  * MCIR관련 등록/생성 및 관리
  * API 규격/샘플 : https://cloud-barista.github.io/rest-api/v0.1.0/tumblebug/mcir/
  * Leader: [@jihoon-seo](https://github.com/jihoon-seo "https://github.com/jihoon-seo") [@seokho-son](https://github.com/seokho-son "https://github.com/seokho-son")
 
#### [멀티 클라우드 인프라 서비스(MCIS) 관리]
  * MCIS 생성/제어 및 관리
  * API 규격/샘플 : https://cloud-barista.github.io/rest-api/v0.1.0/tumblebug/mcis/
  * Leader: [@seokho-son](https://github.com/seokho-son "https://github.com/seokho-son") [@jihoon-seo](https://github.com/jihoon-seo "https://github.com/jihoon-seo")
 
#### [멀티 클라우드 인프라 서비스 특화 기능]
  * 동일 Subnet & Load Balancing 기능
  * API 규격/샘플 : https://cloud-barista.github.io/rest-api/v0.1.0/tumblebug/special-svc/
  * Leader: [@innodreamer](https://github.com/innodreamer "https://github.com/innodreamer")
 
---
 
### [멀티 클라우드 인프라 서비스 통합 모니터링 (CB-Dragonfly)]
  * MCIS 및 VM 모니터링(모니터링 조회 시 UTC 시간 기준으로 time 값 전달)
    * API 규격/샘플 : https://cloud-barista.github.io/rest-api/v0.1.0/dragonfly/
  * Leader: [@kyongminkwon](https://github.com/kyongminkwon "https://github.com/kyongminkwon") [@hyokyungk](https://github.com/hyokyungk "https://github.com/hyokyungk")
  
  