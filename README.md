<div align="center">
<br>
<img width="200" src="https://raw.githubusercontent.com/58xinian/icon/master/Sub-Store1.png" alt="Sub-Store">
<br>
<br>
<h2 align="center">Sub-Store VPS 部署<h2>
</div>

项目修改于：<https://github.com/dompling/DockerFiles/tree/master/Sub-Store>

特别感谢：[@dompling](https://github.com/dompling)

# 请注意，该镜像的前端部分已替换为 [SaintWe/Sub-Store-Front-End](https://github.com/SaintWe/Sub-Store-Front-End)，若有疑请避免使用

*注意：由于后端不含身份认证，在公网使用请自行做相关的保护*

**重新构建了 [Cloudflare Workers 版的 Sub-Store](https://github.com/SaintWe/Sub-Store-Workers) 已实现基于 http authorization bearer 身份认证，前端部分已适配**

## Docker-compose 部署

*在 NODE 环境下运行或可能遇到缓存不清理的问题*

``` yml
version: '3'

# 该镜像含前端
services:
  substore:
    image: saintwe/sub-store:latest
    container_name: substore
    restart: always
    ports:
      - "6080:80"
    volumes:
      - ./root.json:/Sub-Store/root.json
      - ./sub-store.json:/Sub-Store/sub-store.json
    environment:
      - TZ=Asia/Shanghai
      # 如需使用前端请取消注释，用于修改默认的后端地址
      # 仅推荐您在内网环境下使用镜像自带的前端
      # - DOMAIN=http://youdomain
```

将上面内容调整后放到服务器 `docker-compose.yml` 中

**仅推荐您在内网环境下使用镜像自带的前端，公网下可使用下述 2 个由我构建的前端，或您自行构建**

- **Vercel**：<https://sub-store-workers.vercel.app>
- **Cloudflare Pages**：<https://sub-store-workers.pages.dev>
- [关于前端的更多使用细节](https://github.com/SaintWe/Sub-Store-Workers#%E5%89%8D%E7%AB%AF)

在 `docker-compose.yml` 同目录中执行下面 2 条命令

``` sh
echo "{}" > ./sub-store.json
echo "{}" > ./root.json
```

一切准备就绪后在 `docker-compose.yml` 同目录执行

```
# 守护启动
docker-compose up -d

# 打印日志
docker-compose logs

# 更新镜像
docker-compose pull

# 停止容器
docker-compose stop

# 重启容器
docker-compose restart

# 停止并删除容器
docker-compose down
```

## 前端

Fork 此仓库配合 [Cloudflare Pages](https://pages.cloudflare.com/) 以及 [Zero Trust](https://one.dash.cloudflare.com/) 的访问策略可实现前端的身份认证

```
# 构建命令
sed -i "s|https://sub.store|https://youdomain|g" `grep https://sub.store -rl $(pwd)/dist`
```

## 结束语

- 感谢 [@dompling](https://github.com/dompling)
- 感谢 [@Peng-YM](https://github.com/Peng-YM/Sub-Store) 大佬的无私奉献将代码开源
