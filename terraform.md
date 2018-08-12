{
    "id": "auto",
    "style": "css/h4dark.css",
    "layout": "default"
}
%%%END

# HashiCorp Terraform

#### [Providers](http://terraform.io/docs/configuration/providers.html)

    provider "google" {
      region      = "${var.region}"
      project     = "${var.project_name}"
      credentials = "${file(var.account_file_path)}"
    }

#### [Variables](http://terraform.io/docs/configuration/variables.html)

    variable "region" {
      description = "In which region to deploy infrastructure"
      default = "europe-west1"
    }

    variable "ami" {
      description = "image to use depend on region"
      type = "map"
      default = {
        us-east-1 = "ami-0d729a60"
        us-west-1 = "ami-7e44331c"
      }
    }

##### [Variables precedence](http://terraform.io/docs/configuration/variables.html#variable-precedence)

    CLI -> FILE -> ENV -> Default
    -var "region=europe-west1" -> terraform.tfvars -> TF_VAR_REGION = europe-west1


#### [Resources](http://terraform.io/docs/configuration/resources.html)

    resource "google_storage_bucket" "mybucket" {
      name = "sebbraun-yet"
      location = "EU"
    }

##### [meta parameters](http://terraform.io/docs/configuration/resources.html#meta-parameters)
Item | type | Description
-- | -- | --
count | int | number of identical resources to create
depends_on | list of strings | explicit dependencies declaration, should be avoided
provider  | string | name of a specific provider to use
lifecycle | configuration block | can contain create_before_destroy, prevent_destroy, ignore_changes

##### [timeouts](http://terraform.iog/docs/configuration/resources.html#timeouts)

Amount of time a specific operation is allowed to take before being considered an error

    # ...
    timeouts {
      create = "60m"
      delete = "2h"
    }

`PROVIDER.RESOURCE_TYPE.NAME` needs to be unique in config

#### [Outputs](http://terraform.io/docs/configuration/outputs.html)

    output "addresses" {
      description = "instance FQDN"
      value = ["${aws_instance.web.*.public_dns}"]
    }

#### [Locals](http://terraform.io/docs/configuration/locals.html)

    locals {
      default_name_prefix = "${var.project_name}-web"
      name_prefix         = "${var.name_prefix != "" ? var.name_prefix : local.default_name_prefix}"
    }

#### [Modules](http://terraform.io/docs/configuration/modules.html)

    module "mig1" {
      source            = "replicated.yet.org/yet/managed-instance-group/google"
      version           = "1.1.11"
      region            = "europe-west2"
      zone              = "europe-west2-b"
      network           = "${var.network}"
      name              = "group1"
      size              = "${var.group1_size}"
      target_tags       = ["allow-group1"]
      service_port      = 80
      service_port_name = "http"
      startup_script    = "${data.template_file.group-startup-script.rendered}"
    }

#### [Overrides](http://terraform.io/docs/configuration/override.html)

Loaded last and merged into your configuration, named `override` or `WHATEVER_override.tf` or `WHATEVER_override.tf.json`

#### HCL
Item | Description
-- | --
key = value | assignement
# // /\* \*/ | comments
["apple", "peach"] | arrays
service { key = value } | list
${google_compute_instance.vm.id} | resource value
${var.region_zone} | variable [interpolation](http://terraform.io/docs/configuration/interpolation.html)
${lookup(var.ami,var.region,[default])} | lookup for a key value in a map
${var.ami["us-west-1"]} | lookup for a key value in a map 
${file(var.account_file_path)} | output a file content
${self.private_ip} | interpolate to that resource private_ip, usable only within provisionners.
${data.aws_subnet.example.*.cidr_block} | list all the attribute of the [data source](http://terraform.io/docs/configuration/data-sources.html) using the splat syntax
${module.foo.bar} | bar output of the foo module
${count.index} | current index in a multi-count resource
CONDITION ? TRUEVAL : FALSEVAL | conditional
${data.template_file.example.rendered} | render template
${local.name_prefix}-files | value of local variable.

#### Operations

CLI | Description
-- | --
terraform plan -out epoch-\`date +'%s'\`.plan | Plan with epoch
terraform apply epoch-1534094289 | Builds or changes infrastructure according to Plan or config files in current dir.
terraform destroy | Destroy Terraform-managed infrastructure.

#### [Environment Variables](http://terraform.io/docs/configuration/environment-variables.html)

ENV Variable | Description
:-- | :--
TF_LOG |  set to TRACE, DEBUG, INFO, WARN or ERROR to enable detailed logs to appear on stderr
TF_LOG_PATH | where the log should persist its output to
TF_INPUT=0 | disable prompts for variables that haven't had their values specified
TF_MODULE_DEPTH=0 | enable commands such as plan and graph to display more compressed information.
TF_VAR_name | used to set variables
TF_CLI_ARGS, TF_CLI_ARGS_name | specify additional arguments to the command-line
TF_DATA_DIR | changes the location where Terraform keeps its per-working-directory data
TF_SKIP_REMOTE_TESTS | opt-out of any tests requiring remote network connectivity when running unit tests.
CHECKPOINT_DISABLE=true | to disable checkpoint 

#### Versions

Open Source | Pro | Premium
:-- | :-- | :--
Infrastructure as code | All OSS capabilities | All OSS & Pro capabilities
Separation of infrastructure plan and apply | Granular infrastructure management with Workspace | Policy as code management with Sentinel
Multi-provider support | Modern, responsive GUI for simple and safe collaboration workflows | SSO with SAML
Public module registry for reusable configurations | Service catalog to publish and consume infrastructure modules | Track all infrastructure changes with audit logs
| Remote plans, applies, state storage, locking, and rollback rollback | 
| Version control connection |
| Secure variable management |
| SaaS Install | SaaS, Private Install
| Silver support 9x5 | Gold Support 24x7

#### Tips & Tricks

CLI | Description
-- | --
terraform graph XXXPIPE dot -Tpng > graph.png | Graph config

#### Installation

Install GnuPG on your platform

macOS | ArchLinux | Debian
-- | -- | --
brew install gpg | pacman -S gnupg | apt-get install gnupg

On Debian systems you'll also need the following package which brings shasum and unzip used during installation

    apt-get install libdigest-sha-perl unzip

Import HashiCorp PGP public key

    curl https://keybase.io/hashicorp/pgp_keys.asc | gpg --import

Download Terraform binary, shasum and shasum signature from [terraform.io](https://www.terraform.io/downloads.html).

    curl -O https://releases.hashicorp.com/terraform/&lt;VERSION&gt;/terraform_&lt;VERSION&gt;_&lt;OS&gt;_amd64.zip
    curl -O https://releases.hashicorp.com/terraform/&lt;VERSION&gt;/terraform_&lt;VERSION&gt;_SHA256SUMS
    curl -O https://releases.hashicorp.com/terraform/&lt;VERSION&gt;/terraform_&lt;VERSION&gt;_SHA256SUMS.sig

Replace `&lt;VERSION&gt;`, and `&lt;OS&gt;` with your os name like:  *darwin, linux, freebsd, netbsd, openbsd, solaris or windows*

Verify the authenticity of the shasum signature

    gpg --verify terraform_&lt;VERSION&gt;_SHA256SUMS.sig terraform_&lt;VERSION&gt;_SHA256SUMS

Make also sure the key fingerprint is

    91A6 E7F8 5D05 C656 30BE F189 5185 2D87 348F FC4C.

Check shasum of your downloaded zip file
    
    shasum -a 256 -c terraform_&lt;VERSION&gt;_SHA256SUMS

If everything looks good you can safely move terraform binary to your PATH

    unzip terraform_&lt;VERSION&gt;_<OS>_amd64.zip
    mv terraform /usr/local/bin

Check installation

    terraform version

Complete the installation by activating the autocomplete feature

   terraform -install-autocomplete
   source ~/.zshrc