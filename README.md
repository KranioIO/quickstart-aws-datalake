# Quickstart aws datalake

A quickstart project to deploy a serverless datalake over AWS Cloud using S3 Service, all built using Infrastructure as Code (IaC) with best practices applied for data governance.


### Getting Started
---

This project deploys 4 buckets on s3, one for logs and 3 datalake tiers (ingestion -> processing -> consumption), all datalake buckets with:
- server side encryption
- file versioning
- lifecycle rules
- action log track

### Deploy
---

The first **important** step is change the service name over `serverless.yml` file since the names over s3 service are global and can not repeat, By default the project is deployed as dev stage, but you can set with `--stage`, example `serverless deploy --stage qa`.

```yaml
service: YOUR-ORGANIZATION-NAME-datalake

custom:
  tags: ${file(tags.yml):${self:provider.stage}}
  prefix: ${self:service}-${self:provider.stage}

...
```

Obs: S3 only allow lower case names, so your organization name should follow this rule, or you can use [this plugin](https://www.npmjs.com/package/serverless-plugin-utils) for serverless framework utils funciontions like `fn::lower` and avoid this limitation.

The second step is setup adm accounts for KMS encryption at the `datalake-kms-administrators` folder for the desired stage.

To deploy the project you need an AWS account, a CLI environment configured with [AWS-CLI](https://aws.amazon.com/cli/) and [Serverless](https://www.serverless.com/framework/docs/getting-started#via-npm) configured, execute `aws configure` to setup your credentials.

To deploy your datalake just run: `export AWS_PROFILE="your-account-profile" && serverless deploy`
