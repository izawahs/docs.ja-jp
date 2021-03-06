---
title: C# 開発者向けのバージョンと更新に関する考慮事項
description: 新しい言語機能をライブラリに導入すると、それを使用するコードに影響が出る可能性があります。
ms.topic: reference
ms.date: 09/19/2018
ms.openlocfilehash: f7db7c79792d04bcf592bc1858e1f0f05cb34402
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/17/2020
ms.locfileid: "88268128"
---
# <a name="version-and-update-considerations-for-c-developers"></a>C# 開発者向けのバージョンと更新に関する考慮事項

互換性は、新しい機能を C# 言語に追加するときの非常に重要な目標です。 ほとんどの場合、既存のコードは、新しいコンパイラのバージョンで問題なく再コンパイルできます。

新しい言語機能をライブラリに導入するときに、配慮が必要な場合があります。 最新バージョンで見つかった機能を使用して新しいライブラリを作成したが、コンパイラの以前のバージョンを使用してビルドされたアプリがそれを使用できることを保証しなければならない場合があります。 または、既存のライブラリをアップグレードしたが、多くのユーザーがバージョンをまだアップグレードしていない場合があります。 新しい機能の導入を検討するときは、互換性の 2 つのバリエーション (ソース互換とバイナリ互換) を考慮する必要があります。

## <a name="binary-compatible-changes"></a>バイナリ互換の変更

更新されたライブラリを、アプリケーションとそれを使用するライブラリのリビルドなしで使用できる場合、ライブラリの変更は**バイナリ互換**です。 依存するアセンブリのリビルドも、ソース コードの変更も必要ありません。 バイナリ互換の変更は、ソース互換の変更でもあります。

## <a name="source-compatible-changes"></a>ソース互換の変更

更新されたライブラリを使用するアプリケーションとライブラリではソース コードの変更を必要としませんが、ソースを正常に動作させるには、新しいバージョンで再コンパイルする必要がある場合、ライブラリの変更は**ソース互換**です。

## <a name="incompatible-changes"></a>互換性のない変更

変更が**ソース互換**と**バイナリ互換**のどちらでもない場合は、依存するライブラリとアプリケーションのソース コードの変更と再コンパイルが必要です。

## <a name="evaluate-your-library"></a>ライブラリを評価する

これらの互換性の概念は、ライブラリの内部実装ではなく、public 宣言と protected 宣言に影響します。 新しい機能の内部的な導入は、常に**バイナリ互換**です。  

**バイナリ互換**の変更では、古い構文の public 宣言と同じコンパイル済みコードを生成する新しい構文があります。 たとえば、メソッドを式形式のメンバーに変更することは、**バイナリ互換の変更**です。

元のコード:

```csharp
public double CalculateSquare(double value)
{
    return value * value;
}
```

新しいコード:

```csharp
public double CalculateSquare(double value) => value * value;
```

**ソース互換**の変更では、public メンバーのコンパイル コードを変更する構文が導入されますが、それは既存の呼び出しサイトと互換性がある方法で行われます。 たとえば、メソッドのシグネチャを値によるパラメーターから `in` 参照によるパラメーターに変更することはソース互換ですが、バイナリ互換ではありません。

元のコード:

```csharp
public double CalculateSquare(double value) => value * value;
```

新しいコード:

```csharp
public double CalculateSquare(in double value) => value * value;
```

[新機能](index.md)に関する記事に、public 宣言に影響する機能の導入がソース互換であるかバイナリ互換であるかについての注が含まれています。
