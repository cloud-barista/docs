# **멀티 클라우드 서비스 공통 플랫폼 사용자 서비스 및 기능 개요**

멀티 클라우드 서비스 공통 플랫폼(이하 공통 플랫폼)은 사용자에게 멀티 클라우드 네임스페이스, 멀티 클라우드 인프라 자원과 멀티 클라우드 인프라 서비스 및 멀티 클라우드 애플리케이션 실행환경 등을 제공하며, 세부 내용은 다음과 같다(그림 1 참조):

- <ins>멀티 클라우드 네임스페이스(Multi Cloud NameSpace, MCNS)</ins>   
공통 플랫폼은 사용자가 운영 목적에 따라 독립된 자원 운용 및 독자적인 네이밍이 가능한 멀티 클라우드 인프라 자원 및 서비스 그룹을 운영할 수 있도록, 사용자에게 멀티 클라우드 네임스페이스를 제공한다. 사용자는 멀티 클라우드 네임스페이스를 생성한 후, 생성한 네임스페이스 내에서 운영 목적에 맞는 멀티 클라우드 인프라 자원과 멀티 클라우드 인프라 서비스 및 멀티 클라우드 애플리케이션 실행환경 등을 생성 및 관리할 수 있다.

- <ins>멀티 클라우드 인프라 자원(Multi Cloud Infra Resource, MCIR)</ins>   
멀티 클라우드 인프라 자원은 사용자가 멀티 클라우드 인프라 서비스를 생성 및 운용하기 위하여 여러 클라우드 인프라로부터 제공받는 컴퓨팅 인프라 자원들을 말한다. 사용자는 공통 플랫폼 사용자 인터페이스를 통해 멀티 클라우드 인프라 자원을 등록, 생성 및 관리할 수 있다. 주요 멀티 클라우드 인프라 자원은 다음과 같다.

   - VM 이미지: VM의 이미지 정보를 명시하는 자원
   - VM 사양(spec): VM의 사양 정보를 명시하는 자원
   - 가상 네트워크: VM이 포함되는 가상의 네트워크 자원
   - 보안 그룹(Security Group): VM 접속 방화벽 규칙을 지정하는 보안 그룹 자원
   - VM 키페어(Key Pair): VM 접속에 필요한 키페어


- <ins>멀티 클라우드 인프라 서비스(Multi Cloud Infra Service, MCIS)</ins>   
멀티 클라우드 인프라 서비스는 공통 플랫폼에서 관리되는 멀티 클라우드 인프라 자원을 기반으로 생성된 하나 이상의 VM으로 구성되어 VM 간 상호 통신이 가능한 VM 클러스터를 말한다. 사용자는 공통 플랫폼 사용자 인터페이스를 통해 멀티 클라우드 인프라 서비스를 생성할 수 있으며 멀티 클라우드 인프라 서비스 단위 및 개별 단위의 실행 제어 및 모니터링을 할 수 있다.

- <ins>멀티 클라우드 애플리케이션 실행환경(Multi Cloud Application Runtime, MCAR)</ins>   
멀티 클라우드 애플리케이션은 멀티 클라우드 환경에서 실행 가능한 사용자의 응용을 말하며, 멀티 클라우드 서비스 공통 플랫폼은 사용자에게 멀티 클라우드 애플리케이션을 실행할 수 있는 실행환경(Runtime)을 제공한다. 사용자는 공통 프레임워크 사용자 인터페이스를 통해 멀티 클라우드 애플리케이션의 실행을 요청할 수 있으며 실행 중인 멀티 클라우드 애플리케이션의 제어 및 모니터링을 할 수 있다.


멀티 클라우드 서비스 공통 프레임워크의 서비스 대상 및 멀티 클라우드 인프라 서비스 생성 주요 흐름과 멀티 클라우드 애플리케이션 실행 흐름이 그림 1과 같다 (세부 흐름은 그림 내 번호 참조).

![image](https://user-images.githubusercontent.com/7975459/96402508-115d4000-1211-11eb-80a9-53f0b1d006f1.png)

<p align="center">
  <strong>그림 1</strong> 멀티 클라우드 서비스 공통 플랫폼 서비스 대상 및 연관 관계
</p>


# **멀티 클라우드 서비스 공통 플랫폼 사용자 기능**

멀티 클라우드 서비스 공통 플랫폼 사용자에게 제공하는 사용자 기능은 다음과 같다:

<table>
<thead>
  <tr class="header">
    <th><strong>대분류</strong></th>
    <th><strong>중분류</strong></th>
    <th><strong>사용자 기능</strong></th>
  </tr>
</thead>
<tbody>
  <!------------ 멀티 클라우드 인프라 연동 시작 ------------>
  <tr>
    <td rowspan="12">
      <p><strong>멀티 클라우드</strong></p>
      <p><strong>인프라 연동</strong></p>
    </td>
    <td rowspan="3"><p>클라우드 드라이버 관리</p></td>
    <td><p>클라우드 드라이버 정보 등록</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>클라우드 드라이버 정보 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>클라우드 드라이버 정보 삭제</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <td rowspan="3"><p>클라우드 연결 정보 관리</p></td>
    <td><p>클라우드 인프라 연결 정보 등록</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>클라우드 인프라 연결 정보 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>클라우드 인프라 연결 정보 삭제</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <td rowspan="3"><p>클라우드 리젼 정보 관리</p></td>
    <td><p>클라우드 리젼 정보 등록</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>클라우드 리젼 정보 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>클라우드 리젼 정보 삭제</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <td rowspan="3"><p>클라우드 연결 설정 관리</p></td>
    <td><p>클라우드 연결 설정 생성</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>클라우드 연결 설정 정보 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>클라우드 연결 설정 삭제</p></td>
  </tr>  
  <!------------ 멀티 클라우드 인프라 연동 끝 ------------>
  
  <!------------ 멀티 클라우드 네임 스페이스 시작 ------------>
  <tr>
    <td rowspan="3">
      <p><strong>멀티 클라우드</strong></p>
      <p><strong>네임스페이스</strong></p>
    </td>
    <td rowspan="3"><p>멀티 클라우드 네임스페이스 관리</p></td>
    <td><p>멀티 클라우드 네임스페이스 생성</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 네임스페이스 정보 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td>
      <p>멀티 클라우드 네임스페이스 삭제</p></td>
  </tr>
  <!------------ 멀티 클라우드 네임 스페이스  ------------>
  
  <!------------ 멀티 클라우드 인프라 서비스 시작 ------------>
  <tr>
    <td rowspan="37">
      <p><strong>멀티 클라우드</strong></p>
      <p><strong>인프라 서비스</strong></p>
    </td>
    <td rowspan="15">
      <p>멀티 클라우드 인프라 자원 관리</p>
      <p>(VM 이미지 자원, VM 사양 자원,</p>
      <p>가상 네트워크 자원, 보안 그룹 자원,</p>
      <p>VM 키페어 자원 등)</p>
    </td>
    <td><p>VM 이미지 자원 등록</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>VM 이미지 자원 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>VM 이미지 자원 등록 해제</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>가상 네트워크 자원 생성</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>가상 네트워크 자원 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>가상 네트워크 자원 삭제</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>보안 그룹 자원 생성</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>보안 그룹 자원 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>보안 그룹 자원 삭제</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>VM 키페어 자원 생성</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>VM 키페어 자원 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>VM 키페어 자원 삭제</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>VM 사양 정보 등록</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>VM 사양 정보 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>VM 사양 정보 등록 해제</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <td rowspan="3">
      <p>멀티 클라우드 인프라 서비스</p>
      <p>정보 통합 관리</p>
    </td>
    <td><p>멀티 클라우드 인프라 서비스 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 삭제</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 내부 VM 삭제</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <td rowspan="5">
      <p>멀티 클라우드 인프라 서비스</p>
      <p>생성 관리</p>
    </td>
    <td><p>멀티 클라우드 인프라 서비스 생성</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 최적 배치 스케줄링 정책 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스의 최적 배치 스케줄링 기반 생성</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 확장</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스의 최적 배치 스케줄링 기반 확장</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <td rowspan="2">
      <p>멀티 클라우드 인프라 서비스</p>
      <p>라이프사이클 상태 정보 관리</p>
    </td>
    <td><p>멀티 클라우드 인프라 서비스 라이프사이클 상태 정보 통합 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 라이프사이클 상태 정보 개별 VM 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <td rowspan="8">
      <p>멀티 클라우드 인프라 서비스</p>
      <p>라이프사이클 제어</p>
    </td>
    <td><p>멀티 클라우드 인프라 서비스 통합 중지</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 통합 재개</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 통합 재시작</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 통합 종료</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 개별 VM 중지</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 개별 VM 재개</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 개별 VM 재시작</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 개별 VM 종료</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <td rowspan="4">
      <p>정책 기반 멀티 클라우드 인프라 서비스</p>
      <p>품질 자동 제어</p>
    </td>
    <td><p>멀티 클라우드 인프라 서비스 품질 자동 제어 정책 등록</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 품질 자동 제어 정책 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 품질 자동 제어 이벤트 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 성능 품질 진단</p></td>
  </tr>
  <!------------ 멀티 클라우드 인프라 서비스 끝 ------------>
  
  <!------------ 멀티 클라우드 통합 모니터링 시작 ------------>
  <tr>
    <td rowspan="15">
      <p><strong>멀티 클라우드</strong></p>
      <p><strong>통합 모니터링</strong></p>
    </td>
    <td rowspan="4">
      <p>멀티 클라우드 인프라 서비스</p>
      <p>모니터링 정보 제공</p>
    </td>
    <td><p>멀티 클라우드 인프라 서비스 주기적 모니터링 정보 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 특정 VM 주기적 모니터링 정보 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 온디맨드 모니터링 정보 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 특정 VM 온디맨드 모니터링 정보 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <td rowspan="3">
      <p>멀티 클라우드 인프라 서비스</p>
      <p>모니터링 정책 관리</p>
    </td>
    <td><p>멀티 클라우드 인프라 서비스 모니터링 정책 설정</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 모니터링 정책 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 모니터링 정책 초기화</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <td rowspan="7">
      <p>멀티 클라우드 인프라</p>
      <p>서비스 알람 임계치 관리</p>
    </td>
    <td><p>멀티 클라우드 인프라 서비스 상태 임계치 설정</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 상태 임계치 설정 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 상태 임계치 설정 해제</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>알람 이벤트 핸들러 추가</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>알람 이벤트 핸들러 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>알람 이벤트 핸들러 삭제</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 서비스 알람 로그 정보 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <td>
      <p>멀티 클라우드 애플리케이션</p>
      <p>모니터링 정보 제공</p>
    </td>
    <td><p>TBD</p></td>
  </tr>
  <!------------ 멀티 클라우드 통합 모니터링 끝 ------------>
  
  <!------------ 멀티 클라우드 애플리케이션 서비스 시작 ------------>
  <tr>
    <td rowspan="26">
      <p><strong>멀티 클라우드</strong></p>
      <p><strong>애플리케이션 서비스</storng></p>
    </td>
    <td rowspan="4">
      <p>멀티 클라우드 애플리케이션</p>
      <p>구성 정보 관리</p>
    </td>
    <td><p>멀티 클라우드 애플리케이션 구성 정보 등록</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 애플리케이션 구성 정보 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 애플리케이션 구성 정보 변경</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 애플리케이션 구성 정보 삭제</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <td rowspan="4">
      <p>멀티 클라우드 애플리케이션</p>
      <p>실행 요구사항 관리</p>
    </td>
    <td><p>멀티 클라우드 애플리케이션 실행 요구사항 등록</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 애플리케이션 실행 요구사항 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 애플리케이션 실행 요구사항 변경</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 애플리케이션 실행 요구사항 삭제</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <td rowspan="3">
      <p>멀티 클라우드 애플리케이션</p>
      <p>라이프사이클 제어</p>
    </td>
    <td><p>멀티 클라우드 애플리케이션 실행</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 애플리케이션 재시작</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 애플리케이션 종료</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <td rowspan="2">
      <p>멀티 클라우드 애플리케이션</p>
      <p>라이프사이클 상태 정보 조회</p>
    </td>
    <td><p>멀티 클라우드 애플리케이션 라이프사이클 상태 정보 통합 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 애플리케이션의 단위 애플리케이션 상태 정보 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <td rowspan="3">
      <p>멀티 클라우드 애플리케이션</p>
      <p>품질 자동 제어 정책 관리</p>
    </td>
    <td><p>멀티 클라우드 애플리케이션 품질 자동 제어 정책 등록</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 애플리케이션 품질 자동 제어 정책 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 애플리케이션 품질 자동 제어 이벤트 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <td>
      <p>멀티 클라우드 애플리케이션 클라우드간</p>
      <p>연계 운영 관리</p>
    </td>
    <td><p>TBD</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <td rowspan="4">
      <p>멀티 클라우드 애플리케이션</p>
      <p>실행환경 템플릿 관리</p>
    </td>
    <td><p>멀티 클라우드 애플리케이션 실행환경 템플릿 생성</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 애플리케이션 실행환경 템플릿 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 애플리케이션 실행환경 템플릿 변경</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 애플리케이션 실행환경 템플릿 삭제</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <td rowspan="5">
      <p>멀티 클라우드 애플리케이션</p>
      <p>실행환경 관리</p>
    </td>
    <td><p>멀티 클라우드 애플리케이션 실행환경 생성</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 애플리케이션 실행환경 정보 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 애플리케이션 실행환경 변경</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 애플리케이션 실행환경 업그레이드</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 애플리케이션 실행환경 삭제</p></td>
  </tr>
  <!------------ 멀티 클라우드 애플리케이션 서비스 끝 ------------>
  
  <!------------ 멀티 클라우드 인프라 연동 이력 시작 ------------>
  <tr>
    <td rowspan="4">
      <p><strong>멀티 클라우드<strong></p>
      <p><strong>인프라 연동 이력</strong></p>
    </td>
    <td rowspan="4">
      <p>멀티 클라우드 인프라 연동 이력 정보 관리</p>
      <p>&#8251; 연동 이력: 클라우드 연결 및 API 호출 이력</p>
    </td>
    <td><p>멀티 클라우드 인프라 연동 이력 수집 여부 설정</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 연동 이력 정보 조회</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 연동 이력 정보 가시화 도구 제공</p></td>
  </tr>
  <tr>
    <!-- merged td -->
    <!-- merged td -->
    <td><p>멀티 클라우드 인프라 연동 이력 정보 삭제</p></td>
  </tr>
  <!------------ 멀티 클라우드 인프라 연동 이력  ------------>
</tbody>
</table>
