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
# use latest docker image or pin the version
image:
  gw:
    tag: latest
  xtdb:
    tag: latest
  agent:
    tag: latest

# gateway configuration
config:
  API_URL: ''
  IDP_ISSUER: ''
  IDP_CLIENT_ID: ''
  IDP_CLIENT_SECRET: ''
  IDP_AUDIENCE: ''

# enable a default agent running as a sidecard in the gateway
agentConfig:
  enabled: true
  AUTO_REGISTER: 'true'

# gateway database configuration
xtdbConfig:
  PG_HOST: ''
  PG_PORT: '5432'
  PG_USER: ''
  PG_PASSWORD: ''
  PG_DB: ''

# gateway persistence for audits
persistence:
  enabled: false
  storageClassName: ''

# enable ingress for the api / webapp services
ingressApi:
  enabled: false
  ingressClassName: nginx
  annotations: {}
    # kubernetes.io/tls-acme: "true"

  host: hoop.yourdomain.tld
  # -- TLS secret name for nginx ingress
  # tlsSecret: ''

# enable ingress for the gRPC service
ingressGrpc:
  enabled: false
  ingressClassName: nginx
  annotations: {}
    # kubernetes.io/tls-acme: "true"

  host: hoop.yourdomain.tld
  # -- TLS secret name for nginx ingress
  # tlsSecret: ''
EOF
```

```sh
VERSION=$(curl -s https://hoopartifacts.s3.amazonaws.com/release/latest.txt)
helm upgrade --install hoop \
  https://hoopartifacts.s3.amazonaws.com/release/$VERSION/hoop-chart-$VERSION.tgz \
  -f values.yaml
```

Installing hoop agent

> Please refer to [agent configuration reference](https://hoop.dev/docs/configuring/agent) for more information.

```sh
VERSION=$(curl -s https://hoopartifacts.s3.amazonaws.com/release/latest.txt)
helm upgrade --install hoopagent https://hoopartifacts.s3.amazonaws.com/release/$VERSION/hoopagent-chart-$VERSION.tgz \
    --set 'config.gateway.grpc_url=' \
    --set 'config.gateway.token='
```


> The gRPC url of our SaaS instance is configured as https://app.hoop.dev:8443.
> If you have your own gateway, provide a valid public address for the option `config.gateway.grpc_url`
