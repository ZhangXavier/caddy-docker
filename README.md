# caddy2-docker

![Build and Publish Docker](https://github.com/ZhangXavier/caddy-docker/workflows/Build%20and%20Publish%20Docker%20Linux%20image/badge.svg)

这是一个caddy2 docker image，编译加入了如下组件：

- [caddy-dns/cloudflare](https://github.com/caddy-dns/cloudflare)
- [mastercactapus/caddy2-proxyprotocol](https://github.com/mastercactapus/caddy2-proxyprotocol)
- [hairyhenderson/caddy-teapot-module](https://github.com/hairyhenderson/caddy-teapot-module)
- [caddyserver/nginx-adapter](https://github.com/caddyserver/nginx-adapter) (在`2.4`版本中已经去除该组件)

## 已知的问题

- `buildx`编译`golang`，在`s390x`上存在问题，见：[issues/40949](https://github.com/golang/go/issues/40949)。故暂停创建`s390x`的image。

- 不允许使用Token，更新DockerHub存储库描述。 见：[issues/1927](https://github.com/docker/hub-feedback/issues/1927),[issues/1914](https://github.com/docker/hub-feedback/issues/1914),[issues/115](https://github.com/docker/roadmap/issues/115)

- 暂不提供`macOS`与`Windows`版image

## 说明

- 建议使用`latest`版本，与`caddy`的`latest`版本相同。

- 同时提供Docker Hub与Github的package地址。Docker Hub:[hub.docker.com/r/zxavier/caddy)](https://hub.docker.com/r/zxavier/caddy), Github容器地址：[ghcr.io/ZhangXavier/caddy](https://ghcr.io/ZhangXavier/caddy)，两地址版本号相同。

- ⚠️`mainline`版本为最新发布版，包括`rc`与`bate`版，故请在低风险环境中使用。

- `v2.3.0`版本以及后续版本提示如下：

    1. 设置：net.core.rmem_max。这个值在linux上很小，对于高带宽`quic`传输来说需要调整大一些。看这里：[quic-go/wiki/UDP-Receive-Buffer-Size](https://github.com/lucas-clemente/quic-go/wiki/UDP-Receive-Buffer-Size)，Linux调整（Docker的host模式）可以参考连接中的内容。对于Docker，如果不是使用的host模式，对于docker-compose调整见这里：[compose-file-v2/sysctls](https://docs.docker.com/compose/compose-file/compose-file-v2/#sysctls)，需要V2.1以上版本的compose。对于docker run调整见这里：[docker-run/sysctls](https://docs.docker.com/engine/reference/commandline/run/#configure-namespaced-kernel-parameters-sysctls-at-runtime)。在caddy日志的提示如下：
        > failed to sufficiently increase receive buffer size (was: 208 kiB, wanted: 2048 kiB, got: 416 kiB). See https://github.com/lucas-clemente/quic-go/wiki/UDP-Receive-Buffer-Size for details.

    2. remote_ip默认情况下，匹配器不再读取X-Forwarded-For标头。这是未记录的行为，并且是不安全的默认值。如果您碰巧依赖于此，请启用forwarded（在Caddyfile中，仅将其forwarded作为范围之前的第一个参数）以保持该行为。

    3. `️experimental_http3` 已经不在`Caddyfile`的全局选项中了，如果需要使用的话要在Json里设置：'servers > protocol > experimental_http3'。在caddy日志的提示如下：
        > [WARNING][caddyfile] :0: the 'experimental_http3' global option is deprecated, please use the 'servers > protocol > experimental_http3' option instead

## 鸣谢

- [caddy](https://github.com/caddyserver/caddy)
- [xcaddy](https://github.com/caddyserver/xcaddy)
- [caddy-docker](https://github.com/caddyserver/caddy-docker)
- [caddy-dns/cloudflare](https://github.com/caddy-dns/cloudflare)
- [caddyserver/nginx-adapter](https://github.com/caddyserver/nginx-adapter)
- [mastercactapus/caddy2-proxyprotocol](https://github.com/mastercactapus/caddy2-proxyprotocol)

## License

[Apache License 2.0](LICENSE)
