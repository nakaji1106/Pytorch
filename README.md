# Pytorch Dockerfile for ppc64le

```
# ls
Dockerfile
# docker build -t <Account name>/<Image name>:<Tag> .
```

上記手順にてビルドされたDockerImageには、以下のコンポーネントが含まれています。
コンテナ起動時の自動起動サービスとしては設定していないため、稼働後にサービスの立ち上げが必要となります

- Openssh server
- Jupyter Notebook

以上
