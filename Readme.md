https://trends.google.com/trends/explore?date=today%205-y&q=haproxy,nginx,envoy&hl=ko

# 데모 구성으로 Envoy 실행
``` shell
docker run --rm -it -p 9901:9901 -p 10000:10000 envoyproxy/envoy:dev-876753ad28d6601b91c25b8af59db4f4737c84a5
curl -v localhost:10000
```
http://localhost:9901/help



# 기본구성재정의
envoy-overrride.yaml
```yaml
admin:
  address:
    socket_address:
      address: 127.0.0.1
      port_value: 9902
```

```shell
docker run --rm -it \
      -p 9902:9902 \
      -p 10000:10000 \
      envoyproxy/envoy:dev-876753ad28d6601b91c25b8af59db4f4737c84a5 \
          -c /etc/envoy/envoy.yaml \
          --config-yaml "$(cat envoy-override.yaml)"
```

# Envoy 구성 검증
```shell
docker run --rm \
      -v $(pwd)/my-envoy-config.yaml:/my-envoy-config.yaml \
      envoyproxy/envoy:dev-876753ad28d6601b91c25b8af59db4f4737c84a5 \
          --mode validate \
          -c my-envoy-config.yaml
 ```
```shell
docker run --rm -v $(pwd)/envoy-override.yaml:/my-envoy-config.yaml envoyproxy/envoy:dev-876753ad28d6601b91c25b8af59db4f4737c84a5 --mode validate -c my-envoy-config.yaml
```

# 정적 구성 store-envooy 
https://www.envoyproxy.io/docs/envoy/latest/start/quick-start/run-envoy

fuga@FUGA-PC:~/envoy$ ls
envoy-config.yaml  envoy-custom.yaml  envoy-demo.yaml  envoy-dynamic-filesystem-demo.yaml  envoy-override.yaml  envoy-store.yaml  logs  my-envoy-config.yaml
```
docker run --rm -it -p 9902:9902 -p 10000:10000 -v $(pwd)/envoy-store.yaml:/my-envoy-config.yaml envoyproxy/envoy:dev-876753ad28d6601b91c25b8af59db4f4737c84a5 -c my-envoy-config.yaml
```

http://localhost:10000/store/aion
http://localhost:10000/docs


# 동적구성 store-envoy

## go-conrol-plane
```shell
 git clone https://github.com/envoyproxy/go-control-plane.git
```
