# WAS 사용자 만들고 사용하기
1. WAS 서버에 root로 접속한다.
2. 만들고자 하는 ${USERNAME}를 결정하여 다음과 같이 root 권한을 준다.

```shell
# 새로운 유저를 생성합니다.
$ sudo useradd -s /bin/bash -m -d /home/${USERNAME} -g root ${USERNAME}

# 유저 비밀번호를 설정합니다.
$ sudo passwd ${USERNAME}
> 패스워드 입력

# sudoers 파일 권한 변경
$ sudo chmod u+w /etc/sudoers

# sudoers 파일 열고, username 추가
$ sudo vi /etc/sudoers 
> ${USERNAME} ALL=(ALL:ALL) ALL

# passwd 파일 열고, 해당 username의 uid, gid를 0으로 변경
$ sudo vi /etc/passwd
> ${USERNAME}:x:0:0:${USERNAME}:/home/${USERNAME}:/bin/bash

# uid, gid 가 0임을 확인
$ id
> uid=0(root) gid=0(root) groups=0(root)

# sshd_config 파일에 PasswordAuthentication 설정 추가(혹은 변경) - 암호기반 인증방식 허용
$ sudo vi /etc/ssh/sshd_config
> PasswordAuthentication yes

# sshd_config 파일에 root ssh 열어주기
$ vi /etc/ssh/sshd_config
> AllowUsers ${USERNAME}
> AllowGroups root
> PermitRootLogin yes

# ssh를 재시작합니다.
$ sudo service ssh restart

# ssh [username]@host
$ ssh ${USERNAME}@[IP] 와 같은 형식으로 입력합니다.

# 위에서 설정한 패스워드를 입력합니다.

# 완료!
```

<br />

3. 해당 user 로 접속하여 로그 폴더를 만듭니다.

```shell
cd /log
mkdir app
mkdir deploy
```

해당 user로 만들지 않을 거라면 해당 user에게 권한을 준다.

```shell
chown -R ${USERNAME} /log
```

