# Hoop Helm Chart

- Website: https://hoop.dev
- Documentation: https://hoop.dev/docs

[Helm](https://helm.sh) must be installed to use the chart.
Please refer to Helm's [documentation](https://helm.sh/docs/) to get started.

## Installing the Chart

Installing hoop, check the release version at https://github.com/hoophq/hoopcli/releases

```sh
VERSION=
cat - > ./values.yaml <<EOF
# hoop gateway configuration. Please refer to https://hoop.dev/docs/configuring/gateway
config:
  API_URL: ''
  IDP_ISSUER: ''
  IDP_CLIENT_ID: ''
  IDP_CLIENT_SECRET: ''

# hoop gateway configuration. Please refer to https://hoop.dev/docs/configuring/gateway
xtdbConfig:
  PG_HOST: ''
  PG_PORT: "5432"
  PG_USER: ''
  PG_PASSWORD: ''
  PG_DB: ''

# use latest docker image or pin the version
image:
  gw:
    tag: latest
  xtdb:
    tag: latest
  agent:
    tag: latest
EOF
helm install -f values.yaml \
    hoop https://hoopartifacts.s3.amazonaws.com/release/$VERSION/hoop-chart-$VERSION.tgz
```

Installing hoop agent


```sh
# Please refer to https://hoop.dev/docs/configuring/agent
helm install hoop https://hoopartifacts.s3.amazonaws.com/release/$VERSION/hoopagent-chart-$VERSION.tgz \
    --set 'config.SERVER_ADDRESS='
```

> Leave SERVER_ADDDRESS empty if isn't a self-hosted installation
