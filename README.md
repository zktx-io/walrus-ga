# Walrus Sites Github Actions

## Reusable GitHub Action for deploying Walrus Sites

This GitHub action allows users to easily deploy their websites to Walrus Sites using a pre-compiled Walrus Site Builder binary. The action automatically downloads the necessary binaries, streamlining the deployment process and making it easy to integrate into any repository.

### Overview

This action simplifies the deployment of sites to Walrus Sites by using a precompiled Walrus Site Builder. Users can deploy their sites without having to manage the build process themselves.

### Key features

1. **Automated Deployment**: Automatically deploys websites to Walrus Sites using a precompiled Site Builder binary.
1. **Easy Configuration**: Allows users to specify configuration files and site paths to meet different deployment needs.
1. **Seamless Integration**: Designed for reuse, this action can be easily added to workflows in different projects.

### Inputs

The following inputs are required to configure the deployment process:

+ config-path: (Required)
    + Specifies the path to the Site Builder configuration file, which contains the necessary settings such as package, portal, gas budget and other operational details for deploying the site.
	+ Example: './path/to/config.yaml'
+ site-path: (Required)
	+ Specifies the path to the directory containing the site files to be deployed. This directory must have an index.html file generated by your web framework at the root.
	+ Example: './build'
+ network: (Required)
	+ Specifies the network environment to use for the build. Currently only the testnet is supported.
	+ Options:
        + testnet (default)
	+ Example: 'testnet'
+ object-id: (optional)
  + Specifies the hex-string object ID of the existing site to be updated. If specified, the action will update the existing site instead of publishing a new one. Leave this field blank to publish a new site.

### Environment Variables

The following environment variables should be provided securely using GitHub Secrets to protect sensitive information:

+ SUI_ADDRESS:
    + The Sui blockchain address used to provision the site. This should be a valid hexadecimal string (e.g. 0x...).

+ SUI_KEYSTORE
    + The contents of the Sui keystore file containing the private keys required to sign transactions on the Sui blockchain. This keystore must be formatted according to the Sui binary specifications:
      + The keystore is a JSON array of base64 encoded strings.
      + Each entry in the array represents a key.
      + When base64 decoded, each key is 33 bytes in length.
      + The first byte of the decoded key indicates the key type, followed by a 32-byte private key seed.
    + [Sui Keytool CLI](https://docs.sui.io/references/cli/keytool)
    ```bash
      sui keytool generate ed25519
    ```

### Example Usage

Below is an example of how to use this GitHub action in a workflow file to deploy a site to Walrus Sites:

```yaml
name: Deploy site to Walrus
on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Deploy site to Walrus
        uses: zktx-io/walrus-ga@v0.1.1
        with:
          config-path: './path/to/config.yaml'
          site-path: './build'
          network: 'testnet'
        env:
          SUI_ADDRESS: ${{ var.SUI_ADDRESS }}
          SUI_KEYSTORE: ${{ secrets.SUI_KEYSTORE }}
```

### Example Github

[walrus-sites-ga-example](https://github.com/zktx-io/walrus-sites-ga-example)
