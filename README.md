# U - UAA deployment with BOSH CLI

This project is dedicated to making it easy to bring up secure, production-ready UAA on a single VM locally or on any cloud supported by BOSH, and to upgrade it in future. You do not need to have BOSH already installed; instead we use the standalone `bosh create-env` command.

This project is hugely influenced by, and code/files copied from, [BUCC](https://github.com/starkandwayne/com).

The name of the helper application is `u`. This name comes from this project's precessor [BUCC](https://github.com/starkandwayne/bucc), with its helper app `bucc`, which stood for BOSH-UAA-CredHub-Concourse. Since this project only deploys the UAA, we appropriately shorten the helper name to `u`.

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

To use the `u` and `uaa` CLIs anywhere, source the `u env` output:

```plain
source <(path/to/uaa-deployment/bin/u env)
uaa clients
```
