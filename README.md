# caddy2-docker

[![Build and Publish 2.6 Docker Linux image](https://github.com/ZhangXavier/caddy-docker/actions/workflows/docker-linux-build_2.6.yml/badge.svg)](https://github.com/ZhangXavier/caddy-docker/actions/workflows/docker-linux-build_2.6.yml)
[![Build and Publish 2.7 Docker Linux image](https://github.com/ZhangXavier/caddy-docker/actions/workflows/docker-linux-build_2.7.yml/badge.svg)](https://github.com/ZhangXavier/caddy-docker/actions/workflows/docker-linux-build_2.7.yml)

这是一个 caddy2 docker image，编译加入了如下组件：

- [caddy-dns/cloudflare](https://github.com/caddy-dns/cloudflare)
- [caddy-dns/duckdns](https://github.com/caddy-dns/duckdns)
- ~~[mastercactapus/caddy2-proxyprotocol](https://github.com/mastercactapus/caddy2-proxyprotocol)~~
- [hairyhenderson/caddy-teapot-module](https://github.com/hairyhenderson/caddy-teapot-module)
- ~~[caddyserver/nginx-adapter](https://github.com/caddyserver/nginx-adapter) (在 `2.4` 版本中已经去除该组件)~~
- ~~[greenpau/caddy-security](https://github.com/greenpau/caddy-security) (由于我使用了在 `2.7.4` 版本中已经去除该组件)~~

## 已知的问题

- `s390x` 编译问题已经得到解决，见 [feat: add linux/s390x builds](https://github.com/kyverno/kyverno/pull/3277) 从 2.4.6 版本开始提供 `s390x` 的 image。~~`buildx` 编译 `golang`，在 `s390x` 上存在问题，见：[issues/40949](https://github.com/golang/go/issues/40949)。故暂停创建 `s390x` 的 image。~~

- 不允许使用 Token，更新 DockerHub 存储库描述。 见：[issues/1927](https://github.com/docker/hub-feedback/issues/1927),[issues/1914](https://github.com/docker/hub-feedback/issues/1914),[issues/115](https://github.com/docker/roadmap/issues/115)

- 暂不提供 `macOS` 与 `Windows` 版 image

## 说明

- 建议使用 `latest` 版本，与 `caddy` 的 `latest` 版本相同。

- 同时提供 Docker Hub 与 Github 的 package 地址。Docker Hub:[hub.docker.com/r/zxavier/caddy](https://hub.docker.com/r/zxavier/caddy), Github 容器地址：[ghcr.io/ZhangXavier/caddy](https://ghcr.io/ZhangXavier/caddy)，两地址版本号相同。

- ⚠️`mainline` 版本为最新发布版，包括 `rc` 与 `bate` 版，故请在低风险环境中使用。

- `v2.3.0` 版本以及后续版本提示如下：

    1. 设置：net.core.rmem_max。这个值在 linux 上很小，对于高带宽 `quic` 传输来说需要调整大一些。看这里：[quic-go/wiki/UDP-Receive-Buffer-Size](https://github.com/lucas-clemente/quic-go/wiki/UDP-Receive-Buffer-Size)，Linux 调整（Docker的host模式）可以参考连接中的内容。对于 Docker，如果不是使用的host 模式，对于 docker-compose 调整见这里：[compose-file-v2/sysctls](https://docs.docker.com/compose/compose-file/compose-file-v2/#sysctls)，需要 V2.1 以上版本的 compose。对于 docker run 调整见这里：[docker-run/sysctls](https://docs.docker.com/engine/reference/commandline/run/#configure-namespaced-kernel-parameters-sysctls-at-runtime)。在 caddy 日志的提示如下：
        > failed to sufficiently increase receive buffer size (was: 208 kiB, wanted: 2048 kiB, got: 416 kiB). See [https://github.com/lucas-clemente/quic-go/wiki/UDP-Receive-Buffer-Size](https://github.com/lucas-clemente/quic-go/wiki/UDP-Receive-Buffer-Size) for details.

    2. remote_ip 默认情况下，匹配器不再读取 X-Forwarded-For 标头。这是未记录的行为，并且是不安全的默认值。如果您碰巧依赖于此，请启用 forwarded（在 Caddyfile 中，仅将其 forwarded 作为范围之前的第一个参数）以保持该行为。

    3. `️experimental_http3` 已经不在 `Caddyfile` 的全局选项中了，如果需要使用的话要在 Json 里设置：'servers > protocol > experimental_http3'。在 caddy 日志的提示如下：

        ``` sh
            [WARNING][caddyfile] :0: the 'experimental_http3' global option is deprecated, please use the 'servers > protocol > experimental_http3' option instead
        ```

## 鸣谢

- [caddy](https://github.com/caddyserver/caddy)
- [xcaddy](https://github.com/caddyserver/xcaddy)
- [caddy-docker](https://github.com/caddyserver/caddy-docker)
- [caddy-dns/cloudflare](https://github.com/caddy-dns/cloudflare)
- [caddy-dns/duckdns](https://github.com/caddy-dns/duckdns)
- [hairyhenderson/caddy-teapot-module](https://github.com/hairyhenderson/caddy-teapot-module)

## License

[Apache License 2.0](LICENSE)
