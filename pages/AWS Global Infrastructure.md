- https://claude.ai/public/artifacts/4c047de4-c21f-416f-9865-4ce52a9d8776
-
- {{cards [[AWS Infrastructure]] }}
-
-
- Q: AWS의 Edge Location이 주로 활용되는 서비스는 무엇인가요? #card
	- CloudFront, Route 53, Global Accelerator 등에서 사용자와 가까운 위치에서 콘텐츠 캐싱 및 DNS 응답을 수행합니다.
- Q: AWS의 Region과 Availability Zone의 관계는? #card
	- A: Region은 물리적으로 분리된 지리적 영역이며, 각 Region은 다수의 Availability Zone(AZ)으로 구성됩니다. AZ는 독립적인 전력, 네트워크 인프라를 가진 데이터 센터입니다.
- Q: AWS의 Local Zone은 어디에 위치하며 어떤 특징이 있나요? #card
	- A: Local Zone은 대도시 근처에 위치한 AWS 인프라로, 지연을 줄이기 위해 Region 밖에서 일부 서비스(EC2, EBS 등)를 제공합니다. Region에 종속됩니다.
- Q: Wavelength Zone의 특징과 사용 목적은? #card
	- A: Wavelength Zone은 통신사 5G 네트워크 내에 구축된 AWS Zone으로, ultra-low latency 애플리케이션(예: AR/VR, 자율주행)을 위해 사용되며 Region에 종속됩니다.
- Q: 사용자가 CloudFront를 통해 정적 콘텐츠를 요청할 때의 흐름은? #card
	- A: 사용자 → 가까운 Edge Location → 캐시 여부 확인 → 없으면 AWS Backbone → Origin 서버 (예: S3 or EC2 in Region)
- Q: 사용자가 EC2에서 운영되는 API 서버를 호출할 때 흐름은? #card
	- A: 사용자 → ISP → AWS Global Backbone → 대상 Region → 해당 AZ (EC2 인스턴스)
- Q: AWS의 Global Backbone Network란 무엇인가요? #card
	- A: 전 세계 Region, AZ, Edge Location, Local Zone 등을 연결하는 AWS의 고속 전용 네트워크입니다.
- Q: Infrastructure Global Services의 예시는 무엇이며 어떤 특징이 있나요? #card
	- A: Route 53, IAM, CloudFront 등이 있으며, 특정 Region에 종속되지 않고 전 세계 어디서든 사용할 수 있습니다.
- Q: Wavelength Zone은 어떤 통신 인프라 위에 위치하나요? #card
	- A: 5G 통신사의 기지국 인프라 내에 위치하여 초저지연 처리를 가능하게 합니다.
- Q: Edge Location과 Local Zone의 가장 큰 차이점은? #card
	- A: Edge Location은 CDN 캐싱 및 DNS 응답을 위한 프록시 노드이고, Local Zone은 일부 AWS 컴퓨팅 리소스를 실제로 운영할 수 있는 인프라입니다.
-
- ## **🌐 AWS Global Infrastructure 구성 요소 정리**
	- |   **구성 요소**   |   **설명**   |
	  | --- | --- |
	  |   **Infrastructure Global Services**   |   Route 53, CloudFront, IAM, S3 global endpoints 등 리전 독립적으로 전 세계에서 사용 가능한 서비스   |
	  |   **Edge Location**   |   사용자에게 가장 가까운 AWS 지점 (CloudFront, Route 53, Global Accelerator 등 사용)   |
	  |   **Local Zone**   |   특정 도시에 있는 AWS 인프라로, 일부 리전 서비스(AWS::EC2, EBS 등)만 지원하며 Region에 종속됨   |
	  |   **Wavelength Zone**   |   통신사 5G 기지국에 위치한 Zone으로 ultra-low latency 앱을 위해 사용됨, Region에 종속   |
	  |   **Region**   |   지리적으로 구분된 AWS 리소스 배포 단위. 일반적으로 여러 Availability Zone 포함   |
	  |   **Availability Zone (AZ)**   |   Region 내의 물리적 데이터 센터. 고가용성과 장애 격리를 위해 구성됨   |
	- ---
- ## **📡 요청 흐름 예시 (서비스 종류에 따라 다름)**
	- |   **요청 유형**   |   **흐름**   |
	  | --- | --- |
	  |   **CloudFront (정적 콘텐츠)**   |   사용자 → Edge Location → 캐시 확인 → (없으면) AWS Backbone → Origin (S3/EC2 in Region)   |
	  |   **Route 53 (DNS)**   |   사용자 → Edge Location → Route 53 응답 → 대상 IP 반환   |
	  |   **EC2 호출 (API 등)**   |   사용자 → ISP → AWS Backbone → Region → AZ (EC2)   |
	  |   **Wavelength Zone 앱 호출**   |   사용자(5G) → 통신망 → Wavelength Zone (AWS Infra)   |
	  |   **Local Zone 사용 앱 호출**   |   사용자 → 가까운 Local Zone → Region으로 백업 전송 필요 시 AWS Backbone 통해 전달   |
-
- # AWS Global Infrastructure 완벽 가이드
	- ## 1. AWS 인프라 계층 구조
		- ### 🌍 전체 구조
			- ```
			  ┌─────────────────────────────────────────────────────────┐
			  │                    AWS Global Network                    │
			  ├─────────────────────────────────────────────────────────┤
			  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐    │
			  │  │   Region    │  │   Region    │  │   Region    │    │
			  │  │  (도쿄)     │  │  (서울)     │  │  (싱가포르) │    │
			  │  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘    │
			  │         │                 │                 │           │
			  │  ┌──────▼──────────────────▼──────────────▼─────┐     │
			  │  │            AWS Backbone Network               │     │
			  │  └───────────────────────────────────────────────┘     │
			  │                                                         │
			  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐    │
			  │  │Edge Location│  │Edge Location│  │Edge Location│    │
			  │  │  (도쿄)     │  │  (서울)     │  │  (부산)     │    │
			  │  └─────────────┘  └─────────────┘  └─────────────┘    │
			  └─────────────────────────────────────────────────────────┘
			  ```
	- ## 2. 각 구성 요소 상세
		- ### 📍 Region (리전)
			- **정의**: 물리적으로 분리된 지리적 영역
			- **현재**: 33개 리전 (2025년 기준)
			- **구성**: 최소 3개 이상의 AZ로 구성
			- **특징**:
			  collapsed:: true
				- 완전히 독립적인 인프라
				- 리전 간 데이터 복제는 명시적으로만 가능
				- 대부분의 서비스는 리전 단위로 제공
		- ### 🏢 Availability Zone (가용 영역)
			- **정의**: 하나 이상의 독립적인 데이터센터 클러스터
			- **구성**:
			  collapsed:: true
				- 각 AZ는 독립된 전력, 냉각, 네트워킹
				- AZ 간 거리: 수십 km (저지연 연결)
				- 물리적으로 분리되어 있지만 고속 전용선으로 연결
		- ### 🏭 Data Center
			- **정의**: 실제 서버가 위치한 물리적 시설
			- **구성**:
			  collapsed:: true
				- 수천~수만 대의 서버
				- 이중화된 전력 및 네트워킹
				- 24/7 보안 및 모니터링
		- ### 🌐 Edge Location / PoP (Points of Presence)
			- **정의**: CDN 및 가속화 서비스를 위한 캐시 서버 위치
			- **현재**: 600+ 엣지 로케이션
			- **서비스**: CloudFront, Route 53, Global Accelerator
			- **특징**: 사용자와 가장 가까운 위치에서 콘텐츠 제공
	- ## 3. 서비스 범위별 분류
		- ### 🌏 Global Services (리전 독립적)
			- ```
			  완전한 글로벌 서비스:
			  - IAM (Identity and Access Management)
			  - Route 53 (DNS)
			  - CloudFront (CDN)
			  - WAF (Web Application Firewall)
			  - Global Accelerator
			  특징:
			  - 단일 엔드포인트
			  - 모든 리전에서 동일하게 작동
			  - 리전 선택 불필요
			  ```
		- ### 🏛️ Regional Services (리전 범위)
			- ```
			  대부분의 AWS 서비스:
			  - EC2, RDS, S3
			  - VPC, Lambda, ECS/EKS
			  - DynamoDB, ElastiCache
			  - SQS, SNS, Kinesis
			  특징:
			  - 리전별로 독립적으로 운영
			  - 리전 간 복제는 명시적 설정 필요
			  - 리소스 ID는 리전별로 고유
			  ```
		- ### 🏗️ AZ-Scoped Resources
			- ```
			  AZ 레벨 리소스:
			  - EC2 인스턴스
			  - EBS 볼륨
			  - RDS 인스턴스 (Multi-AZ 아닌 경우)
			  - 서브넷
			  특징:
			  - 특정 AZ에 고정
			  - AZ 장애 시 영향 받음
			  - 고가용성을 위해 Multi-AZ 구성 필요
			  ```
	- ## 4. 네트워크 통신 및 과금 정책
		- ### 💰 데이터 전송 비용 구조
			- ```
			  ┌─────────────────────────────────────────────────────────┐
			  │                    비용 구조 다이어그램                   │
			  ├─────────────────────────────────────────────────────────┤
			  │                                                         │
			  │  같은 AZ 내부: $0 (무료)                                │
			  │  ┌─────────┐     ┌─────────┐                          │
			  │  │   EC2   │────▶│   RDS   │                          │
			  │  └─────────┘     └─────────┘                          │
			  │                                                         │
			  │  다른 AZ 간: $0.01/GB (각 방향)                        │
			  │  ┌─────────┐     ┌─────────┐                          │
			  │  │  AZ-1a  │────▶│  AZ-1b  │                          │
			  │  └─────────┘     └─────────┘                          │
			  │                                                         │
			  │  다른 Region 간: $0.02/GB                              │
			  │  ┌─────────┐     ┌─────────┐                          │
			  │  │  Seoul  │────▶│  Tokyo  │                          │
			  │  └─────────┘     └─────────┘                          │
			  │                                                         │
			  │  인터넷으로 나가는 트래픽: $0.09/GB                    │
			  │  ┌─────────┐     ┌─────────┐                          │
			  │  │   AWS   │────▶│Internet │                          │
			  │  └─────────┘     └─────────┘                          │
			  └─────────────────────────────────────────────────────────┘
			  ```
		- ### 📊 상세 과금 정책
			- #### 1. **동일 AZ 내부 통신**
			  collapsed:: true
				- ```
				  비용: 무료
				  예시:
				  - EC2 → RDS (같은 AZ)
				  - EC2 → EC2 (같은 AZ)
				  - EC2 → ElastiCache (같은 AZ)
				  권장사항:
				  - 자주 통신하는 리소스는 같은 AZ에 배치
				  - 단, 고가용성 고려 필요
				  ```
			- #### 2. **Cross-AZ 통신**
			  collapsed:: true
				- ```
				  비용: $0.01/GB (양방향)
				  예시:
				  - EC2(AZ-1a) → RDS(AZ-1b)
				  - ELB가 다른 AZ의 인스턴스로 트래픽 분산
				  권장사항:
				  - Multi-AZ 구성 시 비용 고려
				  - 고가용성 vs 비용 트레이드오프
				  ```
			- #### 3. **Cross-Region 통신**
			  collapsed:: true
				- ```
				  비용: $0.02/GB (송신 리전 기준)
				  예시:
				  - S3(Seoul) → EC2(Tokyo)
				  - RDS Read Replica 복제
				  권장사항:
				  - 꼭 필요한 경우만 Cross-Region 구성
				  - S3 Cross-Region Replication 비용 주의
				  ```
			- #### 4. **인터넷 아웃바운드**
			  collapsed:: true
				- ```
				  비용: 
				  - 첫 10TB/월: $0.09/GB
				  - 다음 40TB/월: $0.085/GB
				  - 그 이상: 협상 가능
				  무료:
				  - 인터넷에서 AWS로 들어오는 트래픽
				  - CloudFront에서 Origin으로의 트래픽
				  ```
	- ## 5. 아키텍처 패턴별 통신 흐름
		- ### 🏗️ Single Region, Multi-AZ 패턴
			- ```
			  ┌─────────────────── Region: ap-northeast-2 ──────────────────┐
			  │                                                              │
			  │  ┌────────── AZ-2a ──────────┐  ┌────────── AZ-2b ────────┐│
			  │  │                           │  │                          ││
			  │  │  ┌─────┐    ┌─────┐      │  │  ┌─────┐    ┌─────┐    ││
			  │  │  │ EC2 │───▶│ RDS │      │  │  │ EC2 │───▶│ RDS │    ││
			  │  │  └─────┘    │Master│      │  │  └─────┘    │Standby│  ││
			  │  │             └─────┘       │  │             └─────┘     ││
			  │  │                 │         │  │                 ▲       ││
			  │  └─────────────────┼─────────┘  └─────────────────┼───────┘│
			  │                    │                               │        │
			  │                    └───────── Sync Replication ────┘        │
			  │                           ($0.01/GB each way)               │
			  └──────────────────────────────────────────────────────────────┘
			  ```
		- ### 🌏 Multi-Region 패턴
			- ```
			  ┌──── Region: Seoul ────┐         ┌──── Region: Tokyo ────┐
			  │                       │         │                       │
			  │  ┌─────────────────┐  │         │  ┌─────────────────┐ │
			  │  │   Primary RDS   │  │         │  │  Read Replica   │ │
			  │  └────────┬────────┘  │         │  └────────▲────────┘ │
			  │           │           │         │           │          │
			  │  ┌────────▼────────┐  │         │  ┌────────┴────────┐ │
			  │  │   Application   │◄─┼─────────┼─▶│   Application   │ │
			  │  └─────────────────┘  │         │  └─────────────────┘ │
			  └───────────────────────┘         └───────────────────────┘
			          Cross-Region Replication: $0.02/GB
			  ```
	- ## 6. 비용 최적화 전략
		- ### 💡 아키텍처 설계 원칙
			- collapsed:: true
			  1.  **같은 AZ 우선 배치**
				- ```
				      적합한 경우:
				      - 개발/테스트 환경
				      - 비용 민감한 워크로드
				      - 짧은 다운타임 허용 가능
				    ```
			- collapsed:: true
			  2.  **선택적 Multi-AZ**
				- ```
				      Multi-AZ 필수:
				      - 프로덕션 데이터베이스
				      - 중요 비즈니스 애플리케이션
				      Single-AZ 고려:
				      - 개발 환경
				      - 배치 처리 시스템
				      - 재생성 가능한 워크로드
				    ```
			- collapsed:: true
			  3.  **VPC Endpoint 활용**
				- ```
				      S3/DynamoDB VPC Endpoint:
				      - 인터넷 게이트웨이 우회
				      - 데이터 전송 비용 절감
				      - 보안 향상
				    ```
			- collapsed:: true
			  4.  **CDN 활용**
				- ```
				      CloudFront 사용:
				      - 정적 콘텐츠는 엣지에서 제공
				      - Origin 트래픽 감소
				      - 전체 비용 절감
				    ```
	- ## 7. 실전 예시: 전자상거래 사이트
		- ### 🛒 비용 효율적인 아키텍처
			- ```
			  구성:
			  - Web Servers: 3개 AZ에 분산 (고가용성)
			  - Database: RDS Multi-AZ (필수)
			  - Cache: ElastiCache - Web Server와 같은 AZ (비용 절감)
			  - Static Assets: S3 + CloudFront (글로벌 배포)
			  월간 데이터 전송 예상:
			  - Same AZ (Cache): 1TB × $0 = $0
			  - Cross AZ (DB): 100GB × $0.01 × 2 = $2
			  - Internet Out: 500GB × $0.09 = $45
			  - 총 전송 비용: ~$47/월
			  ```
	- ## 8. 주요 체크리스트
		- ### ✅ 설계 시 고려사항
			- [ ] 고가용성 요구사항 정의
			- [ ] 데이터 전송 패턴 분석
			- [ ] 비용 vs 가용성 트레이드오프
			- [ ] 규정 준수 (데이터 레지던시)
			- [ ] 재해 복구 요구사항
			- [ ] 글로벌 사용자 분포
		- ### 📈 모니터링 포인트
			- VPC Flow Logs로 트래픽 패턴 분석
			- Cost Explorer로 데이터 전송 비용 추적
			- CloudWatch로 Cross-AZ 트래픽 모니터링
			- AWS Cost Anomaly Detection 활성화
-