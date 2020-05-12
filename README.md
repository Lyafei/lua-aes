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
local aes    = require("resty.aes_ecb")

local data      =   'lyafei'
local key       =   '12345678912345678912345678912345' --length is 32
local ecb       = aes:new();
local enc_data  = ecb:encrypt(key,data );

local dec_data= ecb:decrypt(key,enc_data );

# dec_data = 'lyafei���������'

local function Strip_Control_and_Extended_Codes( str )
    local s = ""
    for i = 1, str:len() do
        if str:byte(i) >= 32 and str:byte(i) <= 126 then
            s = s .. str:sub(i,i)
        end
    end
    return s
end

dec_data = Strip_Control_and_Extended_Codes(dec_data)

# dec_data = 'lyafei'
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