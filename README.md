# 工業会証明書デジタル化コンソーシアム

## プロジェクト概要
法人向けの政策税制においては、確定申告の信頼性を高めるために、第三者による証明が必要なことがあります。  
この証明に関する手続きを簡素化し、関係者間のデータ共有を促進することで、政策税制の運用を効率化し、活用を促進することを目指しています。

## フォルダ構成

-   01 【09_JISA】最終成果報告書\_20230323.docx
-   01 【09_JISA】別添 1 本システムで検証を行うデータ及びデータのやり取りの内容.xlsx
-   01 【09_JISA】別添 2 合意の履行のトレースの内容.xlsx
-   01 【09_JISA】別添 3 TrusterdWeb 実証結果シート.xlsx
-   01 【09_JISA】別添 4 申し送り課題一覧表.xlsx
-   02 【09_JISA】最終成果報告書概要版\_20230324.pptx
-   03 【09_JISA】成果概要\_20230324.pptx
-   04 【09_JISA】アプリケーション開発に係る成果物
    -   01 要件定義書\*20220324.pptx
    -   02 基本設計書\*20220324.pptx
    -   03 ソフトウェアソースコード及びスクリプト類
    -   04 ソフトウェアの使用前説明書・操作手順書（README）等
-   05 【09_JISA】デモ動画

## 動作環境
本実証実験で開発したシステム全体構成を以下に示します。  
なお、VC発行サイト毎に暗号鍵およびDIDドキュメントを生成するため、Microsoft Azureのテナントをサイト毎に分けてアプリケーションを構築しています。

![実験環境](./assets/実験環境.png)

* 主要な製品・ライブラリ一覧
<table>
  <thead>
    <tr>
      <th>コンポーネント名称</th> <th>フレームワーク</th>　 <th>実行環境</th>
    </tr>
  </thead>
  <tr>
    <td> 法人用wallet </td> <td>Node.js(Next.js, Express.js)</td><td> Node 16 LTS </td>
  </tr>
    <tr>
    <td> 個人用wallet </td> <td>Node.js(Next.js)</td><td> Node 16 LTS </td>
  </tr>
    <tr>
    <td> 証明書発行サイト </td> <td>Node.js(Express.js)</td><td> Node 16 LTS </td>
  </tr>
    <tr>
    <td> 証明書検証サイト </td> <td>Node.js(Express.js)</td><td> Node 16 LTS </td>
  </tr>
</table>

* 認証基盤  
  Azure AD B2C（サービス名：Azure Active Directory B2C）
​
## 各リポジトリの役割・機能

![データスキーム図](./assets/データスキーム図.jpg)

### 税務申告手順
1.	中小事業者は全国の IT 企業等の設備メーカー等へ 「SW 利用 VC」 の発行を依頼。  
2.	全国の IT 企業等の設備メーカー等は中小事業者へ 「SW 利用 VC」 を発行。中小事業者は法人 Wallet に 「SW 利用 VC」 を格納。  
3.	中小事業者は JISA 等の工業会等へ「工業会証明書 VC」 の発行を依頼。  
4.	JISA 等の工業会等は中小事業者へ 「SW 利用 VC」 の提示および「工業会証明書 VC」 を発行。中小事業者は法人 Wallet に「工業会証明書 VC」 を格納。  
5.	中小事業者は中小企業庁へ経営力向上計画に係る認定申請書の申請時に「計画認定 VC」 の発行を依頼。  
6.	中小企業庁は中小事業者へ 「SW 利用 VC」 および「工業会証明書 VC」 の提示および「計画認定 VC」 を発行。中小事業者は法人 Wallet に「計画認定 VC」 を格納。  
7.	中小事業者は所轄の税務署へ税務申告を実施  
8.	中小事業者は所轄の税務署へ 「SW 利用 VC」、「工業会証明書 VC」 および「計画認定 VC」 を提示  

### 各コンポーネントの役割・機能

* Holder  
  * 個人用wallet  
  法人用walletのログインに必要なwalletです。  
  事業者VCを格納し、中小事業者等に提示することで法人walletにログインすることができます。  
  * 法人用wallet  
  各団体から発行される工業会証明書などの証明書を保存し、税務申告ができます。  
* Issuer  
  * 全国の IT 企業等の設備メーカー等  
  SW 利用 VC を発行することができます。
  * 代表団体等の工業会等（証明団体）  
  SW 利用 VC を検証し、工業会証明書 VC を発行することができます。
  * 所管官庁（中小企業庁）  
  SW 利用 VC および工業会証明書 VC を検証し計画認定 VC を発行することができます。
* Verifier  
  * 所轄の税務署  
  SW 利用 VC、工業会証明書 VC および計画認定 VC を検証することができます。

## 実行環境について

### 前提条件
個人用walletについては、OSSである[Custom Identity Wallet](https://github.com/did-developer-community/custom-identity-wallet)を使用するため、実行環境の作成は必要ありません。  
認証基盤は別途設定が必要です。Azure AD B2Cを用いる場合は[こちら](https://github.com/Azure-Samples/active-directory-verifiable-credentials/tree/main/B2C)を参照の上設定してください。

### 実行環境の設定手順

動作環境を準備の上、任意の場所にソースコードを展開し、以下のページの手順に従って準備を行います。

* [法人用wallet](https://github.com/09-JISA/corp-wallet)

* [全国の IT 企業等の設備メーカー等](https://github.com/09-JISA/issuer-web-maker#setup)

* [代表団体等の工業会等（証明団体）](https://github.com/09-JISA/issuer-web-jisa#setup)

* [所管官庁（中小企業庁）](https://github.com/09-JISA/issuer-web-agency#setup)

* [所轄の税務署](https://github.com/09-JISA/verifier-web-taxoffice#setup)

## ソフトウェア操作手順書
本実証で作成したソフトウェアの説明書は下記に格納しております。実行環境を準備の上、参照してください。

[法人 Wallet 説明書](./04_%E3%80%9009_JISA%E3%80%91%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E9%96%8B%E7%99%BA%E3%81%AB%E4%BF%82%E3%82%8B%E6%88%90%E6%9E%9C%E7%89%A9/04_%E3%82%BD%E3%83%95%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A2%E3%81%AE%E4%BD%BF%E7%94%A8%E5%89%8D%E8%AA%AC%E6%98%8E%E6%9B%B8%E3%83%BB%E6%93%8D%E4%BD%9C%E6%89%8B%E9%A0%86%E6%9B%B8%EF%BC%88README%EF%BC%89%E7%AD%89/法人Wallet説明書.pdf)



## 注意事項

本アプリケーションは実証実験用のプロトタイプシステムとなり、実運用上での利用は保障されていません。
実運用においては、性能およびセキュリティ等についても別途検討が必要です。
​
​
