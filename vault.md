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
vault -h | General help
vault CMD -h | Help on a subcommand
vault path-help PATH | Help on a `PATH`
vault auth help github | Help on auth method

#### Read/Write Secrets

CLI | Description
:-- | :-- | :--
vault kv put secret/password value=itsasecret | Write a secret from command line
echo -n '{"value":"itsasecret"}' &#448; vault kv put secret/password - | Write a JSON
echo -n "itsasecret" &#448; vault kv put secret/password value=- | Write plain text from STDIN
vault kv put secret/password value=@data.txt| Write from a File
vault kv get secret/password | Read
vault kv get -version=1 -format=yaml secret/password | Output in yaml of version 1

#### Secrets Engines

CLI | Description
-- | --
vault secrets list | List existing secrets engines
vault secrets enable -path=PATH ENGINE | Enable a secret engine at the provided path, or by default it will be accessible with its name
vault write PATH/... key=value key=value  | Write a secret to a specific PATH which is backed by the declared secret engine.

#### Authentication

##### Tokens

##### Auth

CLI | Description
-- | --
vault auth list | list auth methods
vault token revoke -mode path auth/METHOD | revoke any logins from method
vault auth disable METHOD | Disable auth method


CLI | Description
-- | --
vault token create | Create a token which is a child of the current one.
vault login TOKEN | Login using a token
vault token revoke TOKEN | Revoke a token, will also revoke all of its child.
vault token renew TOKEN | Renew lease

##### GitHub

CLI | Description
-- | --
vault auth enable -path=github github | To enable [GitHub](https://www.vaultproject.io/docs/auth/github.html) auth method
vault write auth/github/config organization=my-company | Configure it
GitHub &gt; Setting &gt; Developper settings &gt; Personal access tokens > Generate new token| Create you own [personnal access token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/), at least allow read:org
vault login -method=github token=2046...e9307d | login
vault token revoke -mode path auth/github | revoke GitHub auth
vault auth disable github | disable it

#### Authz, Policies

Apart from pre-existing root and default policies, you can create new ones using HashiCorp Configuration Language.

CLI | Description
-- | --
vault policy list | list existing policies
vault policy fmt policy.hcl | format and check policy
vault policy write my-policy acl.hcl | write a policy
vault policy read my-policy | view policy content
vault token create -policy=my-policy | assign policy to a token at creation time
vault write auth/github/map/teams/default value=my-policy | map policy to GitHub apply to everybody.

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

    vault server -dev

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

    storage "consul" {
      address = "127.0.0.1:8500"
      path    = "vault/"
    }
    
    listener "tcp" {
     address     = "127.0.0.1:8200"
     tls_disable = 1
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
--- 

Cheatsheet freely available on [GitHub](https://github.com/planetrobbie/cheatsheets/vault.md), created by planetrobbie.

