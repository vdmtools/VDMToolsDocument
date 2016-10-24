=============================================================
VDMToolsの開発方法
=============================================================

.. contents:: 目次
  :local:
  :depth: 1


VDMTools 開発環境構築
=============================================================
VDMTools の開発は以下のプラットフォームをサポートしている。

* Mac OS X 10.5/10.6/10.7/10.8
* Linux Ubuntu (10.4～)、Fedora (15～)、Debian 6
* Windows XP/Windows 7

以下の環境においても開発可能である。

* Soralis 10 (x86)
* Freebsd 8/9

.. note::
  * 現時点で Windows で単独開発は行えず、Linux等のサーバーを必要とする
  * VMware などの仮想環境上でサーバーを動かして開発することは可能
  * Mac OS X 10.7 以降は samba ではなく smbfs が使用されていて、現時点ではサーバーとして利用できない


開発ツール設定
=============================================================
Linux
---------------------------------------
Linux はディストリビューションにより、標準インストールされるソフトウエアが異なる。

基本的に以下のパッケージを必要とするので、ディストリビューションのパッケージ管理ソフトを
使用して以下のパッケージの導入確認、インストールを行う。

* g++
* git
* bison
* flex
* texinfo
* filepp
* openjdk-7-jdk
* ant
* python-dev（バージョン2系）
* libxml2-dev
* libncurses5-dev
* libreadline-dev
* unixODBC-dev
* libqt4-dev

.. warning::
  Fedora Linux において、 ``openjdk-dev`` パッケージを導入した場合は、idlj コマンドに対して /usr/bin/javac を
  参考にして同じようにリンクを作成しておく。


Mac OS X
---------------------------------------
Mac OS X は、そのOSバージョンに対応する最新の Xcode をインストールする。
10.7 以降は、Xcode を起動し ``Command Line Tools`` のインストールを行う。

.. warning::
  10.8では10.8SDKの ``usr/include`` に ``FlexLexer.h`` が含まれないので、10.7SDK からコピーして使用する。
  さらに、10.8 の flexの出力は FlexLexer.h と互換性がないので以下の部分を修正する。

修正前：(133行目)

.. code-block:: c

   virtual int LexerInput( char* buf, int max_size );
   virtual void LexerOutput( const char* buf, int size );

修正後：

.. code-block:: c

   virtual size_t LexerInput( char* buf, size_t max_size );
   virtual void LexerOutput( const char* buf, size_t size );

Windows XP/7
---------------------------------------
Windows 環境では以下のバージョンの VisualStudio での開発をサポートしている。

* VisualStudio 2005 SP1
* VisualStudio 2008 SP1
* VisualStudio 2010 SP1

  * Visual Studio 2010は、 `NonSoft - Visual Studio 2010 Expressのダウンロードとインストール <http://nonsoft.la.coocan.jp/Chinamini/20110001/20110308.html>`_ にあるURLから取得可能

VisualStudio から VC++ パッケージをインストールする。

.. warning::
  VisualStudio 2010 の場合、VC++ パッケージのみを選択すると必要なファイルがインストールされないようなので、
  C# も選択してインストールすること。

以下の環境変数の設定を行う。

* INCLUDE
* LIB
* PATH

次に、以下のツールをそれぞれダウンロード→インストールする。

* `Microsoft Windows SDK for Windows 7 and .NET Framework 4 (ISO) <https://www.microsoft.com/en-us/download/details.aspx?id=8442>`_
* `Microsoft Visual Studio 2010 Service Pack 1 (インストーラー)  <https://www.microsoft.com/ja-jp/download/details.aspx?id=23691>`_
* `Windows SDK 7.1 用 Microsoft Visual C++ 2010 Service Pack 1 コンパイラ更新プログラム  <https://www.microsoft.com/ja-jp/download/details.aspx?id=4422>`_

Windows での開発作業は Cygwin のシェル上で行うので、``Cygwin`` もインストールしておく。
以下のパッケージを選択する。

* binutils
* make
* perl
* python
* subversion

Cygwin の ``/usr/bin/link.exe`` コマンドが VisualStudio の link.exe の代わりに起動されることがあるので、
名前を変更しておく。

さらに以下のパッケージをインストールする

* Python 2.7.3
* ActivePerl 5.14/5.16
* 最新版のJDK
* NSIS 3.0


開発リポジトリの取得
=============================================================
Linux、Mac OS X、Cygwin のシェル環境において開発リポジトリの取得を行う。
以下、開発ディレクトリを **$HOME/vdmtools** とする。

VDMTools 開発用のリポジトリ取得
---------------------------------------
Subversion コマンドを使用してVDMTools 開発用のリポジトリを取得する。

.. code-block:: bash

  $ cd $HOME
  $ git clone https://mas0061@bitbucket.org/mas0061/vdmtools.git


環境変数設定
=============================================================
開発に必要な環境変数の設定を行う。

``<開発用リポジトリ>/code/setup`` に各プラットフォームに対応したシェル環境設定ファイルの
サンプルがあるので、それを修正して使用する。

.bashrc や .profile を修正した後、シェルを再起動する。


開発用コマンド、ライブラリのインストール
=============================================================
Linux、Mac OS X
---------------------------------------
インストール中にエラーとなる場合は、パッケージや、環境変数を見直す。

nuweb
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
``<開発用リポジトリ>/code/tools/nuweb-1.58.tar.gz``

インストール方法：

.. code-block:: bash

  $ tar xvf nuweb-1.58.tar.gz
  $ cd nuweb-1.58
  $ make nuweb
  $ make
  $ sudo install -s nuweb /usr/local/bin

fweb
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
``<開発用リポジトリ>/code/tools/fweb-1.62.tar.bz2``

インストール方法：

.. code-block:: bash

  $ tar xvf fweb-1.62.tar.bz2
  $ cd fweb-1.62/Web
  $ ./configure
  $ make
  $ sudo make install

javacc
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
``<開発用リポジトリ>/code/tools/javacc-5.0src.tar.gz``

インストール方法 ： ※ここでは ``/usr/local/javacc-5.0`` にインストールするものとする。

.. code-block:: bash

  $ tar zxf javacc-5.0src.tar.gz
  $ cd javacc
  $ ant
  $ makedist 5.0
  $ sudo tar xvf javacc-5.0.tar.gz -C /usr/local

/usr/local 以外の場所にインストールする場合は、 ``JAVACCHOME`` 環境変数を修正する。

omniORB のインストール
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
``omniORB-4.2.1-2.tar.bz2`` および ``omniORBpy-4.2.1-2.tar.bz2`` をインストール。
上記2ファイルは、インターネットから取得。

ここでは ``/usr/local/omniORB`` にインストールするものとする。

インストール方法：

.. code-block:: bash

  $ tar xvf omniORB-4.2.1-2.tar.bz2
  $ cd omniORB-4.2.1
  $ ./configure --prefix=/usr/local/omniORB
  $ make
  $ sudo make install

  $ tar xvf omniORBpy-4.2.1-2.tar.bz2
  $ cd omniORBpy-4.2.1
  $ ./configure --prefix=/usr/local/omniORB
  $ make
  $ sudo make install

omniORBpyのコンパイルに失敗した場合は、アーカイブの展開から行う。

/usr/local 以外の場所にインストールする場合は、 ``CORBAHOME`` 環境変数を修正する。

..  Qt のインストール(Mac OS Xのみ)
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  ``/usr/local/qt4`` に ``Qt 4.8.4`` をインストールする。

  インストール方法：

  .. code-block:: bash

    $ tar zxf qt-everywhere-commercial-src-4.8.4.tar.gz
    $ cd qt-everywhere-commercial-src-4.8.4
    $ ./configure -cocoa -sdk $SDK -no-webkit -prefix /usr/local/qt4
    $ make
    $ sudo make install

  Mac OS X 10.5 の場合は、 ``-arch x86 -arch ppc -platform macx-g++42`` をオプションに追加する。

VDMTools のインストール
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
VDM++ と VDM-SL パッケージをインストールする。

<開発用リポジトリ>/code/tools の vdmsl と vdmpp を /usr/local に展開。

環境変数 ``CG_SL_CMD`` に ``vdmde`` のフルパス、
環境変数 ``CG_PP_CMD`` に ``vppde`` のフルパスを設定する。

設定しない場合は、
``CG_SL_CMD=/usr/local/bin/vdmsl、CG_PP_CMD=/usr/local/bin/vdmpp``
がデフォルト値となる。


5.2 Windows
---------------------------------------
.. todo:: 未メンテナンス

libxml のインストール
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
``<開発用リポジトリ>/code/win32/libxml2/libxml2-2.7.x.zip`` を展開し ``C:\libxml2`` に配置する。

.. code-block::none

  C:/libxml2/bin
            /include
            /lib

さらに ``iconv-xxx.zip`` と ``zlib-xxx.zip`` も展開し、それぞれ対応するディレクトリに内容をコピーする。

64ビット版Windows の場合は ``<開発用リポジトリ>/code/win32/libxml2_x64`` のファイルを配置する。

omniORB のインストール
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
#. ``omniORB-4.2.1-2.tar.bz2`` を展開し、 ``C:\omniORB-4.2.1-2`` に配置する
#. ``C:\omniORB-4.2.1-2\mk\platforms`` 内の ``x86_win32_vs?.mk`` の中で、開発に使用する VisualSutudio のバージョンに
   対応するファイルを修正する

   * VisualStudio 2005 -> ``x86_win32_vs8.mk``
   * VisualStudio 2008 -> ``x86_win32_vs9.mk``
   * VisualStudio 2010 -> ``x86_win32_vs10.mk``

   修正個所は PYTHON の設定のみ。
   先に ``C:\Python27`` にインストールしていれば、

   .. code-block::bash

     PYTHON=/cygdrive/c/Python27/python

   となる。

#. ``C:\omniORB-4.2.1-2\config\config.mk`` を修正する

   * ``platform =`` で開発に使用する VisualSutudio のバージョンに対応する設定のコメントを外す(先頭の#を消す)

#. Cygwin のターミナルで ``/cygdrive/c/omniORB-4.2.1-2/src`` へ移動する
#. ``make export`` を実行する

Qt のインストール
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
#. `Qtのダウンロードサイト <http://download.qt.io/official_releases/qt/4.8/4.8.7/>`_ から、 ``qt-everywhere-opensource-src-4.8.7.zip`` をダウンロード
#. ``qt-everywhere-opensource-src-4.8.7.zip`` を展開し ``C:\Qt\4.8.4`` に配置する
#. コマンドプロンプト(Cygwinは不可)で、C:\Qt\4.8.4 に移動する。
#. ``configure -no-webkit`` を実行する

   * VisualStudio 2005 の場合は ``-qt-zlib`` オプションを追加する

#. nmake を実行する

   * 64 ビット版の場合、ビルド後に Cygwin で ``/cygdrive/c/Qt/4.8.4/bin/`` および ``/cygdrive/c/Qt/4.8.4/plugins/codecs/``
     内の \*.dll ファイルのパーミッションを確認し、なければ追加しておく
   * パーミッションがないとパッケージ作成の際にエラーとなる

VDMTools のビルド
=============================================================
ワークディレクトリの作成
---------------------------------------
ツールをビルドするためのワークディレクトリを作成する。

Linux、Mac OS X 上でツールをビルドする場合と、 Windows のサーバーで、構成が異なるので、ディレクトリは
別にしておく。

.. code-block:: bash

  $ mkdir vdmwork

Linux、Mac OS X 上で VDMTools をビルドする場合
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
以下のコマンドを実行する。

.. code-block:: bash

  $ <開発用リポジトリのパス>/code/make_tools/lnconf -i BASEDIR=<開発用リポジトリのパス>/code

.. code-block:: bash

  $ ../toolbox/trunk/code/make_tools/lnconf -i <開発用リポジトリのパス>/code

BASEDIR はフルパスで指定しておく。

Linux、Mac OS X を Windows 開発のサーバーとする場合
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
以下のコマンドを実行する

.. code-block:: bash

  $ <開発用リポジトリのパス>/code/make_tool/lnconf -i BASEDIR=<開発用リポジトリのパス>/code OSTYPE=win32

Windows でツールをビルドする場合
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
サーバーの準備ができてから以下のコマンドを実行する。

.. code-block:: bash

  $ <サーバーのワークディレクトリのパス>/lnconf -i -w VPATH=<サーバーのワークディレクトリのパス>

この操作により GNUmakefile と inienv が作成される。

.. code-block:: bash

  $ //ip_address_of_server/vdmwork_vs/lnconf -i -w VPATH=//ip_address_of_server/vdmwork_vs

Windows 7 では、エクスプローラからサーバーに接続しておかないとうまく動かない。


コード生成(Linux、Mac OS X)
---------------------------------------
vdm の仕様からツールをビルドするのに必要なソースファイルの生成を行う。

仕様の中間ファイルの削除
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: bash

  $ make specclean

この操作は、仕様の中間ファイルの削除を行う
仕様を修正した場合に実行する

ソースファイルの生成
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: bash

  $ make gencode >& Logs & tail -f Logs

コード生成終了後エラーがないことを確認する。
Errors クラスと ErrorState クラスに関する出力はエラーではない。

開発サーバの場合の追加処理
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: bash

  $ make clean
  $ make ntinit


ツールのビルド(Linux、Mac OS X)
---------------------------------------
ワークディレクトリ内で以下のコマンドを実行する

.. code-block:: bash

  $ make clean
  $ make distall


ツールのビルド(Windows)
---------------------------------------
ワークディレクトリ内で以下のコマンドを実行する

.. code-block:: bash

  $ source inienv
  $ make clean
  $ make setupall
