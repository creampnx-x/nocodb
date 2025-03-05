# 本地打包总结

> 先配置环境
> + nodejs v22
> + npm 
> + pnpm


## 1. 先运行安装依赖

```bash
pnpm bootstrap
```

## 2. 前端进行编译

```bash
cd packages/nc-gui
pnpm generate
```

## 3. 将编译产物部署到后端

```bash
mv packages/nc-gui/.output/public packages/nocodb/docker/nc-gui
```

## 4. 后端进行编译

> ⚠ 注意：将 `packages/nocodb/docker/rspack.config.js` 中的 `entry` 改为 `'./src/run/local.ts'`

```bash
cd packages/nocodb
pnpm run build
```

## 5. docker build

```bash
docker build . -f Dockerfile.nepdi -t nocodb-nepdi
```

## 6. docker run 

```bash
docker run -d \
  --name nocodb \
  -v "$(pwd)"/home/mapinxue/nocodb:/usr/app/data/ \
  -p 8080:8080 \
  nocodb-nepdi
```