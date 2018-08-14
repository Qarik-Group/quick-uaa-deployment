# Quaa - Quick UAA deployment to any Cloud

```plain
quaa up
```

This project is dedicated to making it easy to bring up secure, production-ready UAA on a single VM locally or on any cloud supported by BOSH, and to upgrade it in future. You do not need to have BOSH already installed; instead we use the standalone `bosh create-env` command.

This project is hugely influenced by, and code/files copied from, [BUCC](https://github.com/starkandwayne/bucc).

The name of the helper application is `quaa` for "Quick UAA". Quick and production-ready UAA.

To install this project, its `quaa` helper CLI, and the `uaa` and `bosh` CLIs:

```plain
git clone https://github.com/starkandwayne/quick-uaa-deployment ~/workspace/quick-uaa-deployment
cd ~/workspace/quick-uaa-deployment
eval "$(bin/quaa env)"
```

Alternately, see [Offline Download](#offline-download) section to download a 900+Mb tarball containing the CLIs, BOSH releases, and stemcell for VirtualBox deployment.

To quickly bootstrap UAA inside VirtualBox:

```plain
quaa up
```

Note, the instructions above will download approximately 600-900Mb of CLIs, BOSH releases, and BOSH stemcells on your first time.

To quickly bootstrap UAA on AWS, Google, or Azure respectively:

```plain
quaa up --cpi aws
quaa up --cpi google
quaa up --cpi azure
```

To target and authorize the [`uaa` CLI](https://github.com/cloudfoundry-incubator/uaa-cli):

```plain
quaa auth-client
```

To use the `quaa` and `uaa` CLIs from outside the `uaa-deployment` folder, source the `quaa env` output:

```plain
eval "$(~/workspace/quick-uaa-deployment/bin/quaa env)"
uaa clients
```

The `quaa env` output will setup various environment variables that you can use with your client applications during local development:

```plain
$ quaa env
export PATH="/Users/drnic/workspace/quick-uaa-deployment/bin:..."
export UAA_URL=https://192.168.50.6:8443
export UAA_CA_CERT='-----BEGIN CERTIFICATE-----
MIIDFDCCAfygAwIBAgIRAPvv3CgQ/brgiaLZx9oozVQwDQYJKoZIhvcNAQELBQAw
...
ZUe/2EzGxceqNSAq99YvFPPI0GdlSkTb
-----END CERTIFICATE-----'
export UAA_CA_CERT_FILE='/var/folders/wd/xnncwqp96rj0v1y2nms64mq80000gn/T/tmp.lDvhJEpT/ca.pem'
```

Confirm that `$UAA_URL` points to your UAA, and that `$UAA_CA_CERT_FILE` is its custom root CA:

```plain
eval "$(~/workspace/quick-uaa-deployment/bin/quaa env)"
curl --cacert $UAA_CA_CERT_FILE -H "Accept: application/json" $UAA_URL/login
```

As an aside, there are also `quaa url` and `quaa cacert` helpers:

```plain
curl --cacert $(quaa cacert) -H 'Accept: application/json' $(quaa url)/login
```

The JSON output might look similar to:

```json
{"app":{"version":"4.19.0"},"links":{"uaa":"https://192.168.50.6:8443","passwd":"/forgot_password","login":"https://192.168.50.6:8443","register":"/create_account"},"zone_name":"uaa","entityID":"192.168.50.6:8443","commit_id":"7897100","idpDefinitions":{},"prompts":{"username":["text","Email"],"password":["password","Password"]},"timestamp":"2018-06-13T12:02:09-0700"}
```

## Example client applications

You can find a selection of example client applications at:

* https://github.com/starkandwayne/ultimate-guide-to-uaa-examples

## Offline Download

The instructions above will progressively download any missing CLIs, BOSH releases, and BOSH stemcell. On your first time this can add up to almost 1G. If you need to download everything at once and then proceed with the deployment at a later time we are publishing an offline tarball via CDN.

To discover the latest offline tarball, and download it:

```plain
curl -s https://raw.githubusercontent.com/starkandwayne/quick-uaa-deployment/master/bin/download-latest-offline | bash
```

To unpack it:

```plain
mkdir -p ~/workspace/quick-uaa-deployment
tar xfz uaa-deployment-offline-*.tar.gz -C ~/workspace/quick-uaa-deployment
```

You can now use the directory `~/workspace/quick-uaa-deployment` as per the rest of the article above.

```plain
cd ~/workspace/quick-uaa-deployment
eval "$(bin/quaa env)"
quaa up
```

Currently the offline download includes CLIs for both Linux & Darwin, but assumes you are deploying to VirtualBox. If you ultimately target a different CPI then the `quaa up` command will download the missing CPI and stemcell files.

Please create an issue if you would like us to publish additional offline tarballs for your target CPI.