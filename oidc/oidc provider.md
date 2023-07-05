 
<font face='Meiryo UI'>

# OIDC Provider グラフィックガイド

_made by pactera,2023.04.06_

---

## 1. WEB側

![OIDC Provider](pc.jpg) 

### 1.1. エンドポイント説明


- **ユーザー**
    *End-User(EU)。人類参加者、以下はEUと略称します。*
<br/>

- **Relying Party**
    *Relying Party(RP)、信頼されたサードパーティアプリケーション(OAuth2.0内のClient)、EUが認証を完成必要があって、OPからClaim情報を取得するためのアプリケーション。以下はRPと略称します。*
<br/>

- **OIDC Provider**
    *OIDC Provider(OP)、ID認証を提供するサービスプロバイダ、OAuth2.0内のAuthorization Server(認証サーバー)，ID認証サービスを提供し、サードパーティーアプリケーションにClaim情報を返すために使用されています(Relying Party)。以下はOPと略称します。*
<br/>

### 1.2 フロー

1. EUはRPサードパーティーログイン画面を開きます。
<br/>
2. サードパーティログインボタンをクリックして、OPログイン画面に遷移します。「authorize」 APIを呼び出す必要があり、パラメータの検証に成功したら自動的にOPログイン画面に遷移します。詳細は[「3.1_authorize」](#authorize)をご参照ください。
<br/>
3. アカウントとパスワードを入力して、ログインをクリックします。
<br/>
4. 認可成功、RPに遷移します<code>redirect_uri?code=xxx&state=xxxx   </code>。
 <br/>
5. RPはコードを利用して、access_token、refresh_token、id_tokenを取得します。詳細は[「3.2_token」](#token) をご参照ください。
<br/>
6. RPはaccess_tokenを利用してユーザー情報を取得します。詳細は[「3.3_userinfo」](#userinfo)をご参照ください。
<br/> 
8. RPはユーザー情報を表示します。
<br/> 

## 2. モバイル端末側



![OIDC Provider](app.jpg) 

### 2.1. エンドポイント説明

- **ユーザー**
    *End-User(EU)。人類参加者、以下はEUと略称します。*
<br/>

- **App**
     *スマートフォンのアプリ。* 
<br/>

- **MiniApp**
    *アプリに搭載されているミニアプリ。*
<br/>

- **Relying Party**
    *Relying Party (RP)、信頼されたサードパーティアプリケーション(OAuth2.0内のClient)、EUが認証を完成必要があって、OPからClaim情報を取得するためのアプリケーション。ここはミニアプリにマウントします。以下はRPと略称します。*
<br/>

- **OIDC Provider**
    *OIDC Provider(OP)、ID認証を提供するサービスプロバイダ、OAuth2.0内のAuthorization Server(認証サーバー)、ID認証サービスを提供し、サードパーティーアプリケーションにClaim情報を返すために使用されています(Relying Party)。以下はOPと略称します。*
<br/>


### 2.2 フロー

1. EUはアプリを開いてログインします。
<br/>
2. EUはミニアプリのアイコンをクリックして、ミニアプリを開きます。「isAuthorized」APIを呼び出し、初回認証の場合は認証画面がポップアップされ（3.ご参照）、認証履歴がある場合は省略されます（4.ご参照）。詳細は[「3.5_isAuthorized」](#isAuthorized)をご参照ください。
<br/>
3. 初回認証の場合は認証画面がポップアップされ、EUは「」ボタンをクリックします。
<br/>
4. ミニアプリはアプリを通じてOPのcodeインターフェースを呼び出し、codeを取得します。詳細は[「3.6_authcode」](#authcode) をご参照ください。
<br/>
5. ミニアプリはRPに遷移して、codeおよびほかの必須パラメータを渡す必要があります。
<br/> 
6. RPはcodeを利用してaccess_token、refresh_token、id_tokenなどを取得します。詳細は[「3.2_token」](#token)をご参照ください。。
<br/> 
7. RPはaccess_tokenを利用してユーザー情報を取得します。詳細は[「3.3_userinfo」](#userinfo)をご参照ください。。
<br/> 
8. RPはユーザー情報を表示します。
<br/> 


## 3 インターフェース

**3.1  アクセスコード取得（/oidc/authorize）**<p id="authorize"></p>

*※<code>GET/POST</code>を利用してパラメータを渡します。POSTを利用する時：*
> Content-Type: application/x-www-form-urlencoded

（GET）ご参照ください：
> http://www.oidc_example.com/oidc/authorize?client_id=abcde12345&response_type=code&redirect_uri=http%3A%2F%2Fwww.client_example.com%2Findex&scope=openid%2520email%2520profile%2520address&nonce=12345&state=abcde&code_challenge=12345abcdefghijk&code_challenge_method=S256&max_age=12000
 
|パラメータ|タイプ|必須|説明
|---------------|--------------|----------------|----------------------------------------------|
|client_id|String|`必須`|Client IDの文字列。アプリケーション登録時に発行したClient IDを指定してください。|
|response_type|String|`必須`|固定値`code`。|
|redirect_uri|String|`必須`|アプリケーション登録時に設定したフルURL（もしくはカスタムURIスキーム）を指定してください。事前に登録されているクライアントのリダイレクト URI 値のひとつに正確に一致している必要があります。<br/>※GETリクエストする時にURLエンコードをしてください。|
|scope|String|`必須`|UserInfo APIから取得できる属性情報を指定できます。詳細は[「4.1_scope」](#scope)をご参照ください 。<br/>※GETリクエストする時にURLエンコードをしてください。|
|state|String|`必須`|要求と応答の間で維持されるランダム値。|
|nonce|String|任意|リプレイアタック (opens new window)を防止するための文字列。この値はレスポンスで返されるIDトークンに含まれます。|
|code_challenge|String|任意|一意のcode_verifierをSHA256で暗号化したうえで、Base64URL形式にエンコードした値です。詳細は[「4.2_code_challenge」](#code_challenge)をご参照ください 。|
|code_challenge_method|String|任意|固定値`S256`。（ハッシュ関数SHA256を表します。）code_verifierからcode_challengeを算出する際の暗号化方式を指定します。|
|max_age|Number|任意|ユーザー認証後に許容される最大経過時間（秒）。max _Mageの場合、返されるID Tokenにはauth _time Claim Value。|

<br/>

正常系手本：
*アカウントとパスワードを入力してログイン操作を完成します。*

> http://redirect_uri?code=xz19ssafynqppsnosznhq3eob9bvxxjcqw5c&state=abcde

|パラメータ|説明|
|---------------|-----------------------------------------------------------|
|code|アクセストークンの取得に使用される認可コード。有効期間は10分です。また、認可コードは1回のみ利用可能です。 |
|state|リクエスト時に指定されたstate値。|

異常系手本：（status code: 4xx ）

> http://www.oidc_example.com/error?error=invalid_request&error_description=パラメーター%7Bclient_id%2Credirect_uri%2Cresponse_type%7Dは必須です&state=abcde

|パラメータ|説明|
|---------------|-----------------------------------------------------------|
|error|エラーコード。<br/>- invalid_request：パラメータは必須です、またはパラメータの値が無効です。<br/>- unsupported_response_type：response_typeの値が無効です。<br/>- invalid_scope：scopeの値が無効です。<br/>- unauthorized_client：指定されたclient_idはサポートされていません。|
|error_description|エラー内容。|
|state|リクエスト時に指定されたstate値。|

<br/>

**3.2 コードよりトークンを取得（/oidc/token）**<p id="token"></p> 

*※<code>POST</code>を利用してパラメータを渡します。POSTを利用する時：*

> Content-Type: application/x-www-form-urlencoded

Headers:
client_secretにBase64エンコードをしてください。またHeadersでパラメータを渡します。<br/>※Client IDがクライアントサイド・アプリケーションとして発行された場合は指定する必要はありません。
> Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW

Body:

|パラメータ|タイプ|必須|説明
|---------------|--------------|----------------|----------------------------------------------|
|client_id|String|`必須`|Client IDの文字列。|
|grant_type|String|`必須`|固定値`authorization_code`。|
|redirect_uri|String|`必須`|同意処理後の戻り先であるURLまたはCustom URI Scheme事前に登録されているクライアントのリダイレクト URI 値のひとつに正確に一致している必要があります。codeをリクエストする時に渡されたredirect _uriパラメータと一致にしてください。|
|code|String|`必須`|Authorizationエンドポイントで取得した認可コードの文字列を指定してください。|
|code_verifier|String|`任意`|半角英数字（a〜z、A～Z、0～9）および記号（-._~）からなるランダムな文字列。建议50文字以下。<br/>※認可リクエスト時にcode_challengeを指定した場合は必須です。|
 
<br/>

正常系手本：（status code: 200 ）
```json
{
    "access_token": "o1qjtqobs4j68nxwbytrxa80aj3xkut7uklg",
    "refresh_token": "60ubebp35maraih4qgrpfshov0qzdphbdk49",
    "token_type": "Bearer",
    "expires_in": "3600",
    "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6Inlya2x3MmdnemwxNjc3ODMwODAyNTkzIn0.eyJhdF9oYXNoIjoiTVRBd01UQXhNREF4TURFd01URXhNREV3TURFeE1UQXhNREF3TURBd01ERXhNREF3TVRBeE1UQXhNREV3TURBd01ERXdNVEF4TVRBeE1EQXhNREF4TURBeE1ERXhNVEF3TVRFeE1EQXhNVEV3TVRFd01URXdNVEV4TVRBeE1ERXdNREV3TVRBeE1ERXdNVEV4TVRFd01EQXdNVEV4TURFd01ERXdNVEF4TURBIiwic3ViIjoiNTRiYzBjOTE4OTBhNDE3ZWM3NTFkYmU3MWRkNjk3ZjUiLCJhdWQiOiJmN1VLRjlzU2NiajVaN3paIiwibmJmIjoxNjgxMDk4MDMzLCJhdXRoX3RpbWUiOjE2ODEwOTgwMzMsImlzcyI6Imh0dHBzOi8vd3d3LmNoaWJhYmFuay5jby5qcCIsIm5hbWUiOiLljYPokYnjgIDlpKrpg47vvJDvvJIiLCJleHAiOjE2ODExMTAwMzMsImlhdCI6MTY4MTA5ODAzMywibm9uY2UiOiIzMzNkZGZnZDlmZ3VkZm9nazkwOTk0NCJ9.EjgbVoyU9TDUYi1xWYoC6_8dAUenRCTM-4UfoCDVLvSS0cxvMlQpFhIHDcaUellDR-eJ1RtbOoz88fyXE7pp2A",
    "scope": "openid email profile"
}
```

|パラメータ|説明|
|---------------|-----------------------------------------------------------|
|access_token|APIへアクセスするのに使用します。 |
|refresh_token|Access Tokenを更新するときに使用します。有効期限は4週間です。|
|token_type|Web APIへアクセスする際にAccess Tokenを適切に用いるために必要な情報を提供します。Bearer Token形式です。固定値`Bearer`。|
|expires_in|Access Tokenの有効期限を表す秒数です。|
|id_token|ユーザー認証情報を含む改ざん検知用の署名付きトークンです。詳細は[「4.3_id_token」](#id_token)をご参照ください。|
|scope|アクセストークンに紐付く許諾されたスコープ ，許可サーバーは「スコープ」応答パラメーターを使用して、発行されたアクセストークンのスコープをクライアントに通知します。|

異常系手本：（status code: 4xx ）
```json
{
  "error": "invalid_grant",
  "error_description": "code無効またはタイムアウト"
}
```

|パラメータ|説明|
|---------------|-----------------------------------------------------------|
|error|エラーコード。<br/>- invalid_request：パラメータは必須です、またはパラメータの値が無効です。<br/>- unsupported_grant_type：grant_typeの値が無効です。<br/>- invalid_grant：codeが無効または期限切れです。<br/>- invalid_client：指定されたclient_idはサポートされていません、client_secretの値が無効です。|
|error_description|エラー内容。| 

<br/>

**3.3  ユーザー情報照会、サードパーティーに取得できるように（/oidc/userinfo）**<p id="userinfo"></p>

*※<code>GET/POST</code>を利用してパラメータを渡します。*

（GET）ご参照ください：
> http://www.oidc_example.com/oidc/userinfo

Headers:
Access Tokenの文字列を指定してください。（Bearer Token形式）。
> Authorization: Bearer 0bta4dzc5om5tcs5mqte5fpcacy5r83n1emj

<br/>

正常系手本：（status code: 200 ）
```json
{
  "sub": "4d4f7fafd2286f8dc7e8d7fffad424f6",
  "name": "太郎",
  "given_name": null,
  "family_name": null,
  "nickname": null,
  "picture": null,
  "birthdate": "1992-01-01",
  "gender": "1",
  "email": "jphxxx@chbank.co.jp",
  "phone_number": "080-6665-xxxx",
  "address": {
    "formatted": "千葉市中央区末広４丁目１−２−xxx",
    "country": null,
    "region": null,
    "locality": null,
    "postal_code": "260-0843"
  }
}
```

|パラメータ|説明|対応するscope|
|---------------|----------------------------------|-------------------------|
|sub|ユーザー識別子|openid|
|name|姓名|profile|
|given_name|名 |profile|
|family_name|姓|profile|
|nickname|表示名|profile|
|picture|プロフィール画像URL|profile|
|birthdate|生年|profile|
|gender|性別，1：男性2：女性|profile|
|email|メールアドレス|email|
|phone_number|電話番号|phone|
|address|- formatted：都道府県 + 市区町村<br/>- country：国コード<br/>- region：都道府県<br/>- locality：市区町村<br/>- postal_code：郵便番号|address|

異常系手本：（status code: 4xx ）
```json
{
  "error": "invalid_token",
  "error_description": "accessToken無効またはタイムアウト"
}
```

|パラメータ|説明|
|---------------|-----------------------------------------------------------|
|error|エラーコード。<br/>- invalid_request：パラメータは必須です、またはパラメータの値が無効です。<br/>- invalid_token：accessTokenが無効または期限切れです。|
|error_description|エラー内容。| 

<br/>


**3.4  kidを利用してpublic_key（/keys/{kid}）を取得します**<p id="public_key"></p>

*※<code>GET</code>を利用してパラメータを渡します。* 

ご参照ください：
> http://www.oidc_example.com/keys/{kid}

pathパラメータ

|パラメータ|タイプ|必須|説明
|---------------|--------------|----------------|----------------------------------------------|
|kid|String|`必須`|署名検証に用いる公開鍵のKey ID。id_tokenのheaderに格納されています。|

<br/>

正常系手本：（status code: 200 ）
```json
{
  "kid": "12345",
  "publicKey": "wwwwDQYJKoZIhvcNAQEBBQADSwAwSAJBAIdVddpMZCJVmnE+s7ZvRUwUiPsUBeLD12//q3Lo29G7fHWhMowGHOdN32rFuDhaaE4nhO/GrQfXqY+iZ/VX2HUCAwEAAQ=="
}
```

|パラメータ|説明|
|---------------|-----------------------------------------------------------|
|kid|ID TokenのHeaderに含まれているKey ID。Signatureを生成したPrivate Keyの対になるPublic Keyを判定するために利用。|
|publicKey|Signatureを生成したPrivate Keyの対になるPublic Key文字列。|  

異常系手本：（status code: 4xx ）
```json
{
  "error": "invaild_kid",
  "error_description": "公開鍵が取得できませんでした。kidの値をご確認ください。"
}
```

|パラメータ|説明|
|---------------|-----------------------------------------------------------|
|error|エラーコード。<br/>- invaild_kid：公開鍵が取得できませんでした。kidの値をご確認ください。|
|error_description|エラー内容。| 

<br/>

**3.5  ミニアプリはユーザー認可を取得したかどうか（/auth/isAuthorized）**<p id="isAuthorized"></p>

*※<code>GET/POST</code>を利用してパラメータを渡します。POSTを利用する時：*
> Content-Type: application/x-www-form-urlencoded

ご参照ください：

> http://www.oidc_example.com/auth/isAuthorized?userId=00000&miniapp_id=321008553559111111
 

|パラメータ|タイプ|必須|説明
|---------------|--------------|----------------|----------------------------------------------|
|userId|String|`必須`|appのユーザーID。|
|miniapp_id|String|`必須`|クライアントID（ mini app id）。|

<br/>

正常系手本：（status code: 200 ）
```json
{
    "isAuthorized": true
}
```

|パラメータ|説明|
|---------------|-----------------------------------------------------------|
|isAuthorized|true=承認済み，false=許可されていますん。| 
 

異常系手本：（status code: 4xx ）
```json
{
  "error": "unauthorized_client",
  "error_description": "サポートされていないクライアント"
}
```

|パラメータ|説明|
|---------------|-----------------------------------------------------------|
|error|エラーコード。<br/>- invalid_request：パラメータは必須です、またはパラメータの値が無効です。<br/>- invalid_client：指定されたclient_idはサポートされていません。 |
|error_description|エラー内容。 | 

<br/>

**3.6  認可コードを取得する（/oidc/authcode）**<p id="authcode"></p>

*※<code>GET/POST</code>を利用してパラメータを渡します。POSTを利用する時：*
> Content-Type: application/x-www-form-urlencoded

（GET）ご参照ください：
> http://www.oidc_example.com/oidc/authcode?userId=00000&client_id=abcde12345&response_type=code&redirect_uri=http%3A%2F%2Fwww.client_example.com%2Findex&scope=openid%2520email%2520profile%2520address&nonce=12345&state=abcde&code_challenge=12345abcdefghijk&code_challenge_method=S256&max_age=12000
 

|パラメータ|タイプ|必須|説明
|---------------|--------------|----------------|----------------------------------------------|
|userId|String|`必須`|appのユーザーID。|
|client_id|String|`必須`|クライアントID（ mini app id）。|
|response_type|String|`必須`|固定値`code`。|
|redirect_uri|String|`必須`|アプリケーション登録時に設定したフルURL（もしくはカスタムURIスキーム）を指定してください。事前に登録されているクライアントのリダイレクト URI 値のひとつに正確に一致している必要があります。<br/>※GETリクエストする時にURLエンコードをしてください。|
|scope|String|`必須`|UserInfo APIから取得できる属性情報を指定できます。詳細は[「4.1_scope」](#scope)をご参照ください 。<br/>※GETリクエストする時にURLエンコードをしてください。|
|state|String|`必須`|要求と応答の間で維持されるランダム値。|
|nonce|String|任意|リプレイアタック (opens new window)を防止するための文字列。この値はレスポンスで返されるIDトークンに含まれます。|
|code_challenge|String|任意|一意のcode_verifierをSHA256で暗号化したうえで、Base64URL形式にエンコードした値です。詳細は[「4.2_code_challenge」](#code_challenge) をご参照ください。|
|code_challenge_method|String|任意|固定値`S256`。（ハッシュ関数SHA256を表します。）code_verifierからcode_challengeを算出する際の暗号化方式を指定します。|
|max_age|Number|任意|ユーザー認証後に許容される最大経過時間（秒）。max _Mageの場合、返されるID Tokenにはauth _time Claim Value。|

<br/>

正常系手本：（status code: 200 ）
```json
{
    "code": "pnmdm7kl88ily6ghfjxyg3jmu6nfyy4bjpao",
    "state": "abcde"
}
```

|パラメータ|説明|
|---------------|-----------------------------------------------------------|
|code|認可コード。| 
|state|要求と応答の間で維持されるランダム値。| 

異常系手本：（status code: 4xx ）
```json
{
    "error": "unsupported_response_type",
    "error_description": "response_typeの値が無効です",
    "state": "abcde"
}
```

|パラメータ|説明|
|---------------|-----------------------------------------------------------|
|error|エラーコード。 <br/>- invalid_request：パラメータは必須です、またはパラメータの値が無効です。<br/>- unsupported_response_type：response_typeの値が無効です。<br/>- invalid_scope：scopeの値が無効です。<br/>- unauthorized_client：指定されたclient_idはサポートされていません。|
|error_description|エラー内容。| 
|state|要求と応答の間で維持されるランダム値。| 

<br/>

**3.7  トークンの有効期限が切れたあと、refresh tokenでトークンをリフレッシュする（/oidc/token）**<p id="authcode"></p>

*※<code>POST</code>を利用してパラメータを渡します。POSTを利用する時：*

> Content-Type: application/x-www-form-urlencoded

Headers:
client_secretにBase64エンコードをしてください。またHeadersでパラメータを渡します。<br/>※Client IDがクライアントサイド・アプリケーションとして発行された場合は指定する必要はありません。
> Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW

Body:

|パラメータ|タイプ|必須|説明
|---------------|--------------|----------------|----------------------------------------------|
|client_id|String|`必須`|Client IDの文字列。|
|grant_type|String|`必須`|固定値`refresh_token`。|
|refresh_token|String|`必須`|トークンエンドポイント（新規発行）より返却されたリフレッシュトークン。 

<br/>

正常系手本：（status code: 200 ）
```json
{
    "access_token": "1u8d3kzf003xu96wfg9m5x9l4wia4wgcxhum8",
    "refresh_token": "1kki32g3khz3c60q17dsiuekp0jcevjvlq8ki",
    "token_type": "Bearer",
    "expires_in": "3600",
    "id_token": "1eyJhbGciOiJSUzI1NiIsInR5cCI6Imp3dCIsImtpZCI6Inlya2x3MmdnemwxNjc3ODMwODAyNTkzIn0.eyJhdF9oYXNoIjoiTVRBd01EQXhNVEF4TVRFd01EQXdNVEF4TURBd01ERXdNREF4TURBd01EQXdNVEF4TVRFd01URXdNREV4TURBeE1URXdNREV3TURBd01EQXhNREV3TURFeE1ERXhNVEF3TVRFeE1ERXhNVEF3TURBeE1UQXdNREF3TVRBd01ERXdNREV4TURBd01URXhNVEV3TURBd01EQXdNREF3TVRBd01EQXdNVEF4TURFIiwic3ViIjoiNTRiYzBjOTE4OTBhNDE3ZWM3NTFkYmU3MWRkNjk3ZjUiLCJhdWQiOiJmN1VLRjlzU2NiajVaN3paIiwibmJmIjoxNjgxMDk4MDUxLCJhdXRoX3RpbWUiOjE2ODEwOTgwNTEsImlzcyI6Imh0dHBzOi8vd3d3LmNoaWJhYmFuay5jby5qcCIsIm5hbWUiOiLljYPokYnjgIDlpKrpg47vvJDvvJIiLCJleHAiOjE2ODExMTAwNTEsImlhdCI6MTY4MTA5ODA1MSwibm9uY2UiOiIzMzNkZGZnZDlmZ3VkZm9nazkwOTk0NCJ9.bsVqpWYlILfEnRkMCJWgLIOXzqcIFvEJHQAMyGmjhBo2oaLvG6b57Bgl_F1JkUarJTtloykXHEAsUVu74C46Zg",
    "scope": "openid email profile"
}
```

|パラメータ|説明|
|---------------|-----------------------------------------------------------|
|access_token|APIへアクセスするのに使用します。 |
|refresh_token|Access Tokenを更新するときに使用します。有効期限は4週間です。|
|token_type|Web APIへアクセスする際にAccess Tokenを適切に用いるために必要な情報を提供します。Bearer Token形式です。固定値`Bearer`。|
|expires_in|Access Tokenの有効期限を表す秒数です。|
|id_token|ユーザー認証情報を含む改ざん検知用の署名付きトークンです。詳細は[「4.3_id_token」](#id_token)をご参照ください。|
|scope|アクセストークンに紐付く許諾されたスコープ ，許可サーバーは「スコープ」応答パラメーターを使用して、発行されたアクセストークンのスコープをクライアントに通知します。|

異常系手本：（status code: 4xx ）
```json
{
    "error": "invalid_grant",
    "error_description": "refresh_tokenが無効または期限切れ"
}
```

|パラメータ|説明|
|---------------|-----------------------------------------------------------|
|error|エラーコード。<br/>- invalid_request：パラメータは必須です、またはパラメータの値が無効です。<br/>- unsupported_grant_type：grant_typeの値が無効です。<br/>- invalid_grant：refresh_tokenが無効または期限切れ。<br/>- invalid_client：指定されたclient_idはサポートされていません、client_secretの値が無効です。|
|error_description|エラー内容。| 

<br/>

## 4. 付録

### <p id="scope">4.1 scope</p>

*scopeパラメータに指定できるスコープは以下のとおりです。複数のスコープを指定するには、URLエンコードされた空白文字（%20）で区切って指定します。*

|範囲値|説明|
|---------------|-----------------------------| 
|openid|（必須）ユーザー識別子を返却します。|
|profile|姓名・生年・性別を返却します。|
|email|メールアドレスと確認済みフラグを返却します。|
|address|ユーザー登録住所情報を返却します。|
|phone|連絡先番号。|

### <p id="code_challenge">4.2 code_challenge</p>

code_verifierおよびcode_challengeの生成方法は下記をご確認ください。
※コードはJava言語を使用する


```java{.line-numbers}
    /**
     * @param code_verifier
     * @Description: code_challengeの生成
     * @return: java.lang.String
     */
    public String createChallenge(String code_verifier) throws NoSuchAlgorithmException {
        MessageDigest digest = MessageDigest.getInstance("SHA-256");
        byte[] encodedhash = digest.digest(code_verifier.getBytes(StandardCharsets.UTF_8));
        String hexString  = bytesToHex(encodedhash);
        return java.util.Base64.getUrlEncoder().withoutPadding().encodeToString(hexString .getBytes(StandardCharsets.UTF_8));
    }
    /**
     * @param hash
     * @Description: バイトから16進
     * @return: java.lang.String
     */
    private String bytesToHex(byte[] hash) {
        StringBuilder hexString = new StringBuilder(2 * hash.length);
        for (int i = 0; i < hash.length; i++) {
            String hex = Integer.toHexString(0xff & hash[i]);
            if (hex.length() == 1) {
                hexString.append('0');
            }
            hexString.append(hex);
        }
        return hexString.toString();
    }
```

### <p id="id_token">4.3 id_token</p>

ID Tokenとはユーザー認証情報を含む改ざん検知用の署名付きTokenであり、JWT（JSON Web Token）フォーマットでエンコードされています。
JWTはピリオド（"."）区切りのHeader、Payload、Signatureから構成されます。

**Header**

署名のアルゴリズムなどJWTを取り扱うために必要なパラメーターが含まれます。

|範囲値|説明|
|---------------|-----------------------------| 
|typ|固定値`JWT`。|
|alg|固定値`RS256`。|
|kid|Public KeysエンドポイントのPublic Keyのキー名に一致します。詳細は[「4.4_public_key」](#public_key)をご参照ください。|
 
 **Payload**

ユーザー識別子や認証時刻などユーザーの認証情報のパラメーターが含まれます。

|範囲値|説明|
|---------------|-----------------------------| 
|iss|ID Tokenの発行元（固定値）。|
|sub|ユーザー識別子,openid。|
|aud|リクエストする時伝来したClient ID。|
|iat|ID Tokenの発行時刻のUNIXタイムスタンプ。	|
|nbf|発効時間。|
|nonce|RP送信要求時に提供されるランダム文字列。|
|auth_time|EUが認証を完了した時間。RPがAuthN要求を送信するときにmax _Mageのパラメータを指定するには、このClaimが必要です。|
|exp|ID Tokenの有効期限のUNIXタイムスタンプ。|
|at_hash|Access Tokenのハッシュ値。Access TokenをID TokenのSignature生成時と同じハッシュアルゴリズム（SHA256）でハッシュ化し、オクテッドの前部をBase64URLエンコードした文字列。詳細は[「4.4_at_hash」](#at_hash)をご参照ください。|

**Signature**

HeaderとPayloadから生成された改ざん防止のための署名です。

Base64URLエンコードされたHeader + "." + Payloadを入力値としてRSA-SHA256で署名した結果をBase64URLエンコードした文字列です。

**検証手順**

- 取得したJWTをピリオド（"."）で区切りHeader、Payload、Signatureの3つに分割します。
<br/>
- Headerに含まれるkid値を用いて、Public Keysエンドポイントからkid値（またはキー名）から対になるPublic Keyを取得します。詳細は[「3.4_public_key」](#public_key)をご参照ください。
<br/>
- 署名が正しいかを検証し、次のコードを参照してください：
※コードはJava言語を使用する

```java{.line-numbers}
    /**
     * @param idToken
     * @param publicKey
     * @Description: Verify signature
     * @return: boolean
     */
    public boolean verify(String idToken, String publicKey) throws Exception {
        try {
            Algorithm algorithm = Algorithm.RSA256((RSAPublicKey) generateRsaPublic(publicKey), null);
            JWTVerifier verifier = com.auth0.jwt.JWT.require(algorithm).withIssuer("固定値，OPに確認してください").build();
            verifier.verify(idToken);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return true;
        }
    }
    /**
     * @param key
     * @Description: Generate a public key
     * @return: java.security.PublicKey
     */
    private PublicKey generateRsaPublic(String key) throws Exception {
        KeyFactory rsa_fact = KeyFactory.getInstance("RSA");
        byte[] data = Base64.decode((key.getBytes()));
        X509EncodedKeySpec spec = new X509EncodedKeySpec(data);
        return rsa_fact.generatePublic(spec);
    }
```
<br/>

- リクエストがnonceパラメータを渡した場合は、Payloadにnonceが存在するかどうかを確認し、値が渡したものと同じであるかどうかを確認してください。
<br/>
- Headers内のiss値はOPから提供されている値と同じかどうかを確認してください。
<br/>
- Payloadのaud値を確認して、現在のクライアントの client_idであるかどうかを確認してください。
<br/>
- Access TokenとID Tokenを同時に発行する場合には、 Access TokenをHeaderのalgと同じハッシュアルゴリズム（SHA256）でハッシュ化し、オクテッドの前部をBase64URLエンコードした文字列とPayloadのat_hash値が一致していることを検証します。詳細は[「4.4_at_hash」]をご参照ください(#at_hash)。
<br/>
- ID Tokenの有効期限を確認するために、Payloadのexp値が検証時のUNIXタイムスタンプの値よりも大きいことを検証します。


### <p id="at_hash">4.4 at_hash</p>

Access Tokenのハッシュ値，Access TokenをID TokenのSignature生成時と同じハッシュアルゴリズム（SHA256）でハッシュ化し、オクテッドの前部をBase64URLエンコードした文字列。
※コードはJava言語を使用する，issはOPに確認してください。


```java{.line-numbers}
    /**
     * @param accessToken
     * @Description: use accessToken to create at_hash
     * @return: java.lang.String
     */
    public String createHash(String accessToken) throws Exception {
        MessageDigest digest = MessageDigest.getInstance("SHA-256");
        byte[] encodedhash = digest.digest(accessToken.getBytes(StandardCharsets.UTF_8));
        String hexString = bytesToHex(encodedhash);
        BigInteger sint = new BigInteger(hexString, 16);
        //10進2進
        return java.util.Base64.getUrlEncoder().withoutPadding().encodeToString(sint.toString(2).substring(0, 128).getBytes());
    }
    /**
     * @param hash
     * @Description: バイトから16進
     * @return: java.lang.String
     */
    private String bytesToHex(byte[] hash) {
        StringBuilder hexString = new StringBuilder(2 * hash.length);
        for (int i = 0; i < hash.length; i++) {
            String hex = Integer.toHexString(0xff & hash[i]);
            if (hex.length() == 1) {
                hexString.append('0');
            }
            hexString.append(hex);
        }
        return hexString.toString();
    }
```


# The End #
</font>
