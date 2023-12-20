# Hoop Helm Chart

- Website: https://hoop.dev
- Documentation: https://hoop.dev/docs

[Helm](https://helm.sh) must be installed to use the chart.
Please refer to Helm's [documentation](https://helm.sh/docs/) to get started.

## Installing Self Hosted

Installing latest version of hoop. For different version check out the [releases page](https://github.com/hoophq/hoopcli/releases)

> Please refer to [gateway configuration reference](https://hoop.dev/docs/configuring/gateway) for more information

```sh
cat - > ./values.yaml <<EOF
# use latest docker image or pin the version
image:
  gw:
    tag: latest
  agent:
    tag: latest

# gateway configuration
config:
  POSTGRES_DB_URI: ''
  API_URL: ''
  IDP_ISSUER: ''
  IDP_CLIENT_ID: ''
  IDP_CLIENT_SECRET: ''
  IDP_AUDIENCE: ''

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

## Installing Hoop Agent

Please refer to [agent configuration reference](https://hoop.dev/docs/configuring/agent) for more information.

```sh
VERSION=$(curl -s https://hoopartifacts.s3.amazonaws.com/release/latest.txt)
helm upgrade --install hoopagent https://hoopartifacts.s3.amazonaws.com/release/$VERSION/hoopagent-chart-$VERSION.tgz \
    --set 'config.gateway.grpc_url=' \
    --set 'config.gateway.token='
```


> The gRPC url of our SaaS instance is configured as https://app.hoop.dev:8443.
> If you have your own gateway, provide a valid public address for the option `config.gateway.grpc_url`

## Development

To add new configuration(s)

1. Go to `./chart/gateway|agent/templates/secrets-config.yaml`
2. Add any relevant environment variables
3. Edit `./chart/gateway|agent/values.yaml` and add defaults or any necessary comment

Test it by running

```sh
helm template ./chart/<component>/ -f yourvalues.yaml
```

Use helm lint to see if everything is ok

```sh
helm lint ./chart/<component>/ -f yourvalues.yaml
```
