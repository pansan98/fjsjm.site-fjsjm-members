# fjsjm-members - 会員管理機能

これはWordpressのプラグインです。
エンジニア向に配布を行なっています。

---

## 機能

- 会員管理はすばやく簡単に構築できます
    - 会員の管理や停止中アカウントの復旧、強制退会など会員に対するその他設定が可能
    - 2段階認証も有効にしてセキュリティ面も安心
    - パスワード忘れや再発行処理も導入
    - 会員さまへのお知らせ機能
    - デフォルトで用意されたHOME、お知らせページ、マイページの変種

## アプリ構成

- fjsjm-members
    - ┗assets
        - js、cssファイル群
    - ┗classes
        - PHPクラスファイル群
    - ┗core
        - アクションフック、フィルターフック群
    - ┗includes
        - configや定数設定
    - templates
        - viewファイル群

## 公開画面のページ追加方法
ルーティングは別途プラグイン内で実装されています。  
管理画面から設定ページを開き「公開画面URL設定」で設定されているURLパス以降のページへ飛ぶとルーティング対象になります。  
**※プラグイン内で行なっているルーティングはパーマリンク設定が「基本」に設定されている場合は機能しません。**

### 公開画面用のルーティング設定
PHPクラスファイル群のhttpディレクトリ内にある「Routing.php」で設定されています。  
addメソッドでルートを追加し、「:」コロンを付与すると動的URLも設定できます。

    $route->add('path/to', function() {
        // add to method
    });

動的パラメーターは「Request」クラスから取得することができます。  
動的パラメーターはwhereメソッドを呼び出すことで正規表現を付与することもできます。

    $route->add('path/to/:route', function() {
        var_dump(\FjsjmMembers\Classes\Request::get()->parameter('route'));
    })->where('path/to/:route', 'route', "/^[a-zA-Z0-9]+$/");

### テンプレートを出力する
テンプレートはHttpトレイトファイルから呼び出せます。Httpトレイトからrenderメソッドを呼び出すことでテンプレートを出力できます。  
呼び出したいクラスファイルからHttpをインポートします。

    use \FjsjmMembers\Classes\Http\Controllers\Http;

    public function index() {
        $this->render('template/path/to', [], true);
    }

第1引数にはテンプレートのパス、第2引数にはテンプレートに渡したいデータ、第3引数は出力後に処理を停止するか出力するテンプレートを取得することができます。  
第1引数は必須です。第3引数はデフォルトではfalseです。  
大元はViewクラスに存在しており、直接Viewクラスから呼び出すこともできます。