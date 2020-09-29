# CSRF（クロスサイトリクエストフォージェリ）

CSRFとは、ウェブサイトが外部サイトを経由した悪意のあるリクエストを受け入れてしまう場合に、悪意のある人が用意した罠により利用者が予期しない処理を実行させられてしまう攻撃です。

CSRFは利用者が意図したリクエストであるかどうかを識別する仕組みを持つことで対策できます。

その手段としてはバックエンドでランダムな値（以下CSRFトークンと呼びます）を生成し、それをフロントエンドに渡します（このときバックエンド側ではセッションにCSRFトークンを保存します）。
フロントエンドでは、HTTPリクエストにCSRFトークンを埋め込み、送信します。
バックエンドではHTTPリクエストに埋め込まれたCSRFトークンと、セッションから取り出したCSRFトークンを突き合わせて、一致すれば利用者が意図したリクエストだとみなして処理を続行します。
CSRFトークンが一致しなければ不正なリクエストだとみなして処理を終了します。

!!! Note
    CSRFトークンの生成には暗号論的擬似乱数生成器を使用する必要があります。

一般的なウェブアプリケーションフレームワークであれば、フレームワーク本体やよく使われるプラグインなどがCSRFトークンの仕組みを備えています。

SPAでCSRFトークンを扱う場合、まずバックエンド側にCSRFトークンを返すREST APIを用意します。
フロントエンド側ではSPAの初期処理でREST APIからCSRFトークンを取得し、インメモリに保持します。
そしてHTTPリクエストを行う際に、必要に応じてHTTPリクエストヘッダへCSRFトークンを設定します。
