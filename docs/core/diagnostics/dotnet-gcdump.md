---
title: dotnet-gcdump - .NET Core
description: dotnet-gcdump コマンドライン ツールのインストールおよび使用。
ms.date: 07/26/2020
ms.openlocfilehash: a7b19f4d7487677b975621e7267a17894dae2e3a
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656652"
---
# <a name="heap-analysis-tool-dotnet-gcdump"></a>ヒープ分析ツール (dotnet-gcdump)

**この記事の対象:** ✔️ .NET Core 3.1 SDK 以降のバージョン

## <a name="install-dotnet-gcdump"></a>dotnet-gcdump のインストール

`dotnet-gcdump` [NuGet パッケージ](https://www.nuget.org/packages/dotnet-gcdump)の最新のリリース バージョンをインストールするには、次のように [dotnet tool install](../tools/dotnet-tool-install.md) コマンドを使用します。

```dotnetcli
dotnet tool install -g dotnet-gcdump
```

## <a name="synopsis"></a>構文

```console
dotnet-gcdump [-h|--help] [--version] <command>
```

## <a name="description"></a>説明

`dotnet-gcdump` グローバル ツールは、ライブ .NET プロセスの GC (ガベージ コレクター) ダンプを収集する手段です。 これには Windows の ETW に代わるクロス プラットフォームである EventPipe テクノロジが使用されています。 GC ダンプを作成するには、ターゲット プロセスで GC をトリガーし、特殊なイベントを有効にし、イベント ストリームからオブジェクト ルートのグラフを再生成します。 このプロセスにより、プロセスの実行中のオーバーヘッドを最小限に抑えながら GC ダンプを収集できます。 このようなダンプは、いくつかのシナリオで役立ちます。

- 複数の時点でヒープ上にあるオブジェクト数を比較する。
- オブジェクトのルートを分析する ("この種類に対する参照がまだ残っているものはあるか" などの質問に回答する場合)。
- ヒープ上のオブジェクト数に関する一般的な統計情報を収集する。

### <a name="view-the-gc-dump-captured-from-dotnet-gcdump"></a>dotnet-gcdump からキャプチャされた GC ダンプを表示する

Windows では、分析のために [PerfView](https://github.com/microsoft/perfview) で、または Visual Studio で `.gcdump` ファイルを表示できます。 現在、Windows 以外のプラットフォームで `.gcdump` を開く方法はありません。

複数の `.gcdump` を収集し、それらを Visual Studio で同時に開いて、比較を行うことができます。

## <a name="options"></a>Options

- **`--version`**

  `dotnet-gcdump` ユーティリティのバージョンを表示します。

- **`-h|--help`**

  コマンド ライン ヘルプを表示します。

## `dotnet-gcdump collect`

現在実行中のプロセスから GC ダンプを収集します。

### <a name="synopsis"></a>構文

```console
dotnet-gcdump collect [-h|--help] [-p|--process-id <pid>] [-o|--output <gcdump-file-path>] [-v|--verbose] [-t|--timeout <timeout>] [-n|--name <name>]
```

### <a name="options"></a>オプション

- **`-h|--help`**

  コマンド ライン ヘルプを表示します。

- **`-p|--process-id <pid>`**

  GC ダンプを収集するプロセス ID。

- **`-o|--output <gcdump-file-path>`**

  収集された GC ダンプが書き込まれるパス。 既定値は *.\\YYYYMMDD\_HHMMSS\_\<pid>.gcdump* です。

- **`-v|--verbose`**

  GC ダンプの収集時にログを出力します。

- **`-t|--timeout <timeout>`**

  この秒数より長くかかる場合は、GC ダンプの収集をあきらめてください。 既定値は 30 です。

- **`-n|--name <name>`**

  GC ダンプを収集するプロセスの名前。

## `dotnet-gcdump ps`

GC ダンプを収集できる dotnet プロセスを一覧表示します。

### <a name="synopsis"></a>概要

```console
dotnet-gcdump ps
```

## `dotnet-gcdump report <gcdump_filename>`

以前に生成された GC ダンプまたは実行中のプロセスからレポートを生成し、`stdout` に書き込みます。

### <a name="synopsis"></a>構文

```console
dotnet-gcdump report [-h|--help] [-p|--process-id <pid>] [-t|--report-type <HeapStat>]
```

### <a name="options"></a>オプション

- **`-h|--help`**

  コマンド ライン ヘルプを表示します。

- **`-p|--process-id <pid>`**

  GC ダンプを収集するプロセス ID。

- **`-t|--report-type <HeapStat>`**

  生成するレポートの種類。 使用可能なオプション: heapstat (既定値)。

## <a name="troubleshoot"></a>トラブルシューティング

- gcdump に型情報はありません。

   .NET Core 3.1 より前のバージョンでは、EventPipe を使って呼び出されたときに、gcdump 間で型キャッシュがクリアされない問題がありました。 これにより、型情報を判別するために必要なイベントが 2 番目以降の gcdump に対して送信されなくなりました。 これは .NET Core 3.1-preview2 で修正されました。

- COM 型と静的な型は GC ダンプに含まれていません。

   .NET Core 3.1-preview2 より前のバージョンでは、EventPipe を介して GC ダンプが呼び出されたときに静的な型と COM 型が送信されない問題がありました。 これは .NET Core 3.1-preview2 で修正されました。
