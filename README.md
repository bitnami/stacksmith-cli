# stacksmith-cli
CLI utility to talk to https://stacksmith.bitnami.com

The main use case is to call this utility from the build automation (CI) tool that
already builds your application and let Stacksmith build cloud images for you.

## Example

1. Create an application on https://stacksmith.bitnami.com
2. Create a `Stackerfile.yml` file pointing to your app and your local build artifacts to upload
3. Depending on the template you might also want to add custom build and run scripts
4. run `stacksmith build`

See full example in https://github.com/bitnami-labs/stacksmith-ci-example using Travis CI.

## Configuring CI authorization

If you need to run stacksmith build on another machine, e.g. as part of an automated build job,
you obviously cannot rely on the interactive `stacksmith auth login` tool on the server.

You can export a token for such purposes by following these instructions:

1. Make sure you have a working account on https://stacksmith.bitnami.com

2. Download the stacksmith client (see the [release page](https://github.com/bitnami/stacksmith-cli/releases)) for your platform and copy&paste the output of the following command into a secret env var, e.g. `STACKSMITH_AUTH_TOKEN` on your CI system.

```
stacksmith auth login --print
```

This command will open a browser window which might ask you to log-in into stacksmith if you haven't already.
The resulting authentication token lasts only 24h.
Please [contact us](https://bitnami.com/stacksmith#contact) if you need the enterprise grade access tokens suitable for long running automation.
