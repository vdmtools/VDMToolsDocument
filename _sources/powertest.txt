=============================================================
Powertest実行方法
=============================================================

はじめに
=============================================================
``powertest`` はVDMToolsの回帰テストを行うためのツールである。
python により記述されたプログラムと、テストデータで構成されている。

基本的なディレクトリは以下のように仮定する。

* ツールをビルドするディレクトリ： ``$HOME/vdmwork``
* ツールのソースなどsubversionで取得するリポジトリ： ``$HOME/toolbox/trunk``
* リポジトリのディレクトリを ``$TBDIR`` と記述する(TBDIR=$HOME/toolbox/trunk)
* ツールがビルドされた状態でテストを実行する


環境設定
=============================================================
Linux、Mac OS X
---------------------------------------
CORBA を利用した API テストを行う場合は、``omniORBpy`` をインストールする。

.. code-block:: bash

  $TBDIR/test/powertest において
  $ make

を実行する

APIテストを行わない場合は、 ``$TBDIR/test/powertest/runtest.py`` の

.. code-block:: python

  import apirun

の部分をコメントアウトする。


Windows
---------------------------------------
powertestの実行はCygwin環境下で行うため、CygwinでPythonをインストールしておく。
APIテストはうまく動作しないので、 ``$TBDIR/test/powertest/runtest.py`` の

.. code-block:: python

  import apirun

の部分をコメントアウトする。

Javaコード生成を行う場合は、C:\\Program Files\\java 内の jdk ディレクトリを Cygwin の /usr/java 内にコピーし、
``default`` という名前のリンクを作成しておく。

例：

.. code-block:: bash

  $ cd /usr
  $ mkdir java
  $ cp /cygdrive/c/Program\ Files/Java/jdk_1.*.*_** java/.
  $ ln -s jdk1.7.0_15 default


3. テスト用ワークディレクトリ作成
=============================================================
テストを行うためのワークディレクトリを任意の場所に作成する。


4. 設定ファイル作成
=============================================================
ワークディレクトリに、設定ファイル ``.powertest-setup`` を作成する。
通常は、 ``$TBDIR/test/powertest/default-setup`` をコピーして編集する。

``$TBDIR/code/setup/pt_xxx.tar.bz2`` は OS 毎に各種のテストを設定してあるので
展開して利用する。

主な編集項目を以下に示す

* What-To-Run

  * テストの項目の指定を行う
  * ``TC、IP、CPP、Java、Api、metaiv、POG`` が指定可能

* Language

  * テスト対象の言語の指定を行う
  * ``SL、PP、RT`` が指定可能

* Run-Type

  * 実装か仕様の指定を行う
  * ``impl、spec`` が指定可能
  * spec を指定する場合は、What-To-Runは ``TC、IP、CPP、Java、POG`` のいずれか

* compiler

  * C++ テストで使用するコンパイラを指定する
  * ``Windows では自動的に cl.exe が使用されるので指定は不要``

* cflag

  * OS によってC++コンパイラのオプションが異なるので、OS にあわせてフラグを選択する

その他テスト項目ごとに指定があるが、前述の仮定ディレクトリ設定下では変更の必要はない。

PP の ip テストを行う場合、ツールのビルドディレクトリで ``dlclass_test1.so`` を作成し、
テストディレクトリにコピーする。

PP の CPP テストを行う場合は、ツールのビルドディレクトリでLinux、Mac OS X では`` dlclass_test1.o`` 、
Windows では ``dlclass_test1.obj`` を作成し、テストディレクトリにコピーする。

Windows で CPP テストを行う場合、配布パッケージの ``cg/include`` 内のファイルをテストディレクトリに
コピーする。

Windows で java テストを行う場合は、 ``VDM.jar`` をテストディレクトリにコピーする。


5. テスト実行
=============================================================
テスト用ワークディレクトリ内で以下のシェルスクリプトを実行する

.. code-block:: bash

  $TBDIR/test/powertest/powertest


6. 結果確認
=============================================================
テストが終了すると、テストディレクトリ内には、テストプログラム、結果とともに、
``report-日付、errors-日付`` というファイルが作成され、最新のものが report、errorsというリンクファイルが
作成される。
