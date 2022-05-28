# Redis

### 설치
```cmd
# 설치
apt-get install redis-server

# 설치 확인
redis-server --version
> Redis server v=5.0.7

# 포트 확인(기본 6379)
netstat -nlpt | grep 6379
```

<hr />
<br />

### 설정
```text
# 설정 접근
vim /etc/redis/redis.conf

# 설정 변경
as-is bind 127.0.0.1
to-be bind 0.0.0.0

requirepass xxxxx

# Redis 재시작
systemctl restart redis-server

# Redis 상태보기
systemctl status redis-server
```


```text
# 6379 포트 오픈
sudo iptables -I INPUT 5 -i ens3 -p tcp --dport 6379 -m state --state NEW,ESTABLISHED -j ACCEPT

# 방화벽 오픈 확인
netstat -nap | grep LISTEN
```
- 참고: [Redis 외부접속 설정하기](https://infoscoco.com/92)

<hr />
<br />

#### 메모리 제한
```text
# 접속
redis-cli

# 설정
$ AUTH {비밀번호}
$ config set maxmemory 10mb
$ config set maxmemory-policy allkeys-lru
```

<hr />
<br />


#### 테스트
```
redis-cli -h ***.***.***.*** -p 6379 -a password
```

<br />

```text
# 접속
redis-cli

# 저장
set key value

# key 조회
keys *

# key 이름 변경
rename key newKey

# key로 삭제
del newKey
```

<hr />
<br />

## Ref
- https://hayden-archive.tistory.com/429
- https://cloud-oky.tistory.com/395