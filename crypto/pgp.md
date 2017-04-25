# pgp

> pretty good privacy

* `gpg` is probs the best client
* annoyingly, most linux systems still have two versions: `gpg` and `gpg2`
* **assume that i'm referring to `gpg2` below**

## use with git

```shell
git config --global commit.gpgsign true
git config --global gpg.program $(which gpg)
```

## use with keybase.io

```shell
# import public and private keys to local
keybase pgp export -q <key-id> | gpg2 --import
keybase pgp export -q <key-id> --secret | gpg2 --import --allow-secret-key-import


```

## revoke a key

```shell
# creates a revoke cert for later use
gpg --gen-revoke <key-id>

# when you want to use it
gpg --import my_revocation.txt
gpg --keyserver pgp.mit.edu --send-keys <key-id>
```

## keyservers

* repositories of public keys
* usually searchable
* global propogation is a thing
* _revocation certs_ are propogated too
