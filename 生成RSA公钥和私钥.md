### Mac上生成RSA公钥和私钥

#### step1：

在系统任意位置新建一个文件夹用于存放公钥私钥，在终端使用cd命令进入该文件夹。例如：cd /Users/Shawn/RSAKey

#### step2：

Mac系统一般都自带OpenSSL，如果没有可以进行安装
sudo apt-get install openssl

#### step3：

生成私钥
openssl genrsa -out rsa_private_key.pem 1024

此时会提示输入密码，这个密码必须记住，后面可能会用到。
```
私钥文件名：rsa_private_key.pem
```

#### step4：

把RSA私钥转换成PKCS8格式
openssl pkcs8 -topk8 -inform PEM -in rsa_private_key.pem -outform PEM –nocrypt

#### step5：

生成公钥（通过私钥得到）
openssl rsa -in rsa_private_key.pem -out rsa_public_key.pem -pubout
```
公钥文件名：rsa_public_key.pem
```
