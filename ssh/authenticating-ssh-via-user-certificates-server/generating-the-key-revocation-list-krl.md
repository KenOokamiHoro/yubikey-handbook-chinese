### 生成密钥吊销列表（KRL）

KRL 是一种可以吊销已签名 SSH 证书的简洁的二进制形式文件。

1. 创建一个空的吊销列表：

  ```sh
  ❯ touch /etc/ssh/revoked_keys
  ```

2. 修改 `/etc/ssh/sshd_config` 文件使其包含新的吊销列表：

  ```sh
  ❯ RevokedKeys /etc/ssh/revoked_keys
  ```

3. 需要的时候，可以吊销第一个签名的证书：

  ```sh
  ❯ ssh-keygen -k -f revoked_keys -s sshuser.root.ca.pub foo-cert.pub
  ```

4. 需要时也可以增加更多的要吊销的证书（使用 `-u`）：

  ```sh
  ❯ ssh-keygen -k -f revoked_keys -s sshuser.root.ca.pub -u bar-cert.pub
  ```

5. 确认吊销生效：

  ```sh
  ❯ ssh-keygen -Qf revoked_keys foo-cert.pub
  ```

6. 使用 `rsync`，`scp` 或者其他编排工具将`revoked_keys` 分发给每个主机（`/etc/ssh/revoked_keys`）

注意：只要序列号正常工作， `ssh-keygen` 不需要使用签名的公共证书去吊销。但是，目前在 OpenSSH 7.2p2 (Ubuntu) 上无效。
