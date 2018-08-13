# U - UAA deployment with BOSH CLI

```plain
u up
```

This project is dedicated to making it easy to bring up secure, production-ready UAA on a single VM locally or on any cloud supported by BOSH, and to upgrade it in future. You do not need to have BOSH already installed; instead we use the standalone `bosh create-env` command.

This project is hugely influenced by, and code/files copied from, [BUCC](https://github.com/starkandwayne/bucc).

The name of the helper application is `u`. This name comes from this project's precessor [BUCC](https://github.com/starkandwayne/bucc), with its helper app `bucc`, which stood for BOSH-UAA-CredHub-Concourse. Since this project only deploys the UAA, we appropriately shorten the helper name to `u`.

To install this project, its `u` helper CLI, and the `uaa` and `bosh` CLIs:

```plain
git clone https://github.com/starkandwayne/uaa-deployment ~/workspace/uaa-deployment
cd ~/workspace/uaa-deployment
eval "$(bin/u env)"
```

To bootstrap UAA inside VirtualBox:

```plain
u up
```

To bootstrap UAA on AWS:

```plain
u up --cpi aws
```

To target and authorize the [`uaa` CLI](https://github.com/cloudfoundry-incubator/uaa-cli):

```plain
u auth-client
```

To use the `u` and `uaa` CLIs from outside the `uaa-deployment` folder, source the `u env` output:

```plain
eval "$(~/workspace/uaa-deployment/bin/u env)"
uaa clients
```

The `u env` output will setup various environment variables that you can use with your client applications during local development:

```plain
$ u env
export PATH="/Users/drnic/workspace/uaa-deployment/bin:..."
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
eval "$(~/workspace/uaa-deployment/bin/u env)"
curl --cacert $UAA_CA_CERT_FILE -H "Accept: application/json" $UAA_URL/login
```

The JSON output might look similar to:

```json
{"app":{"version":"4.19.0"},"links":{"uaa":"https://192.168.50.6:8443","passwd":"/forgot_password","login":"https://192.168.50.6:8443","register":"/create_account"},"zone_name":"uaa","entityID":"192.168.50.6:8443","commit_id":"7897100","idpDefinitions":{},"prompts":{"username":["text","Email"],"password":["password","Password"]},"timestamp":"2018-06-13T12:02:09-0700"}
```

## Example client applications

You can find a selection of example client applications at:

* https://github.com/starkandwayne/ultimate-guide-to-uaa-examples
