=============================================================
VDMTools全体像
=============================================================

動作環境
=============================================================
* Mac OS X
* Linux
* Windows XP/Vista/7


機能
=============================================================
* 構文解析／チェック
* 型チェック
* インタープリタ
* C++/Javaコード生成
* 証明課題生成
* 清書
* UMLリンク
* JavaからVDM++への変換(現在未サポート)


ソースディレクトリ構成
=============================================================
VDMTools のソースディレクトリ構成を示す。
使用していないもの、基本機能に関連しないものは省略する。

.. code-block:: none

  .-+-code-+-api              ... CORBAインターフェース実装、サンプル
    |      +-build            ... ツールをビルドするためのファイル
    |      +-cg               ... C++/javaコード生成実装
    |      +-dep
    |      +-diverse          ... コマンドラインメインプログラム等
    |      +-errmsg           ... 型チェックエラーメッセージ
    |      +-eval             ... インタープリタ実装、清書機能実装
    |      +-java             ... javaランタイムライブラリ
    |      +-java2vdm++       ... JavaVDM++変換手実装部分
    |      +-lib              ... メタデータ実装等
    |      +-make_tools       ... perl ツール類
    |      +-parser           ... 構文解析実装
    |      +-pog              ... 証明課題生成手実装部分
    |      +-pog-aux          ... 証明課題生成手実装部分
    |      +-pog-pretty       ... 証明課題生成手実装部分
    |      +-port             ... OS間差異吸収
    |      +-qtgui            ... GUI版GUI実装部分
    |      +-random           ... 乱数実装
    |      +-recover          ... 構文解析実
    |      +-servicemanager   ... CORBA経由サービス実装(機能未使用)
    |      +-specfile         ... 仕様ファイル操作
    |      +-specman          ... 仕様ファイル管理手実装部分
    |      +-statsem          ... 型チェック実装
    |      +-traces           ... 組み合わせテスト実装
    |      +-transforms       ... JavaVDM++変換手実装部分
    |      +-uml              ... UMLリンク手実装部分
    |      +-utils            ... 各種ユーティリティ
    |      +-vice             ... vice手実装部
    |      +-win32            ... Windows用各種ファイル
    |      +-word             ... MS Word 用ファイル
    |
    +-spec-+-ast              ... 抽象構文木等基本データ構造定義、コード生成対象
    |      +-cg-spec          ... C++/javaコード生成仕様、コード抽象構文木定義等一部コード生成対象
    |      +-dep-spec
    |      +-eval-spec        ... インタープリタ仕様、動的意味定義等一部コード生成対象
    |      +-j2vtf-spec       ... JavaVDM++変換仕様
    |      +-java2vdm         ... JavaVDM++変換仕様、一部コード生成対象
    |      +-jss-spec         ... JavaVDM++変換仕様、一部コード生成対象
    |      +-pog-aux-spec     ... 証明課題生成仕様、コード生成対象
    |      +-pog-pretty-spec  ... 証明課題生成仕様、コード生成対象
    |      +-pog-spec         ... 証明課題生成仕様、コード生成対象
    |      +-specman-spec     ... 仕様ファイル管理、ツール機能のインターフェース定義
    |      +-ss-spec          ... 型チェック仕様
    |      +-stdlib-spec      ... 標準ライブラリ仕様、コード生成用ファイル
    |      +-traces-spec      ... 組み合わせテスト仕様
    |      +-transform-spec   ... JavaVDM++変換仕様、一部コード生成対象
    |      +-uml-spec         ... UMLリンク仕様、コード生成対象
    |
    +-doc----latex            ... 清書機能に用いるスタイル


VDMTools の動作
=============================================================
構文解析／チェック
-------------------------------------------------------------
.. blockdiag::
  :width: 800

  blockdiag {
    （字句、構文解析） [style = dotted];

    VDM仕様ドキュメント -> （字句、構文解析） [dir = none];
    （字句、構文解析） -> 抽象構文木, 字句情報
  }

型チェック
-------------------------------------------------------------
.. blockdiag::
  :width: 800

  blockdiag {
    （型チェック） [style = dotted];

    抽象構文木, 字句情報 -> （型チェック） [dir = none];
    （型チェック） -> 型チェック結果, 型情報付字句情報
  }

インタープリタ
-------------------------------------------------------------
.. blockdiag::
  :width: 800

  blockdiag {
    （動的意味定義変換） [style = dotted];
    （インタープリタ実行） [style = dotted];

    抽象構文木, 字句情報 -> （動的意味定義変換） [dir = none];
    （動的意味定義変換） -> 動的意味定義, 中間コード
    動的意味定義, 中間コード -> （インタープリタ実行） [dir = none];
    （インタープリタ実行） -> 実行結果, カバレージ情報付構文情報
  }

清書機能
-------------------------------------------------------------
.. blockdiag::
  :width: 800

  blockdiag {
    （清書） [style = dotted];

    抽象構文木, カバレージ情報付字句情報 -> （清書） [dir = none];
    （清書） -> 清書文書（TEX、RTF）
  }

C++/javaコード生成
-------------------------------------------------------------
.. blockdiag::
  :width: 800

  blockdiag {
    （コード生成） [style = dotted];
    （ファイル出力） [style = dotted];

    抽象構文木, 型情報付字句情報 -> （コード生成） [dir = none];
    （コード生成） -> コード抽象構文木
    コード抽象構文木 -> （ファイル出力） [dir = none];
    （ファイル出力） -> "C++/javaソースコード"
  }

証明課題生成
-------------------------------------------------------------
.. blockdiag::
  :width: 800

  blockdiag {
    （証明課題生成） [style = dotted];

    抽象構文木, 型情報付字句情報 -> （証明課題生成） [dir = none];
    （証明課題生成） -> 証明課題
  }

UMLリンク
-------------------------------------------------------------
.. blockdiag::
  :width: 800

  blockdiag {
    （AUML変換） [style = dotted];
    （XMI読込） [style = dotted];
    （AUMLマージ） [style = dotted];
    （XMI書出） [style = dotted];
    "（VDM++変換）" [style = dotted];

    抽象構文木, 型情報付字句情報 -> （AUML変換） [dir = none];
    （AUML変換） -> AUMLデータ1
    読込XMIファイル -> （XMI読込） [dir = none];
    （XMI読込） -> AUMLデータ2
    AUMLデータ1, AUMLデータ2 -> （AUMLマージ） [dir = none];
    （AUMLマージ） -> 統合AUMLデータ [folded];
    統合AUMLデータ -> （XMI書出）, "（VDM++変換）" [dir = none];
    （XMI書出） -> XMIファイル
    "（VDM++変換）" -> "VDM++ファイル"
  }

.. 抽象構文木       -+-(AUML変換)-->AUMLデータ-+-(AUMLマージ)--> AUMLデータ -+-(XMI書出)----> XMIファイル
   型情報付字句情報 -+                         |                             +-(VDM++変換)--> VDM++ファイル
   XMIファイル      ---(XMI読込)--->AUMLデータ-+


ライブラリ構成
=============================================================
.. list-table::
   :widths: 300 300
   :header-rows: 1

   * - ライブラリ名
     - 役割
   * - libCG-xx.a
     - C++/javaコード生成
   * - libCG.a
     - C++生成コード用ランタイムライブラリ
   * - libMGR-xx.a
     - 仕様ファイル管理
   * - libPOG-xx.a
     - 証明課題生成
   * - libPOG_AUX-xx.a
     - 証明課題生成
   * - libPOG_PRETTY-xx.a
     - 証明課題生成
   * - libUML-xx.a
     - UMLリンク
   * - libcorba-xx.a
     - CORBAインターフェース
   * - libeval-xx.a
     - インタープリタ／清書
   * - libgen-xx.a
     - 仕様からのコード生成部分
   * - libj2v-xx.a
     - javaVDM変換
   * - libjss-xx.a
     - javaVDM変換
   * - libparser-xx.a
     - 構文解析
   * - libqtgui-xx.a
     - GUI
   * - libservman-xx.a
     - CORBAサービス(機能未使用)
   * - libspecfile-xx.a
     - 仕様文書管理
   * - libss-xx.a
     - 型チェック
   * - libtrans-xx.a
     - javaVDM変換
   * - libvdm.a
     - メタデータ


関連ソフトウエア
=============================================================
.. list-table::
   :widths: 300 300

   * - Qt
     - GUIライブラリ
   * - omniORB
     - CORBAライブラリ
   * - bison/flex
     - C/C++上の構文解析、字句解析ツール(Linux,Mac OS X付属)
   * - nuweb,fweb
     - ドキュメントツール
   * - javacc
     - java上の構文解析ツール
