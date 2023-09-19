## Error contacting service. It is probably not running.

``` bash
sh /opt/zookeeper/bin/zkServer.sh status
JMX enabled by default
Using config: /opt/zookeeper/bin/../conf/zoo.cfg
Error contacting service. It is probably not running.zkServer.sh status
```

[[Zookeeper]] 최초 실행 시 문제가 발생함

최초 실행 시 아래와 같은 문제 발생
```
drwxr-xr-x 1 root root 4096 Sep 12 02:53 zookeeper
drwxr-xr-x 2 root root 4096 Sep 12 02:53 zookeeper?
```
알 수 없는 zookeeper? 디렉토리 안에 `zookeeper_server.pid`가 생성됨

시도 01 (zoo.cfg)
```
# 아래 경ㄹ로를
dataDir=/data/zookeeper
# 아래와 같이 변경
dataDir=/data/zookeeper/
```
위와 같이 하니 zookeeper?는 사라지지만
```
drwxr-xr-x 2 root root 4096 Sep 12 02:57 ?
-rw-r--r-- 1 root root    2 Sep 12 02:57 myid
drwxr-xr-x 2 root root 4096 Sep 12 02:57 version-2
```
위와 같이 `/data/zookeeper/?` 안에 `zookeeper_server.pid` 가 생성됨

## 문제 원인
zoo.cfg 의 텍스트 제어 문자가 [CRLF](/Code_Convention/new_line/new_line)이기 때문에 문제가 발생 한 것.
