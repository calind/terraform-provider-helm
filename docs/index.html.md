---
layout: "helm"
page_title: "Provider: Helm"
sidebar_current: "docs-helm-index"
description: |-
  The Helm provider is used to deploy software packages in Kubernetes. The provider needs to be configured with the proper credentials before it can be used.
---

# Helm Provider

The Helm provider is used to deploy software packages in Kubernetes. The provider needs to be configured with the proper credentials before it can be used.

Use the navigation to the left to read about the available resources.

## Example Usage

```hcl
resource "helm_chart" "my_databsase" {
    name      = "my_databsase"
    chart     = "stable/mariadb"

    value {
        name = "mariadbUser"
        content = "foo"
    }

    value {
        name = "mariadbPassword"
        content = "qux"
    }
}
```

## Requirements

- You must have Kubernetes installed. We recommend version 1.4.1 or later.
- You should also have a local configured copy of kubectl.
- You should also have a local configured copy of [helm](https://docs.helm.sh/using_helm/#installing-helm).
- You must have [Tiller](https://docs.helm.sh/using_helm/#initialize-helm-and-install-tiller) installed on Kubernetes.

## Authentication

There are generally two ways to configure the Helm provider.

### File config

The provider always first tries to load **a config file** (usually `$HOME/.kube/config`), for access kubenetes and reads all the Helm files from home (usually `$HOME/.helm`).

### Statically defined credentials

The other way is **statically** define all the credentials:

```hcl
provider "helm" {
    kubernetes {
        host     = "https://104.196.242.174"
        username = "ClusterMaster"
        password = "MindTheGap"

        client_certificate     = "${file("~/.kube/client-cert.pem")}"
        client_key             = "${file("~/.kube/client-key.pem")}"
        cluster_ca_certificate = "${file("~/.kube/cluster-ca-cert.pem")}"
    }
}
```

If you have **both** valid configuration in a config file and static configuration, the static one is used as override.
i.e. any static field will override its counterpart loaded from the config.

## Argument Reference

The following arguments are supported:

* `host` - (Required) Set an alternative Tiller host. The format is host:port. Can be sourced from `HELM_HOST`.
* `home` - (Required) Set an alternative location for Helm files. By default, these are stored in '$HOME/.helm'. Can be sourced from `HELM_HOME`.
* `namespace` - (Optional) Set an alternative Tiller namespace.
* `debug` - (Optional)
* `plugins_disable` - (Optional) Disable plugins. Set HELM_NO_PLUGINS=1 to disable plugins.

* `insecure` - (Optional) Whether server should be accessed without verifying the TLS certificate. Can be sourced from `HELM_HOME`.
* `client_key` - (Optional) PEM-encoded client certificate key for TLS authentication. By default read from `$HELM_HOME/key.pem`.
* `client_certificate` - (Optional) PEM-encoded client certificate for TLS authentication. By default read from `$HELM_HOME/cert.pem`.
* `ca_certificate` - (Optional) PEM-encoded root certificates bundle for TLS authentication. CBy default read from `$HELM_HOME/ca.pem`.
* `kubernetes` - Kubernetes configuration.

The `kubernetes` block supports:

* `config_path` - (Optional) Path to the kube config file, defaults to `~/.kube/config`. Can be sourced from `KUBE_CONFIG`.
* `host` - (Optional) The hostname (in form of URI) of Kubernetes master. Can be sourced from `KUBE_HOST`.
* `username` - (Optional) The username to use for HTTP basic authentication when accessing the Kubernetes master endpoint. Can be sourced from `KUBE_USER`.
* `password` - (Optional) The password to use for HTTP basic authentication when accessing the Kubernetes master endpoint. Can be sourced from `KUBE_PASSWORD`.
* `insecure` - (Optional) Whether server should be accessed without verifying the TLS certificate. Can be sourced from `KUBE_INSECURE`.
* `client_certificate` - (Optional) PEM-encoded client certificate for TLS authentication. Can be sourced from `KUBE_CLIENT_CERT_DATA`.
* `client_key` - (Optional) PEM-encoded client certificate key for TLS authentication. Can be sourced from `KUBE_CLIENT_KEY_DATA`.
* `cluster_ca_certificate` - (Optional) PEM-encoded root certificates bundle for TLS authentication. Can be sourced from `KUBE_CLUSTER_CA_CERT_DATA`.
* `config_context` - (Optional) Context to choose from the config file. Can be sourced from `KUBE_CTX`.






