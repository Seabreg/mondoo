# Mondoo Terraform Provisioner

This provisioner runs Mondoo vulnerability scan as part of a Terraform apply run. It is specifically designed to assess the initial state of the instance and does not install anything on the machine for continuous assessment. 

Further documentation is available at [Mondoo Terraform Integration Docs](https://mondoo.io/docs/apps/terraform). An AWS build example is located in the [examples directory](../examples/terraform-aws)

## Preparation

1. Install Mondoo agent on your workstation
2. Download and install the Terraform plugin and place it into `~/.terraform.d/plugins`

## Usage

**Vulnerability Assessment at Time of Provisioning**

You want to determine the state of known vulnerabilities at the time of deployment. This plugin is designed for that use case. It uses ssh to detect the operating system and reads the installed packages. No additional tooling needs to be installed.

```
provisioner "mondoo" {
  report = {
    format = "cli"
  }
}
```

**Continuous Vulnerability Assessment**

In many cases, a continuous vulnerability assessment is desired. If you wish to achieve that, we recommend installing the Mondoo agent. It can be easily installed with Terraform by using the `remote-exec` provisioner with our [bash installer](https://mondoo.io/docs/agent/bash).

```
provisioner "remote-exec" {
  inline = [
    "export MONDOO_REGISTRATION_TOKEN='ey..gg'",
    "curl -sSL https://mondoo.io/install.sh | bash"
  ] 
}
```

## Compiling the Terraform plugin from source

If you wish to compile from source, you need to have [Go](https://golang.org/) installed and configured.

1. Clone the mondoo repository from GitHub into your $GOPATH:

```
$ mkdir -p $(go env GOPATH)/src/github.com/mondoolabs && cd $_
$ git clone https://github.com/mondoolabs/mondoo.git
$ cd mondoo/terraform-provisioner-mondoo
```

2. Build the plugin for your current system and place the binary in the packer plugin directory

```
make install
```