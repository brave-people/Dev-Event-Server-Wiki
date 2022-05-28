# 성능 테스트 nGrinder

## 설치 & 실행
```
# 설치
$ tar -xvf ngrinder-agent-3.5.5-p1-localhost.tar 

# 실행
$ java -jar ngrinder-controller-3.5.5-p1.war --port 7070  
```
- 접속: http://localhost:7070/login
- 초기: admin / admin

<br />
<br />


## Agent 실행
```
sh ./run_agent.sh
```
- agent 다운로드 후 실행

<br />
<br />

## 한 번에 실행
```shell
#!/bin/bash

java -jar ngrinder-controller-3.5.5-p1.war --port 7070&./ngrinder-agent/run_agent.sh
```
```
sh ./run_all.sh
```
- 각각 실행하는 것은 너무 번거롭다.
- nGrinder, agent 한 번에 실행 스크립트

<br />
<br />

## Ref
- [[nGrinder] 설치와 사용법 정리](https://velog.io/@hellonayeon/nGrinder-install-and-how-to-use-memo)