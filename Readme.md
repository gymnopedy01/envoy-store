# 리버스 프록시란
리버스 프록시(reverse proxy)는 컴퓨터 네트워크에서 클라이언트를 대신해서 한 대 이상의 서버로부터 자원을 추출하는 프록시 서버의 일종이다. 그런 다음 이러한 자원들이 마치 웹 서버 자체에서 기원한 것처럼 해당 클라이언트로 반환된다. 관련 클라이언트들을 위해 임의의 서버에 접속하는 중간 매개체인 포워드 프록시(forward proxy)와는 반대로, 리버스 프록시는 관련 서버들을 위해 임의의 클라이언트가 해당 서버에 접속하는 중간 매개체이다. 

널리 보급된 웹 서버들은 리버스 프록시 기능을 사용하는 일이 잦으며 취약한 HTTP 기능의 애플리케이션 프레임워크를 보호한다.  

# 트렌드
https://trends.google.com/trends/explore?date=today%205-y&q=haproxy,nginx,envoy&hl=ko

# 데모 구성으로 Envoy 실행
``` shell
docker run --rm -it -p 9901:9901 -p 10000:10000 envoyproxy/envoy:dev-876753ad28d6601b91c25b8af59db4f4737c84a5
curl -v localhost:10000
docker exec 89e4ca793a0d cat /etc/envoy/envoy.yaml
```
http://localhost:9901/help


# 기본구성재정의
[envoy-override.yaml](./envoy-override.yaml)
```yaml
admin:
  address:
    socket_address:
      address: 0.0.0.0
      port_value: 9902
```
```shell
docker run --rm -it \
      -p 9901:9901 \
      -p 9902:9902 \
      -p 10000:10000 \
      envoyproxy/envoy:dev-876753ad28d6601b91c25b8af59db4f4737c84a5 \
          -c /etc/envoy/envoy.yaml \
          --config-yaml "$(cat envoy-override.yaml)"
```
http://localhost:9902/help


# Envoy 구성 검증
[my-envoy-config.yaml](./my-envoy-config.yaml)
```shell
docker run --rm \
      -v $(pwd)/my-envoy-config.yaml:/my-envoy-config.yaml \
      envoyproxy/envoy:dev-876753ad28d6601b91c25b8af59db4f4737c84a5 \
          --mode validate \
          -c my-envoy-config.yaml
 ```

# 정적 구성 store-envoy 
- https://www.envoyproxy.io/docs/envoy/latest/start/quick-start/run-envoy
- [envoy-store.yaml](./envoy-store.yaml)
```
docker run --rm -it -p 9901:9901 -p 10000:10000 -v $(pwd)/envoy-store.yaml:/my-envoy-config.yaml envoyproxy/envoy:dev-876753ad28d6601b91c25b8af59db4f4737c84a5 -c my-envoy-config.yaml
```
- http://localhost:10000/store/aion
- http://localhost:10000/docs


# 동적 구성 store-envoy
[envoy-dynamic-control-plane-demo.yaml](./envoy-dynamic-control-plane-demo.yaml)
```shell
docker run --rm -it -p 19000:19000 -p 10000:10000 -v $(pwd)/envoy-dynamic-control-plane-demo.yaml:/my-envoy-config.yaml envoyproxy/envoy:dev-876753ad28d6601b91c25b8af59db4f4737c84a5 -c my-envoy-config.yaml
```

## go-conrol-plane
```shell
 git clone https://github.com/envoyproxy/go-control-plane.git
 make docker_tests
 go-control-plane/bin/example
```

## java-control-plane
https://github.com/envoyproxy/java-control-plane

## envoy내 curl 설치
```
apt-get update
apt-get install curl
apt-get install curl
curl 172.30.112.1:18000
curl 172.30.112.1:18000
```


sudo apt-get install libc6-dev
