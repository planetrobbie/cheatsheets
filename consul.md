{
    "id": "auto",
    "style": "css/h4dark.css",
    "layout": "default"
}
%%%END

# HashiCorp Consul

#### Clustering

CLI | Description
-- | --
consul members -detailed | local view of the cluster members
curl localhost:8500/v1/catalog/nodes | strongly consistent view of the cluster members

#### Installation

Install GnuPG on your platform

macOS | ArchLinux | Debian
-- | -- | --
brew install gpg | pacman -S gnupg | apt-get install gnupg

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

    unzip consul_&lt;VERSION&gt;_darwin_amd64.zip
    mv consul /to/your/path

#### Consul Devt Agent

To start a development agent, in this mode you'll have a single node environment quickly and easily. But it is just for learning and testing, for sure not for production.

    consul agent -dev
