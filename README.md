# Hoop Helm Chart

- Website: https://hoop.dev
- Documentation: https://hoop.dev/docs

[Helm](https://helm.sh) must be installed to use the chart.
Please refer to Helm's [documentation](https://helm.sh/docs/) to get started.

## Installing the Chart

Installing latest version of hoop. For different version check out the [releases page](https://github.com/hoophq/hoopcli/releases)

> Please refer to [gateway configuration reference](https://hoop.dev/docs/configuring/gateway) for more information

```sh

cat - > ./values.yaml <<EOF
config:
  API_URL: ''
  IDP_ISSUER: ''
  IDP_CLIENT_ID: ''
  IDP_CLIENT_SECRET: ''

xtdbConfig:
  PG_HOST: ''
  PG_PORT: '5432'
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
VERSION=$(curl -s https://hoopartifacts.s3.amazonaws.com/release/latest.txt)
helm install hoop https://hoopartifacts.s3.amazonaws.com/release/$VERSION/hoop-chart-$VERSION.tgz \
  -f values.yaml
```

Installing hoop agent

> Please refer to [agent configuration reference](https://hoop.dev/docs/configuring/agent) for more information.

```sh
VERSION=$(curl -s https://hoopartifacts.s3.amazonaws.com/release/latest.txt)
helm install hoopagent https://hoopartifacts.s3.amazonaws.com/release/$VERSION/hoopagent-chart-$VERSION.tgz \
    --set 'config.SERVER_ADDRESS='
```

> Leave SERVER_ADDDRESS empty if isn't a self-hosted installation
