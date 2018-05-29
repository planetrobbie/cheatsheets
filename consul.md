{
    "id": "auto",
    "style": "css/h4dark.css",
    "layout": "default"
}
%%%END

# HashiCorp Consul

#### Help
consul -h | General Help
consul CMD -h | Help on a subcommand

#### Read/Write Key/Values

CLI | Description
-- | --
consul kv get -recurse redis/ | Treat the path as a prefix and list all keys which start with the given prefix
consul kv get -keys |  List keys which start with the given prefix

#### Clustering

CLI | Description
-- | --
consul members -detailed | local view of the cluster members
curl localhost:8500/v1/catalog/nodes | strongly consistent view of the cluster members

#### Versions

Open Source | Pro | Premium
:-- | :-- | :--
Multi-datacenter service discovery (DNS + HTTP) | [Automated backups](https://www.consul.io/docs/enterprise/backups/index.html) | [Redundancy zone](https://www.consul.io/docs/enterprise/redundancy/index.html)
Health checks | [Automated upgrades](https://www.consul.io/docs/enterprise/upgrades/index.html) | [Advanced federation for complex network topologies](https://www.consul.io/docs/enterprise/federation/index.html)
Key/Value storage | [Enhanced read scalability](https://www.consul.io/docs/enterprise/read-scale/index.html) | Gold Support: 24x7 support w/ SLA
Runtime configuration (Consul Template) | [Network segments](https://www.consul.io/docs/enterprise/network-segments/index.html) |
Runtime orchestration | Silver Support: 9x5 support w/ SLA |
Built-in web UI for editing K/V and viewing health check status |

#### Installation

Install GnuPG on your platform

macOS | ArchLinux | Debian
-- | -- | --
brew install gpg | pacman -S gnupg | apt-get install gnupg

On Debian systems you'll also need the following package which brings shasum and unzip used during installation

    apt-get install libdigest-sha-perl unzip

Import HashiCorp PGP public key

    curl https://keybase.io/hashicorp/pgp_keys.asc | gpg --import

Download Consul binary, shasum and shasum signature from [consul.io](https://www.consul.io/downloads.html).

    curl -O https://releases.hashicorp.com/consul/&lt;VERSION&gt;/consul_&lt;VERSION&gt;_&lt;OS&gt;_amd64.zip
    curl -O https://releases.hashicorp.com/consul/&lt;VERSION&gt;/consul_&lt;VERSION&gt;_SHA256SUMS
    curl -O https://releases.hashicorp.com/consul/&lt;VERSION&gt;/consul_&lt;VERSION&gt;_SHA256SUMS.sig

Replace `&lt;VERSION&gt;`, and `&lt;OS&gt;` with your os name like:  *darwin, linux, freebsd, netbsd, openbsd, solaris or windows*

Verify the authenticity of the shasum signature

    gpg --verify consul_&lt;VERSION&gt;_SHA256SUMS.sig consul_&lt;VERSION&gt;_SHA256SUMS

Make also sure the key fingerprint is

    91A6 E7F8 5D05 C656 30BE F189 5185 2D87 348F FC4C.

Check shasum of your downloaded zip file
    
    shasum -a 256 -c consul_&lt;VERSION&gt;_SHA256SUMS

If everything looks good you can safely move consul binary to your PATH

    unzip consul_&lt;VERSION&gt;_<OS>_amd64.zip
    mv consul /usr/local/bin

Check installation

    consul version

Complete the installation by activating the autocomplete feature

    consul -autocomplete-install

#### Configuration

XXX See longer article.

#### Consul Devt Agent

To start a development agent, in this mode you'll have a single node environment quickly and easily. But it is just for learning and testing, for sure not for production.

    consul agent -dev -ui

You can access Consul Web UI

    http://localhost:8500/ui

#### Raft

    consul operator raft list-peers