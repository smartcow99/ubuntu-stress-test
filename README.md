# 🌟 Spring Boot 애플리케이션 부하 테스트 가이드 🌟

이 가이드는 **Ubuntu 환경**에서 **Spring Boot 애플리케이션**에 대한 부하 테스트를 수행하는 방법을 설명합니다. 

부하 테스트는 애플리케이션의 성능과 안정성을 평가하는 중요한 과정입니다. 

아래의 도구들을 사용해 효율적으로 테스트해보세요! 🚀

## 🔧 준비 사항

- Ubuntu 운영체제
- Spring Boot 애플리케이션 실행 중
- ApacheBench (ab) 설치
- (선택) stress-ng 설치

## 🏗️ Spring Boot 애플리케이션 정보

### 📦 애플리케이션 구성

- **Framework**: Spring Boot
- **Version**: 2.7.x
- **Database**: MySQL
- **API Endpoint**: `/api/v1/resource`
- **Port**: 8080
- **Dependency Management**: Maven

애플리케이션은 간단한 REST API를 제공합니다. 데이터베이스는 H2 (in-memory)를 사용하거나 MySQL과 연동할 수 있으며, 기본적으로 포트 `8080`에서 동작합니다.

### 📂 프로젝트 구조
```text
spring-boot-load-test/
├── src/
│   ├── main/
│   │   ├── java/com/example/demo/
│   │   │   ├── DemoApplication.java
│   │   │   ├── controller/
│   │   │   │   └── ResourceController.java
│   │   │   └── service/
│   │   │       └── ResourceService.java
│   │   └── resources/
│   │       └── application.properties
│   └── test/
│       └── java/com/example/demo/
│           └── DemoApplicationTests.java
├── build.gradle
└── README.md

```

---

### 1. 💻 ApacheBench (ab) 설치하기

`ApacheBench (ab)`는 웹 애플리케이션에 부하를 주기 위한 툴입니다. 

다음 명령어로 설치하세요:
```bash
sudo apt-get update
sudo apt-get install apache2-utils
```

### 2. 🚀 부하 테스트 실행하기

ApacheBench를 사용해 Spring Boot 애플리케이션에 부하를 걸어봅니다. 

다음 명령어를 사용하세요:
```bash
ab -n 1000 -c 50 http://localhost:8080/
```

- `-n 1000`: 총 1000번의 요청을 보냅니다.
- `-c 50`: 50개의 동시 요청을 보냅니다.


#### 📊 예시 결과:
```text
Server Software:        Apache-Coyote/1.1
Server Hostname:        localhost
Server Port:            8080
Document Path:          /
Document Length:        183 bytes
Concurrency Level:      50
Time taken for tests:   2.215 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      254000 bytes
HTML transferred:       183000 bytes
Requests per second:    451.44 [#/sec] (mean)
Time per request:       110.737 [ms] (mean)
```


### 3. 🧠 시스템 리소스 스트레스 테스트 (선택)

| 부하 테스트 도중 시스템 리소스에 추가적인 스트레스를 주고 싶다면 stress-ng 도구를 사용해보세요.

**stress-ng** 설치하기:

```bash
sudo apt-get install stress-ng
```

**CPU 및 메모리 스트레스 테스트 예시:**

```bash
stress-ng --cpu 4 --vm 2 --vm-bytes 512M --timeout 60s
```

- `--cpu 4`: CPU 코어 4개를 사용합니다.
- `--vm 2`: 가상 메모리 테스트 2개를 실행합니다.
- `--vm-bytes 512M`: 각 가상 메모리에 512MB의 메모리를 할당합니다.
- `--timeout 60s`: 60초 동안 부하를 줍니다.

  
### 4. 🔍 결과 분석하기

부하 테스트 결과를 분석해 애플리케이션 성능을 평가하세요. 

주요 지표는 다음과 같습니다:

- **초당 요청 수 (Requests per second)**: 초당 애플리케이션이 처리할 수 있는 요청 수
- **실패한 요청 수 (Failed requests)**: 부하 중 실패한 요청이 있는지 확인
- **요청당 평균 시간 (Time per request)**: 각 요청이 완료되는 데 걸리는 평균 시간

### 5. 📈 추가 테스트 시나리오

Spring Boot 애플리케이션에 대한 다양한 추가 테스트 시나리오도 진행해 보세요!

#### 5.1. 🔄 특정 API 엔드포인트 부하 테스트

특정 API 엔드포인트에 대해 부하를 주고 싶다면, 다음과 같이 URL을 지정하세요:

```bash
ab -n 1000 -c 50 http://localhost:8080/api/v1/resource
```

#### 5.2. 💾 디스크 I/O 스트레스 테스트

애플리케이션의 로그 또는 데이터베이스가 저장된 디스크에 부하를 주는 방법:

```bash
dd if=/dev/zero of=/tmp/testfile bs=1G count=10 oflag=direct
```

이 명령어는 10GB의 임시 파일을 생성해 디스크 I/O 부하를 테스트합니다.

#### 5.3. 🏷️ 장기 부하 테스트

단기 테스트 외에도, 몇 시간에서 며칠 동안 지속적인 부하를 가해 메모리 누수 또는 성능 저하를 확인하세요.

#### 5.4. 🔥 최대 부하 임계치 테스트

부하를 점진적으로 늘려 애플리케이션의 처리 한계를 알아보세요. 

점차적으로 -n 및 -c 값을 증가시키며 테스트합니다.

#### 5.5 🛠️ 비동기 요청 부하 테스트

비동기적으로 작동하는 API를 가진 애플리케이션의 경우, 비동기 요청이 쌓였을 때 성능을 테스트할 필요가 있습니다. 
이를 위해 ab의 동시 요청 수를 극대화하여 테스트할 수 있습니다.

```bash
ab -n 1000 -c 100 http://localhost:8080/api/v1/resource/async
```

비동기 API 특성을 고려해 동시 요청 수(-c)를 크게 늘리고 테스트해봅니다.

#### 5.6 📅 스케줄링 작업 부하 테스트

Spring Boot 애플리케이션은 스케줄링 작업(Cron jobs)을 사용할 수 있습니다. 
스케줄링된 작업이 실행되는 동안 부하가 걸릴 때 애플리케이션이 어떻게 반응하는지 확인할 수 있습니다.

애플리케이션이 5초마다 스케줄링 작업을 수행하는 경우, 스케줄링 중 부하를 줄 수 있는 방법을 시뮬레이션하세요:

```bash
ab -n 5000 -c 200 http://localhost:8080/api/v1/resource
```

#### 5.7 🔄 무한 루프 또는 메모리 누수 테스트
애플리케이션이 무한 루프에 빠지거나 메모리 누수가 발생할 가능성을 탐지하기 위해, 장시간 부하 테스트를 진행합니다. 아래 명령어로 몇 시간 동안 지속적으로 부하를 걸어 성능을 모니터링합니다.

```bash
ab -n 100000 -c 100 http://localhost:8080/api/v1/resource
```
- `-n 100000`: 10만 개의 요청을 연속적으로 보냅니다.
- `-c 100`: 동시 요청 수를 100으로 유지합니다.

#### 5.8 🌐 네트워크 상태 테스트

네트워크 장애나 속도가 느린 환경에서 애플리케이션이 잘 동작하는지 테스트할 수 있습니다. 
tc (traffic control) 명령어를 사용해 네트워크 속도를 인위적으로 조정한 후 부하를 걸어보세요.

```bash
sudo tc qdisc add dev eth0 root tbf rate 100mbit burst 32kbit latency 400ms
ab -n 1000 -c 50 http://localhost:8080/api/v1/resource
```

tc 명령어로 네트워크 속도를 조정하고, 그 상황에서 부하 테스트를 진행합니다.

### 6. 🎯 결론

이 가이드를 통해 Spring Boot 애플리케이션의 부하 테스트를 진행하고, 성능 및 확장성에 대한 정보를 얻을 수 있습니다. 

필요에 따라 추가적인 테스트 시나리오를 적용하여 애플리케이션의 모든 측면을 점검하세요! 💡

Happy Testing! 😊
