https://trends.google.com/trends/explore?date=today%205-y&q=haproxy,nginx,envoy&hl=ko

# 데모 구성으로 Envoy 실행
``` shell
docker run --rm -it -p 9901:9901 -p 10000:10000 envoyproxy/envoy:dev-876753ad28d6601b91c25b8af59db4f4737c84a5
curl -v localhost:10000
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
https://www.envoyproxy.io/docs/envoy/latest/start/quick-start/run-envoy
[envoy-store.yaml](./envoy-store.yaml)
```
docker run --rm -it -p 9901:9901 -p 10000:10000 -v $(pwd)/envoy-store.yaml:/my-envoy-config.yaml envoyproxy/envoy:dev-876753ad28d6601b91c25b8af59db4f4737c84a5 -c my-envoy-config.yaml
```
- http://localhost:10000/store/aion
- http://localhost:10000/docs


# 동적 구성 store-envoy
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

```
apt-get update
apt-get install curl
curl 172.30.112.1:18000
curl 172.30.112.1:18000
```