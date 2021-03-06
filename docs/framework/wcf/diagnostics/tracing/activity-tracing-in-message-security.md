---
title: メッセージ セキュリティにおけるアクティビティ トレース
ms.date: 03/30/2017
ms.assetid: 68862534-3b2e-4270-b097-8121b12a2c97
ms.openlocfilehash: bb8a4c6782cc52de393eacc2458e216d0f069866
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/22/2019
ms.locfileid: "69933521"
---
# <a name="activity-tracing-in-message-security"></a>メッセージ セキュリティにおけるアクティビティ トレース
ここでは、次の 3 つの段階で発生するセキュリティ処理のアクティビティ トレースについて説明します。  
  
- ネゴシエーション/SCT 交換。 トランスポート層 (バイナリデータ交換を使用した場合) またはメッセージ層 (SOAP メッセージ交換を使用した場合) で発生する可能性があります。  
  
- メッセージの暗号化と復号化、および署名の検証と認証。 トレースは、アンビエント アクティビティ (通常、"プロセス アクション") に表示されます。  
  
- 承認と検証。 ローカルで、またはエンドポイント間の通信中に発生する可能性があります。  
  
## <a name="negotiationsct-exchange"></a>ネゴシエーション/SCT 交換  
 ネゴシエーション/SCT 交換フェーズでは、クライアントで次の2つのアクティビティの種類が作成されます。"セキュリティで保護されたセッションの設定" と "セキュリティで保護されたセッションを閉じる"。 "セキュリティで保護されたセッションの設定" は RST/RSTR/SCT メッセージ交換のトレースを含み、"セキュリティで保護されたセッションを閉じる" は "キャンセル" メッセージのトレースを含みます。  
  
 サーバーでは、RST/RSTR/SCT の各要求/応答がそのアクティビティに表示されます。 サーバーとクライアントの両方で、サーバー上のアクティビティは同じIDを持ち、サービストレースビューアーで表示すると、"セキュリティで保護されたセッションのセットアップ"に一緒に表示されます。`propagateActivity` = `true`  
  
 このアクティビティ トレース モデルは、ユーザー名/パスワード認証、証明書認証、および NTLM 認証に対して有効です。  
  
 ネゴシエーションと SCT 交換のアクティビティとトレースを次の表に示します。  
  
||ネゴシエーション/SCT 交換の発生タイミング|アクティビティ|トレース|  
|-|-------------------------------------------------|----------------|------------|  
|セキュリティで保護されたトランスポート<br /><br /> (HTTPS、SSL)|最初のメッセージを受信したとき。|トレースは、アンビエント アクティビティで出力されます。|-Exchange トレース<br />-セキュリティで保護されたチャネルが確立済み<br />-取得したシークレットを共有します。|  
|セキュリティで保護されたメッセージ層<br /><br /> (WSHTTP)|最初のメッセージを受信したとき。|クライアント側 :<br /><br /> -"セキュリティで保護されたセッションのセットアップ" は、RST/RSTR/SCT の各要求/応答に対して、最初のメッセージの "プロセスアクション" から除外されます。<br />-"プロキシアクティビティを閉じる" については、[exchange のキャンセル] の [セキュリティで保護されたセッションを閉じる] を使用します。 このアクティビティは、セキュリティで保護されたセッションが閉じられるタイミングにより、他のアンビエント アクティビティで発生する可能性があります。<br /><br /> サーバー側 :<br /><br /> -サーバー上の RST/SCT/Cancel の要求/応答ごとに1つの "処理アクション" アクティビティ。 の`propagateActivity`場合、RST=/RSTR/SCT アクティビティは "セキュリティセッションのセットアップ" にマージされ、Cancel はクライアントからの "Close" アクティビティとマージされます。 `true`<br /><br /> "セキュリティで保護されたセッションの設定" には 2 つの段階があります。<br /><br /> 1. 認証ネゴシエーション。 クライアントが既に適切な資格情報を持つ場合は、省略可能です。 この段階は、セキュリティで保護されたトランスポート、またはメッセージ交換を通じて行われます。 後者の場合、1 つまたは 2 つの RST/RSTR 交換が発生する可能性があります。 これらの交換では、トレースは、従来の設計どおり、新しい要求/応答アクティビティで出力されます。<br />2. セキュリティで保護されたセッションの確立 (SCT)。この段階では、1 つの RST/RSTR 交換が発生します。 前述のように、これには同じアンビエント アクティビティが含まれます。|-Exchange トレース<br />-セキュリティで保護されたチャネルが確立済み<br />-取得したシークレットを共有します。|  
  
> [!NOTE]
> 混在セキュリティ モードでは、ネゴシエーション認証はバイナリ交換で発生しますが、SCT はメッセージ交換で発生します。 純粋なトランスポート モードでは、ネゴシエーションは追加のアクティビティを持たないトランスポートでのみ発生します。  
  
## <a name="message-encryption-and-decryption"></a>メッセージの暗号化と復号化  
 メッセージの暗号化と復号化および署名認証のアクティビティとトレースを次の表に示します。  
  
||セキュリティで保護されたトランスポート<br /><br /> (HTTPS、SSL) およびセキュリティで保護されたメッセージ層<br /><br /> (WSHTTP)|  
|-|---------------------------------------------------------------------------------|  
|メッセージの暗号化/復号化、および署名認証の発生タイミング|メッセージを受信したとき。|  
|アクティビティ|トレースは、クライアントとサーバーの ProcessAction アクティビティで出力されます。|  
|トレース|-sendSecurityHeader (送信者):<br />-署名メッセージ<br />-要求データを暗号化する<br />-receiveSecurityHeader (受信側):<br />-署名の確認<br />-応答データの暗号化解除<br />-認証|  
  
> [!NOTE]
> 純粋なトランスポート モードでは、メッセージの暗号化/復号化は追加のアクティビティを持たないトランスポートでのみ発生します。  
  
## <a name="authorization-and-verification"></a>承認と検証  
 承認のアクティビティとトレースを次の表に示します。  
  
||承認の発生タイミング|アクティビティ|トレース|  
|-|-------------------------------------|----------------|------------|  
|ローカル (既定)|サーバーでメッセージが複号化された後|トレースは、サーバーの ProcessAction アクティビティで出力されます。|ユーザーの承認。|  
|Remote|サーバーでメッセージが複号化された後|トレースは、ProcessAction アクティビティによって呼び出された新しいアクティビティで出力されます。|ユーザーの承認。|
