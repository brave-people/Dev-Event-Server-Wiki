# Redis 도입기

<br />
<br />

# Heroku Redis
### 설치
```text
redis-cli -u  {redis url}
```
```text
ec2-3-***-***-***.compute-1.amazonaws.com:****> keys * 

I/O error
Error: Connection reset by peer
```
- 저렴이 사이드 프로젝트는 오늘도 저렴한 서비스를 찾으러 떠났다.
- 공짜 헤로쿠 레디스는 접속시 cli 안먹는다. 
- 에플리케이션 단에서만 붙을수 있도록 허용된것으로 보임
- 주는것도 25mb정도. 
- 많은 정보를 저장할 것이 아니기에 개발서버에 redis랑 공생을 결정

<br />
<br />

### Risk
```cmd
# Simple
free -h

# Total
cat /proc/meminfo
```
- 메모리 여유공간이 90Mi 밖에 안남음
- 헤로쿠는 25Mi 비하면 혜자. 운영레벨에서 버틸지 모르겠음
- 일단 redis 설치해보니 76Mi 남았다.
- 못버티면 Ehcache로 전환해야함