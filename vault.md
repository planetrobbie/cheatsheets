{
    "id": "auto",
    "style": "css/h4dark.css",
    "layout": "default"
}
%%%END

# HashiCorp Vault

#### Help

CLI | Description
:-- | :-- | :--
vault -h | General Help
vault CMD -h | Help on a subcommand
vault path-help PATH | Help on a PATH
vault auth help METHOD | Help on auth method

#### Operations

CLI | Description
:-- | :-- | :--
vault operator init | Initializes a server
vault operator unseal | Unseals the Vault server
vault operator seal | Seals the Vault server
vault status | Output current state

#### Secrets Engines

CLI | Description
-- | --
vault secrets list | List existing secrets engines
vault secrets enable [-path=PATH] SECRET_ENGINE | Enable a secret engine at the provided path, or by default it will be accessible with its name
vault write PATH key=value key=value  | Write a secret to a specific PATH which is backed by the declared secret engine.
vault read PATH | Read from a secret engine at PATH

#### Read/Write Secrets

CLI | Description
:-- | :-- | :--
vault kv put secret/password value=itsasecret | Write a secret from command line
echo -n '{"value":"itsasecret"}' &#448; vault kv put secret/password - | Write a JSON
echo -n "itsasecret" &#448; vault kv put secret/password value=- | Write plain text from STDIN
vault kv put secret/password value=@data.txt| Write from a File
vault kv get secret/password | Read
vault kv get -version=1 -format=yaml secret/password | Output in yaml of version 1

#### Authentication

##### Tokens

CLI | Description
-- | --
vault token create | Create a token which is a child of the current one.
vault token renew TOKEN | Renew lease
vault token revoke -mode path auth/METHOD | revoke any logins from method
vault token revoke TOKEN | Revoke a token, will also revoke all of its child.
vault login -method=userpass DATA | Login to Vault using the specified method.

##### Auth

CLI | Description
-- | --
vault auth list | list auth methods
vault auth [-path=PATH] enable METHOD | Enable auth method
vault auth disable METHOD | Disable auth method

#### Auth methods

Method (api/cli name) | Authenticate using
:-- | :-- | :--
[approle](https://www.vaultproject.io/docs/auth/approle.html) | role_id, secret_id
[aws](https://www.vaultproject.io/docs/auth/aws.html) | [AWS IAM credentials](https://www.hashicorp.com/resources/deep-dive-vault-aws-auth-backend), mapping an IAM user or role to a Vault role.
[gcp](https://www.vaultproject.io/docs/auth/gcp.html) | Google credentials
[kubernetes](https://www.vaultproject.io/docs/auth/kubernetes.html) | a Kubernetes Service Account Token. Makes it easy to introduce a Vault token into a Kubernetes Pod.
[github](https://www.vaultproject.io/docs/auth/github.html) | a GitHub personal access token
[ldap](https://www.vaultproject.io/docs/auth/ldap.html) | an existing LDAP server and user/password credentials. Groups and Users can be mapped to Vault policies.
[okta](https://www.vaultproject.io/docs/auth/okta.html) | Okta and user/password credentials. Groups mapped to Vault policies.
[radius](https://www.vaultproject.io/docs/auth/radius.html) | an existing RADIUS server that accepts the PAP authentication scheme.
[cert](https://www.vaultproject.io/docs/auth/cert.html) | SSL/TLS client certificates which are either signed by a CA or self-signed.
[token](https://www.vaultproject.io/docs/auth/token.html) | a token. Built-in method.
[userpass](https://www.vaultproject.io/docs/auth/userpass.html) | a username and password

##### GitHub

CLI | Description
-- | --
vault write auth/github/config organization=my-company | Configure GitHub auth
GitHub &gt; Setting &gt; Developper settings &gt; Personal access tokens > Generate new token| Create you own [personnal access token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/), at least allow read:org
vault login -method=github token=2046...e9307d | login

#### [Policies](https://www.vaultproject.io/guides/identity/policies.html)

Apart from pre-existing root and default policies, you can create new policies using HashiCorp Configuration Language.

CLI | Description
-- | --
vault policy list | list existing policies
vault policy fmt policy.hcl | format and check policy
vault policy write my-policy acl.hcl | write a policy
vault policy read my-policy | view policy content
vault token create -policy=my-policy | assign policy to a token at creation time
vault write auth/github/map/teams/default value=my-policy | map policy to GitHub apply to everybody.

Capabilities: create, read, update, delete, list, sudo, deny

#### Audit Devices

Audit Devices](https://www.vaultproject.io/docs/audit/index.html) gives you a pluggable way to keep a detailed log of all requests and response to Vault. You'll find the full request and response for every interaction, but secrets will be hashed using HMAC-SHA256

CLI | Description
-- | --
vault audit enable file file_path=/var/log/vault_audit.log | enable [file](https://www.vaultproject.io/docs/audit/file.html) Audit Device
vault audit enable syslog tag="vault" facility="AUTH" | enable [Syslog](https://www.vaultproject.io/docs/audit/syslog.html) Audit Device
vault audit enable socket address=127.0.0.1:9090 socket_type=tcp | enable [Socket](https://www.vaultproject.io/docs/audit/socket.html) Audit Device

#### Environment Variables, CLI Options

ENV Variable | Description | CLI Options
:-- | :-- | :--
VAULT_TOKEN | Vault authentication token |
VAULT_ADDR | Vault server URL and Port | -address=...
VAULT_CACERT | Path to a PEM-encoded CA certificate file to verify Vault server certificate | -ca-cert=...
VAULT_CAPATH | Path to a directory of PEM-encoded CA certificate files | -ca-path=... 
VAULT_CLIENT_CERT | Path to a PEM-encoded client certificate | -client-cert=...
VAULT_CLIENT_KEY | Path to an unencrypted, PEM-encoded private key | 
VAULT_CLIENT_TIMEOUT | Timeout variable, default 60s |
VAULT_CLUSTER_ADDR | Used by other cluster members to connect to this node when in HA mode |
VAULT_MAX_RETRIES | Max retries when 5xx error received. default 2 |
VAULT_REDIRECT_ADDR | clients are redirected to this address when in HA mode. |
VAULT_SKIP_VERIFY | Do not verify Vault certificate, not recommended. | -tls-skip-verify | 
VAULT_TLS_SERVER_NAME | Name to use as the server name (SNI) host when connecting via TLS. | -tls-server-name=...
VAULT_CLI_NO_COLOR | supress ANSI color escape sequence characters from Vault output |
VAULT_WRAP_TTL | Wraps the response in a cubbyhole token with the requested TTL. The response is available via the "vault unwrap" command. |
VAULT_MFA | mfa_method_name[:key[=value]] for Multi-factor authentication, enterprise only ! | -mfa=...

#### Storage Backends

Backend | Highly Available? | Support
:-- | :-- | :--
[Azure](https://www.vaultproject.io/docs/configuration/storage/azure.html) | No | Commumity
[CockroachDB](https://www.vaultproject.io/docs/configuration/storage/cockroachdb.html) | No | Community
[Consul](https://www.vaultproject.io/docs/configuration/storage/consul.html) | Yes | HashiCorp
[CouchDB](https://www.vaultproject.io/docs/configuration/storage/couchdb.html) | No | Community
[DynamoDB](https://www.vaultproject.io/docs/configuration/storage/dynamodb.html) | Yes | Community
[Etcd](https://www.vaultproject.io/docs/configuration/storage/etcd.html) | Yes | Community
[Filesystem](https://www.vaultproject.io/docs/configuration/storage/filesystem.html) | No | HashiCorp
[Google Cloud Storage](https://www.vaultproject.io/docs/configuration/storage/google-cloud-storage.html) | Yes | Community
[Google Cloud Spanner](https://www.vaultproject.io/docs/configuration/storage/google-cloud-spanner.html) | Yes | Community
[In-Memory](https://www.vaultproject.io/docs/configuration/storage/in-memory.html) | No | HashiCorp, good for testing
[Manta](https://www.vaultproject.io/docs/configuration/storage/manta.html) | No | Community
[MySQL](https://www.vaultproject.io/docs/configuration/storage/mysql.html) | No | Community
[PostgreSQL](https://www.vaultproject.io/docs/configuration/storage/postgresql.html) | No | Community
[Cassandra](https://www.vaultproject.io/docs/configuration/storage/cassandra.html) | No | Community
[S3](https://www.vaultproject.io/docs/configuration/storage/s3.html) | No | Community
[Swift](https://www.vaultproject.io/docs/configuration/storage/swift.html) | No | Community
[Zookeeper](https://www.vaultproject.io/docs/configuration/storage/zookeeper.html) | Yes | Community

#### Versions

Open Source | Pro | Premium
:-- | :-- | :--
[Secure storage](https://www.vaultproject.io/docs/secrets/index.html) | Disaster Recovery Replication | [HSM Autounseal](https://www.vaultproject.io/docs/enterprise/hsm/index.html)
[Key Rolling](https://www.vaultproject.io/docs/internals/rotation.html) | [UI with cluster management](https://www.vaultproject.io/docs/enterprise/ui/index.html) | [Performance Replication](https://www.vaultproject.io/docs/enterprise/replication/index.html)
[Credential leasing & revocation](https://www.vaultproject.io/docs/concepts/lease.html) | Init and unseal workflow | Mount Filters
[Detailed Audit Logs](https://www.vaultproject.io/docs/audit/index.html) | AWS KMS Auto-unseal | [Multi-Factor Authentication](https://www.vaultproject.io/docs/enterprise/mfa/index.html)
[Secure Plugins](https://www.vaultproject.io/docs/internals/plugins.html) | GCP Cloud KMS Auto-unseal | [Sentinel Integration](https://www.hashicorp.com/sentinel)
[Access control policies](https://www.vaultproject.io/docs/concepts/policies.html) | Silver support: 9x5 support w/ SLA | Gold support: 24x7 support w/ SLA
[Encryption as a service](https://www.vaultproject.io/docs/secrets/transit/index.html) | | [Seal Wrap / FIPS 140-2 Compliance](https://www.hashicorp.com/vault-compliance)
Identities (Entities + Groups) | | [Control Groups](http://www.vaultproject.com/docs/enterprise/control-groups/index.html)

#### Namespaces

[Multitenancy](https://www.vaultproject.io/guides/operations/multi-tenant.html) within vault, isolate: Secret Engines, Auth Methods, Identities (Entities and Identity Groups), Policies, Tokens, 

CLI | Description
:-- | :--
vault namespace create education | create a namespace
vault namespace create -namespace=education training | create a child namespace
vault namespace list | list namespaces
vault namespace list -namespace=education | list child namespaces
export VAULT_NAMESPACE=education | target a namespace
unset VAULT_NAMESPACE | leave namespace

#### Installation

Install GnuPG on your platform

macOS | ArchLinux | Debian
-- | -- | --
brew install gpg | pacman -S gnupg | apt-get install gnupg

Import HashiCorp PGP public key

    curl https://keybase.io/hashicorp/pgp_keys.asc | gpg --import

Download Vault binary, shasum and shasum signature from [vaultproject.io](https://www.vaultproject.io/downloads.html).

    curl -O https://releases.hashicorp.com/vault/&lt;VERSION&gt;/vault_&lt;VERSION&gt;_&lt;OS&gt;_amd64.zip
    curl -O https://releases.hashicorp.com/vault/&lt;VERSION&gt;/vault_&lt;VERSION&gt;_SHA256SUMS
    curl -O https://releases.hashicorp.com/vault/&lt;VERSION&gt;/vault_&lt;VERSION&gt;_SHA256SUMS.sig

Replace `&lt;VERSION&gt;`, and `&lt;OS&gt;` with your os name like:  *darwin, linux, freebsd, netbsd, openbsd, solaris or windows*

Verify the authenticity of the shasum signature

    gpg --verify vault_&lt;VERSION&gt;_SHA256SUMS.sig vault_&lt;VERSION&gt;_SHA256SUMS

Make also sure the key fingerprint is

    91A6 E7F8 5D05 C656 30BE F189 5185 2D87 348F FC4C.

Check shasum of your downloaded zip file
    
    shasum -a 256 -c vault_&lt;VERSION&gt;_SHA256SUMS

If everything looks good you can safely move vault binary to your PATH

    unzip vault_&lt;VERSION&gt;_darwin_amd64.zip
    mv vault /to/your/path

Complete the installation by activating the autocomplete feature

    vault -autocomplete-install

#### Vault Devt Server

To start a development server, in this mode Vault runs entirely in-memory
and starts unsealed with a single unseal key. So it is just for learning and testing.

    VAULT_UI=true vault server -dev -dev-root-token-id="root"

In a new terminal, export Vault server address

    export VAULT_ADDR='http://127.0.0.1:8200'

Verify everything looks good

    vault status

Vault client uses a customizable token helper to cache the token after authentication. By default it will be stored at `~/.vault-token`.

#### Vault Server

##### Starting the server
If your server configuration is in `config.hcl`, you can start your server like this

    vault server -config=config.hcl

##### Example configuration

    ui = true
    
    listener "tcp" {
     address     = "0.0.0.0:8200"
     tls_cert_file = "/etc/letsencrypt/live/DOMAIN/fullchain.pem"
     tls_key_file = "/etc/letsencrypt/live/DOMAIN/privkey.pem"
     tls_client_ca_file = "/etc/vault/letsencryptauthorityx3.pem"
    }

    storage "consul" {
      address = "127.0.0.1:8500"
      path    = "vault/"
    }

##### Initialization

Needs to happen once per cluster, following command only works on brand new and empty Vault

    vault operator init

Make sure you distribute the Unseal keys to five person, they need to store them securely, encrypted with their own PGP key at [Keybase](https://www.vaultproject.io/docs/concepts/pgp-gpg-keybase.html). This information won't be stored by Vault, this is your unique opportunity to see it. To unseal To unseal the Vault, you'll need at least three unseal keys !

Vault starts sealed, let's unseal it. It is a stateful process, so it works from multiple computer, provided they connect to the same server.

    vault operator unseal

Repeat this process three time, if keys are correct you should see `Sealed  false`. You can now authenticate using your root token

    vault login TOKEN

In case of an emergency you can reseal it

    vault operator seal

License for Vault Enterprise can be set using

    vault write sys/license text="LICENSE_KEY"

Check license

    vault read sys/license

--- 

Cheatsheet freely available on [GitHub](https://github.com/planetrobbie/cheatsheets/vault.md), created by planetrobbie.

