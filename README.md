# 🌟 Spring Boot 애플리케이션 부하 테스트 가이드 🌟

이 가이드는 **Ubuntu 환경**에서 **Spring Boot 애플리케이션**에 대한 부하 테스트를 수행하는 방법을 설명합니다. 

부하 테스트는 애플리케이션의 성능과 안정성을 평가하는 중요한 과정입니다. 

아래의 도구들을 사용해 효율적으로 테스트해보세요! 🚀

## 👨‍💻Team

|<img src="https://avatars.githubusercontent.com/u/139302518?v=4" width="100" height="100"/>|<img src="https://avatars.githubusercontent.com/u/78792358?v=4" width="100" height="100"/>|
|:-:|:-:|
|곽병찬<br/>[@gato-46](https://github.com/gato-46)|박현우<br/>[@smartcow99](https://github.com/smartcow99)|

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
- **Dependency Management**: Gradle

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
![image](https://github.com/user-attachments/assets/f504354a-c3a6-4356-8a14-b82c0613fcd3)


```text
Server Software:        
Server Hostname:        localhost
Server Port:            8080

Document Path:          /
Document Length:        89 bytes

Concurrency Level:      50
Time taken for tests:   2.235 seconds
Complete requests:      1000
Failed requests:        0
Non-2xx responses:      1000
Total transferred:      283000 bytes
HTML transferred:       89000 bytes
Requests per second:    447.36 [#/sec] (mean)
Time per request:       111.766 [ms] (mean)
Time per request:       2.235 [ms] (mean, across all concurrent requests)
Transfer rate:          123.64 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    6  16.0      0      86
Processing:     2  103  49.9     98     479
Waiting:        0   96  47.2     88     394
Total:          9  109  49.0    106     479

Percentage of the requests served within a certain time (ms)
  50%    106
  66%    125
  75%    136
  80%    144
  90%    171
  95%    192
  98%    218
  99%    235
 100%    479 (longest request)
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

```bash
❯ stress-ng --cpu 4 --vm 2 --vm-bytes 512M --timeout 60s
stress-ng: info:  [5627] setting to a 60 second run per stressor
stress-ng: info:  [5627] dispatching hogs: 4 cpu, 2 vm
stress-ng: info:  [5627] successful run completed in 60.08s (1 min, 0.08 secs)
```

### 5. 📈 추가 테스트 시나리오

Spring Boot 애플리케이션에 대한 다양한 추가 테스트 시나리오도 진행해 보세요!

#### 5.1. 🔄 특정 API 엔드포인트 부하 테스트

특정 API 엔드포인트에 대해 부하를 주고 싶다면, 다음과 같이 URL을 지정하세요:

```bash
ab -n 1000 -c 50 http://localhost:8080/api/v1/resource
```

##### 📊 예시 결과:
![image](https://github.com/user-attachments/assets/d7af392b-13e3-4d0a-bc60-1f85e75c4273)

```text
Server Software:        
Server Hostname:        localhost
Server Port:            8080

Document Path:          /api/v1/resource
Document Length:        13 bytes

Concurrency Level:      50
Time taken for tests:   2.395 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      146000 bytes
HTML transferred:       13000 bytes
Requests per second:    417.45 [#/sec] (mean)
Time per request:       119.774 [ms] (mean)
Time per request:       2.395 [ms] (mean, across all concurrent requests)
Transfer rate:          59.52 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   1.0      0       9
Processing:   101  112  15.4    108     254
Waiting:      100  111  15.3    107     254
Total:        101  112  15.6    108     254

Percentage of the requests served within a certain time (ms)
  50%    108
  66%    113
  75%    117
  80%    119
  90%    125
  95%    135
  98%    149
  99%    201
 100%    254 (longest request)
```

#### 5.2. 💾 디스크 I/O 스트레스 테스트

애플리케이션의 로그 또는 데이터베이스가 저장된 디스크에 부하를 주는 방법:

```bash
dd if=/dev/zero of=/tmp/testfile bs=1G count=10 oflag=direct
```
이 명령어는 10GB의 임시 파일을 생성해 디스크 I/O 부하를 테스트합니다.


##### 📊 예시 결과:

![image](https://github.com/user-attachments/assets/beba1440-29a0-41b6-bdcb-be80f12f7da1)

```text
6+0 records in
5+0 records out
5368709120 bytes (5.4 GB, 5.0 GiB) copied, 15.8357 s, 339 MB/s
```

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

##### 📊 예시 결과:

![image](https://github.com/user-attachments/assets/87130d4f-328c-4496-8ac9-c231ebe6cfb6)

```text
Server Software:        
Server Hostname:        localhost
Server Port:            8080

Document Path:          /api/v1/resource/async
Document Length:        31 bytes

Concurrency Level:      100
Time taken for tests:   0.968 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      164000 bytes
HTML transferred:       31000 bytes
Requests per second:    1032.59 [#/sec] (mean)
Time per request:       96.844 [ms] (mean)
Time per request:       0.968 [ms] (mean, across all concurrent requests)
Transfer rate:          165.38 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   1.3      0       7
Processing:     1   87  36.0     81     256
Waiting:        1   87  36.0     81     256
Total:          1   88  35.7     82     258

Percentage of the requests served within a certain time (ms)
  50%     82
  66%     98
  75%    107
  80%    115
  90%    137
  95%    153
  98%    173
  99%    200
 100%    258 (longest request)
```


#### 5.6 📅 스케줄링 작업 부하 테스트

Spring Boot 애플리케이션은 스케줄링 작업(Cron jobs)을 사용할 수 있습니다. 
스케줄링된 작업이 실행되는 동안 부하가 걸릴 때 애플리케이션이 어떻게 반응하는지 확인할 수 있습니다.

애플리케이션이 5초마다 스케줄링 작업을 수행하는 경우, 스케줄링 중 부하를 줄 수 있는 방법을 시뮬레이션하세요:

```bash
ab -n 5000 -c 200 http://localhost:8080/api/v1/resource
```


##### 📊 예시 결과:
![image](https://github.com/user-attachments/assets/2ff25f17-bdb0-44d7-b117-f6d17b714a38)


```text
❯ ab -n 5000 -c 200 http://localhost:8080/api/v1/resource
This is ApacheBench, Version 2.3 <$Revision: 1879490 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 500 requests
Completed 1000 requests
Completed 1500 requests
Completed 2000 requests
Completed 2500 requests
Completed 3000 requests
Completed 3500 requests
Completed 4000 requests
Completed 4500 requests
Completed 5000 requests
Finished 5000 requests


Server Software:        
Server Hostname:        localhost
Server Port:            8080

Document Path:          /api/v1/resource
Document Length:        13 bytes

Concurrency Level:      200
Time taken for tests:   2.846 seconds
Complete requests:      5000
Failed requests:        0
Total transferred:      730000 bytes
HTML transferred:       65000 bytes
Requests per second:    1756.79 [#/sec] (mean)
Time per request:       113.844 [ms] (mean)
Time per request:       0.569 [ms] (mean, across all concurrent requests)
Transfer rate:          250.48 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    2   2.9      1      17
Processing:   100  105   7.2    103     156
Waiting:      100  104   6.7    102     156
Total:        100  107   8.3    104     160

Percentage of the requests served within a certain time (ms)
  50%    104
  66%    106
  75%    108
  80%    110
  90%    119
  95%    125
  98%    131
  99%    145
 100%    160 (longest request)
```


#### 5.7 🔄 무한 루프 또는 메모리 누수 테스트
애플리케이션이 무한 루프에 빠지거나 메모리 누수가 발생할 가능성을 탐지하기 위해, 장시간 부하 테스트를 진행합니다. 아래 명령어로 몇 시간 동안 지속적으로 부하를 걸어 성능을 모니터링합니다.

```bash
ab -n 100000 -c 100 http://localhost:8080/api/v1/resource
```
- `-n 100000`: 10만 개의 요청을 연속적으로 보냅니다.
- `-c 100`: 동시 요청 수를 100으로 유지합니다.


##### 📊 예시 결과:
![image](https://github.com/user-attachments/assets/cda0c645-da3c-4f5c-b621-0b2e448febd3)


```text
❯ ab -n 100000 -c 100 http://localhost:8080/api/v1/resource
This is ApacheBench, Version 2.3 <$Revision: 1879490 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 10000 requests
Completed 20000 requests
Completed 30000 requests
Completed 40000 requests
Completed 50000 requests
Completed 60000 requests
Completed 70000 requests
Completed 80000 requests
Completed 90000 requests
Completed 100000 requests
Finished 100000 requests


Server Software:        
Server Hostname:        localhost
Server Port:            8080

Document Path:          /api/v1/resource
Document Length:        13 bytes

Concurrency Level:      100
Time taken for tests:   106.548 seconds
Complete requests:      100000
Failed requests:        0
Total transferred:      14600000 bytes
HTML transferred:       1300000 bytes
Requests per second:    938.54 [#/sec] (mean)
Time per request:       106.548 [ms] (mean)
Time per request:       1.065 [ms] (mean, across all concurrent requests)
Transfer rate:          133.82 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    2   1.5      1      25
Processing:   100  105   3.3    104     131
Waiting:       90  103   2.6    102     130
Total:        100  106   4.1    105     131

Percentage of the requests served within a certain time (ms)
  50%    105
  66%    108
  75%    109
  80%    110
  90%    112
  95%    114
  98%    116
  99%    119
 100%    131 (longest request)
```

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
