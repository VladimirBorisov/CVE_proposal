
As a result of the design of Dovecot LUA auth, lack of Dovecot documentation and a mistake in LUA auth script that is included in Mailcow the attacker can change the internal variables of Dovecot.

## Overviev
The Dovecot can use the LUA script (https://doc.dovecot.org/configuration_manual/authentication/lua_based_authentication/) as a part of the authentication process.
The Mailcow utilize the password_verify function of this module.
Dovecot documentation does not contain a full explanation of return values for this function.

The Mailcow LUA auth script returns the password in the form of "password=pass" as a return value (as Dovecot documentation describes for auth_passdb_lookup function). As a result in case the password contains space, the sequence after the space will be parsed by Dovecot as a key=value sequence. 

As a result, the attacker can set the password like "pass mail_crypt_save_version=0". The encryption will be switched off in this example.

Mostly the issue will not affect the server or other users because most of the variables cannot be used by attacker. But in some cases, this can cause the issue.
