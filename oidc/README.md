カレントディレクトリの主なリソースは次のとおりです：
1. Dockerfileファイル：イメージのすべてのコマンドのテキストファイルを作成します。その中で<code>--spring.config.additional-location=/oidc-provider/application.properties</code> は、マウントディレクトリ中の構成ファイルapplication.propertiesをロードすることを指定します。DB、redisなどの各パラメータをカスタマイズするに使用し、構成ファイルパラメータを変更した後コンテナを再起動するだけで有効になります；
2. oidc-provider：コンテナのマウントディレクトリ、OIDCプロジェクトのjarパッケージと構成ファイルを含まれています。詳細は次のとおりです：
    * oidc-provider-api.jar：プロジェクトのjarパッケージ、ポートは8080;
    * application.properties：プロジェクト構成ファイル、DB、redisなどの各パラメータをカスタマイズするに使用します。
    * user_pkey_chiba.der：FT基盘密钥。
3. dockerコマンド：
    * dockerビルド：
    ```bash
    sudo docker build -t oidc-provider-api:v1.0 .
    ```
    * すべてのイメージを表示します：
    ```bash
    sudo docker images
    ```
    * コンテナを実行し、oidc-providerディレクトリをマウントします：
    ```bash
    sudo docker run  -p 8088:8080  -v /home/ubuntu/miniapp/provider/dockerBuild/oidc-provider:/oidc-provider  --name oidc-provider-api -d oidc-provider-api:v1.0
    ```

    ※请根据实际路径修改<code>/home/ubuntu/miniapp/provider/dockerBuild/oidc-provider</code>。
<br/>
    * すべてのコンテナ情報を表示します：
    ```bash
    sudo docker ps -a
    ```
    * コンテナを再起動します：
    ```bash
    sudo docker container restart oidc-provider-api
    ```


※请注意，在OIDC的接口中，会调用FT基盘的<code>chiba/businesses/user_authentication</code>接口来验证登录有效性，此接口需要对password参数按照FT基盘的加密格式进行加密。

加密文件user_pkey_chiba.der需要与application.properties文件保持同一路径，并且在application.properties中设置该文件的路径（）。

```xml
ft-keypath=/oidc-provider/user_pkey_chiba.der
```

如果您修改了下面语句的红色部分，请对应修改ft-keypath属性。
<code>/home/ubuntu/miniapp/provider/dockerBuild/oidc-provider:/<font color=red>oidc-provider</font> </code>