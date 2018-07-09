## Redis设置密码[了解]

命令: `config set requirepass [密码]`

示例: `config set requirepass 123456`  设置密码为123456

当设置好密码后，redis会立即生效，此时需要通过如下命令进行密码验证:  `auth [设置的密码]`

通过`auth`命令进行密码验证。