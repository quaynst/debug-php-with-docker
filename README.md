# 必要な手順

引用元 : [Docker for Mac 上のコンテナに対して、PhpStorm + xdebug でリモートデバッグ](https://qiita.com/kunit/items/52518cb1460d3deb6034#mac-%E4%B8%8A%E3%81%AE%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AF%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%95%E3%82%A7%E3%83%BC%E3%82%B9-lo0-%E3%81%AB%E5%AF%BE%E3%81%97%E3%81%A6%E3%82%A8%E3%82%A4%E3%83%AA%E3%82%A2%E3%82%B9%E3%82%92%E8%BF%BD%E5%8A%A0%E3%81%99%E3%82%8B)

以下の手順が必要になる。

* Mac 上のネットワークインタフェース lo0 に対してエイリアスを追加する
* xdebug.ini を設定する
* PhpStormを設定する

(注意) DBGp の設定は必要ない


## Mac 上のネットワーク・インタフェース lo0 に対してエイリアスを追加する

以下のコマンドをターミナル等で入力して、エイリアスを追加する。


```bash
sudo ifconfig lo0 alias 10.254.254.254
```

## xdebug.ini の設定

Docker image を作る際に以下のファイルを COPY するようにするか、RUNで echo させながら追加する等のことをする。

* (注意) xdebug.so のパスは使っているディストリビューションによって違うかもしれない。以下のものは CentOS のもの
* (注意) 以下の設定はリモートデバッグをするのに必要最低限度のものなので、プロファイルを取りたい場合等は適宜 xdebug.profiler_enable 等の設定を追加すること


```ini:xdebug.ini
zend_extension=/usr/lib64/php/modules/xdebug.so

xdebug.remote_enable = 1
xdebug.remote_autostart = 1
xdebug.remote_host = 10.254.254.254
xdebug.remote_port = 9000
xdebug.idekey = PHPSTORM
```

# 参考

## see  
- https://qiita.com/furu8ma/items/86a147f90ad6344b4cd9
- https://qiita.com/castaneai/items/d5fdf577a348012ed8af
- https://cockscomb.hatenablog.com/entry/docker-for-mac-localhost
- https://qiita.com/prgseek/items/e557a371d7bd1f57b9b1
    
## old
- https://gist.github.com/chadrien/c90927ec2d160ffea9c4
