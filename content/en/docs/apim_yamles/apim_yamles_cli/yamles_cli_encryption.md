{
"title": "Encrypt YAML configuration using CLI",
"linkTitle": "Encrypt YAML configuration using CLI",
"weight":"70",
"date": "2020-11-11",
"description": "Learn how to use the YAML configuration CLI to encrypt, decrypt, and re-encrypt a YAML configuration."
}

These are the encryption related options available in the YAML CLI:

* `encrypt`: Encrypt an unencrypted YAML configuration, a string, or a file.
* `change-passphrase`: Change the passphrase of a YAML entity store.

## Encrypt an unencrypted YAML configuration

Encryption of XML or YAML configuration means that sensitive fields of type `encrypted` in the Entity Store model are encrypted. All other data remains in the clear. If the XML federated configuration you converted to YAML was not encrypted, it remains unencrypted after conversion. If you wish to deploy the configuration to an API Gateway group that has a group passphrase set, you must encrypt the YAML configuration before deployment.

If you get an error when attempting to encrypt the YAML configuration, that indicates that the passphrase for the configuration is incorrect, it means that the configuration is already encrypted. Use the `change-passphrase` option to change the passphrase.

It is also possible to encrypt a YAML configuration at deployment time via [projdeploy](/docs/apim_reference/devopstools_ref/#projdeploy-command-options).

The following are examples of how you can use the `encrypt` option in the `yamles` CLI to encrypt an unencrypted YAML configuration.

**Example 1**: Specify a directory with passphrase `changeme`:

```
./yamles encrypt --source /home/user/yaml --passphrase changeme
```

**Example 2**: Specify a URL with passphrase `changeme`:

```
./yamles encrypt --source yaml:file:/home/user/apiprojects/myyaml --passphrase changeme
```

**Example 3**: Specify a `.tar.gz` file with passphrase `changeme`:

```
./yamles encrypt --source /home/user/archives/myconfig.tar.gz --passphrase changeme
```

You can run the following help command for more details on each parameter:

```
yamles encrypt --help
```

## Encrypt strings and files to add to an encrypted YAML configuration

If the XML federated configuration you converted to YAML was encrypted with a passphrase, it remains encrypted with the same passphrase after conversion to YAML format. If you wish to add new configuration that includes fields of type `encrypted`, you need to be able to encrypt that data with the same entity store passphrase so that it can be read by the API Gateway when deployed. Say for example you wish to add an entity of type `DbConnection` which has a field `password` which is of type `encrypted`. You must put the encrypted value of the password string into the YAML file for the `DbConnection`. If your database password is `dbpassword` and your passphrase for the YAML configuration is `changeme`.

Use the following command to encrypt the database password string value:

```
./yamles encrypt --text "dbpassword" --passphrase "changeme"
```

The output will be as follows:

```
Your encrypted base64 encoded string content is:-
wWVn7dS/ycwDg7Miqd1TU0YKTiOY//5i
```

Copy and paste the string `wWVn7dS/ycwDg7Miqd1TU0YKTiOY//5i` into your yaml file for your `DbConnection`:

```yaml
---
type: DbConnection
fields:
  url: jdbc:mysql://127.0.0.1:3306/DefaultDb
  name: MySQL/local
  username: root
  password: wWVn7dS/ycwDg7Miqd1TU0YKTiOY//5i
```

A private key is sensitive data that is externalized into a separate file in the YAML configuration. Its data is also of type `encrypted` and so the file content must be encrypted if the YAML configuration is encrypted. A private key file can be encrypted as follows:

```
./yamles encrypt --file /home/user/private-key.der --passphrase "changeme"
```

See section [Add a new certificate and private key to a YAML configuration](/docs/apim_yamles/yamles_edit/#add-a-new-certificate-and-private-key-to-a-yaml-configuration) for more information regarding adding a certificate and private key to a YAML configuration. Note that `encrypt --file` option may be used to encrypt the content of any type of file. The operation will update the file which will always have binary content after encryption. Note that encrypting a private key via `openssl` and adding it to the YAML configuration is not supported.

## Change the encryption passphrase of a YAML configuration

If you have encrypted your YAML configuration you may wish to encrypt it with a different passphrase. You may also need to decrypt it by setting the `--new-passphrase` to `""`. In order to do this you need to know what the current passphrase is.

It is also possible to change the passphrase of a YAML configuration at deployment time via [projdeploy](/docs/apim_reference/devopstools_ref/#projdeploy-command-options).

The following are examples of how you can use the `change-passphrase` option in the `yamles` CLI to change the passphrase of a YAML configuration:

**Example 1**: Specify a directory from `changeme` to `newpassphrase`:

```
./yamles change-passphrase --source /home/user/yaml --old-passphrase changeme --new-passphrase newpassphrase
```

**Example 2**: Specify a URL from `changeme` to `newpassphrase`:

```
./yamles change-passphrase --source yaml:file:/home/user/apiprojects/myyaml --old-passphrase changeme --new-passphrase newpassphrase
```

**Example 3**: Specify `.tar.gz` file from `changeme` to `newpassphrase`:

```
./yamles change-passphrase --source /home/user/archives/myconfig.tar.gz --old-passphrase changeme --new-passphrase newpassphrase
```

**Example 4**: Decrypt a YAML configuration currently encrypted with passphrase `changeme`:

```
./yamles change-passphrase --source yaml:file:/home/user/apiprojects/myyaml --old-passphrase changeme --new-passphrase ""
```

**Example 5**: Encrypt an unencrypted YAML configuration, this is equivalent to the `encrypt` option:

```
./yamles change-passphrase --source yaml:file:/home/user/apiprojects/myyaml --old-passphrase "" --new-passphrase changeme
```

You can run the following help command for more details on each parameter:

```
yamles change-passphrase --help
```
