# IPアドレスを管理するライブラリ
ログインや2段階認証などのシステムで同一IPアドレスからの連続アクセスを監視するライブラリです。

## 仕様
IPアドレスを保存するデータベースは任意のIDの中に作成されます。同一のIDを使ってインスタンス化することで、データベースにアクセスすることができます。

## 使用方法
```
$ip_configs = array(
  'id' => 'adminLogin',
  'server' => $_SERVER,
  'options' => array(
    'time_interval' => 5 * 60,
    'mistake_count' => 3,
    'save' => true
  )
);
$ip_control = new IP_Control( $ip_configs );

$ip_check = $ip_control->ip_lock_check(); // Boolean
// $ip_control->ip_recording();
// $ip_control->ip_accomplished();
```

### インスタンス化パラメータ
インスタンス化の時に以上の配列を渡すことができます。'id'と'server'のパラメータは必須です。<br>
'id'は任意の文字列を渡すことができ、idの数だけjsonを保存するディレクトリを作成します。<br>
<br>
'option'パラメータはIPアドレスの保存方法について設定することができます。初期値は 5*60 です。<br>
'time_interval'は、IPがロックされた後に待機させる秒数です。ここの数値を大きくすることで実質的に永久ロックをすることができます。
'mistake_count'は、IPがロックされるまでの回数を指定することができます。初期値は 3 です。<br>
'save'は、falseにすることで、IPロック解除時にそのIPアドレスのデータベースを削除する設定をすることができます。初期値は true です。

### IP管理メソッド
$ip_control->ip_lock_check\(\) はそのIPアドレスが現在ロック中か確認することができます。ロック中であればtrueが返ります。<br>
$ip_control->ip_recording\(\) はアクセスされたIPアドレスを記録します。認証できていない状態で記録されるので、ログイン可否の前に使用することを想定します。<br>
$ip_control->ip_accomplished\(\) はIPアドレスを認証します。ログイン完了後に使用することを想定しています。
