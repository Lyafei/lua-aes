# luaaes


some phper maybe find that the libraries in nginx-lua is not enough,
like aes mcrypt with ECB mode,
so  I create this.

like encrypt with PHP:
```php
mcrypt_encrypt(MCRYPT_RIJNDAEL_128, $key,$text, MCRYPT_MODE_ECB );
```

encrypt with lua:
```lua
local data      =   'lyafei'
local key       =   '1234567891234567' --length is 16
local aes    = require("resty.aes_ecb")
local ecb       = aes:new();
local enc_data  = ecb:encrypt(key,data );

```

MCRYPT_RIJNDAEL_256 is not AES-256, it's a different variant of the Rijndael block cipher. If you want AES-256 in mcrypt, you have to use MCRYPT_RIJNDAEL_128 with a 32-byte key. 

mean while,you will need to install libmcrypt,
because the luaaes will load  the libmcrypt with FFI,
try to install libmcrypt
Linux:
```
yum install libmcrypt libmcrypt-devel
```

Dockefile:
```
RUN yum install -y epel-release
RUN yum install -y libmcrypt libmcrypt-devel
```