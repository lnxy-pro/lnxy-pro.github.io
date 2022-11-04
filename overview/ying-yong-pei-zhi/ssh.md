# SSH

**生成公钥**

```apl
ssh-keygen     # $HOME/.ssh/文件下生成id_rsa.pub(公)和id_rsa(私)
ssh-copy-id  user@host # 上传公钥
--------------------------------👇失败情况--------------------------------
打开远程主机 /etc/ssh/sshd_config 文件
检查配置,取消 # 
     # RSAAuthentication yes
　　# PubkeyAuthentication yes
　　# AuthorizedKeysFile .ssh/authorized_keys
重启远程ssh
        service ssh restart  # ubuntu系统
      /etc/init.d/ssh restart     # debian系统　　
远程主机将用户公钥保存在用户主目录
ssh user@host 'mkdir -p .ssh && cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub
```
