# mod_authn_yubikey

If you want to use your yubikey with your own yubico HSM server you need to use this version of mod_authn_yubikey, because the original one does not support do declare your own AuthYkValidationHost and the AuthYkValidationProtocol.

You can specify an api version to use yubico (yubiserve compliant) 2.0 validation protocol.
Default is to not use any api version.

You can share the AuthYubiKeyTmpFile between vhost using AuthYkShareDb on. Default is to overwrite previous entry with the same username (in this case, you need one tmpDb for each vhost).

###### Setup local config files
To add the user frank.nord with the password foobar123 and the token id abcdefghijkl you have to do:

```
htpasswd -csb $your_yubikeyConfigLocation/…/yubikey/ykUserDb abcdefghijkl frank.nord:foobar123
```

```
cat ../yubikey/ykUserDb
abcdefghijkl:{SHA}l7xgM5Yq0sDBK6CYZmSwDKuEiAY=
```

Now you can edit your Apache Config (tested on Apache2 2.2.22-13+deb7u3 with Debian wheezy >=7.5)

###### Apache Sample Config
```
<Location /yubitest>
AuthType Basic
AuthBasicProvider yubikey
AuthName "Please Log In using your YubiKey"
AuthYubiKeyTimeout 30
AuthYubiKeyTmpFile yubikey/ykTmpDb
AuthYubiKeyUserFile yubikey/ykUserDb
AuthYubiKeyRequireSecure On
AuthYubiKeyExternalErrorPage Off
AuthYkApiVersion 2.0

AuthYkValidationProtocol https
AuthYkValidationHost api.yubico.com

AuthYkShareDb off

Require valid-user
</Location>
```

You can also define your fav. choice of your AuthYkValidationProtocol http or https or … 

###### Build and Install
This has yet to be documented.

###### ToDo
Build and upload a current .deb file for simple dpkg -i libapache2-mod-authn-yubikey_$version_amd64.deb because this one inside the Debian apt repository doesn't support this feature.


