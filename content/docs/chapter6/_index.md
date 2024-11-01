---
title: "第6章　ASM活用事例"
description: "ASM導入検討を進めるためのガイダンス（基礎編）"
weight: 6
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
# 第6章　ASM活用事例

## 事例1

### 企業Aの背景

- 企業Aは国内外で多くのITサービスを展開しており、その統制は国内と海外で分かれている
- 国内は[3ラインモデル](https://www.iiajapan.com/leg/pdf/data/iia/2020.07_1_Three-Lines-Model-Updated-Japanese.pdf)により統制、海外のグループ会社は各社に運用管理を一任している

### 企業AのIT資産管理状況

- 国内は上記背景のとおり統制しており、IT資産をある程度管理している
- 国内の事業部門が利用するドメインは、おおよそ単一部署が管理しているが、全ドメインを把握しているわけではない
- 国内の事業部門が利用するインフラは、各事業の特性に応じて、オンプレ、クラウド(AWS、GCP、AZURE、ORACLE、その他)上に構築されており、**どのようなアセットがあるかは一元的に収集されていない**
- 統制を担うガバナンス部門（第2線）では、国内、海外とも、どのようなホスト(FQDN, IP等)があるか、どんなコンポーネントを利用しているかなど、**全体の可視化ができてはおらず、現場(第一線)の運用に依存しているという状態**

### 課題

上記のような統制をする中で、以下の問題や課題が生じている。

1. 自組織に対する外部からのスキャンやOSINTによる調査結果が公開・報道されるケースの増加
    - 管理対象の資産も多く、外部からのスキャンは抑止できない（構っていられない）
    - 先んじて対策を行おうにも、IT資産全体の可視化が出来ていないため、ガバナンス部門（第2線）が把握できる範囲が限られてしまっている。
    - 海外の事業部門は統制による対策が進めづらい
2. サイバー攻撃がよりHuman Operatedにシフトした傾向があり、攻撃者目線で自組織を評価し対策を講じる必要がある
    - 特に「攻撃の初期段階(Reconnaissance, Initial Access)」の観点から現状を把握する必要がある
 
### ASM導入に関する要件定義

課題1、2を解決するため、以下を主軸に要件を細分化をした。
- 機能観点
    - 資産検出
        - 攻撃者と同様の手段で、幅広くIT資産を再帰的に検知、把握できること　
        - 特に、DNS, ASN, IP, SSL, Whois, etc
        - 対象のインフラを限定しないこと（オンプレ、クラウドどちらも対象にできること）
    - セキュリティ評価
        - 攻撃者がよく利用する、攻撃の初期段階の観点で評価できること
        - 検出結果に対してリスク評価を行い、対策の優先順位付けができること

また、ASMを運用するメンバーが1名だったため、運用負荷を減らすためにも以下も重要視した。
- 運用観点
    - 結果の信頼性
        - 誤検知（偽陽性）の確認、データの信頼性の確認が必要
        - Raw情報に近い情報が提供されていること
    - 履歴管理
        - 履歴情報が提供されていること（資産情報、脆弱性のいずれも）
    - 対策推進
        - アクションに繋りやすい状態での情報が提供されていること
        - 検知理由が明瞭、是正策が提示されているなど
    - 他システム統合
        - 他システムと連携しやすいこと
        - チケット管理システムの接続、APIが公開されているかなど

### 導入前に整理した点

ASMの運用プロセス全体を整理してから開始するのは工数的に難しいため、初期の整理として、ASMツールを導入した際にハレーションが起きない/小さくできるように組み立て、動きながら改善していく方針とした。

- 想定した"起こしたくない"ハレーション
    - ASMツールによるアクセスの増加により、サービスサイドのモニタリング（アクセス分析など）に影響する
    - ASMツールによるアクセスでセキュリティアラートが上がり、サービスサイドの調査工数が増える
    - 上記によるサービスサイドからガバナンスサイドへの問い合わせの増加
    - 情報共有不足による問い合わせ一次受け組織の負担増
    - 他の脆弱性管理施策との整合性の不一致

- 上記への解決策
    - 対象範囲の決定
    - コミュニケーションフロー
        - 脆弱性検査と異なり、ASMツールでは検出したアタックサーフェスがどの事業部門のものであるのかが不明なことがある
        - そのため、持ち主不明のアタックサーフェスが見つかった場合に誰が主体として動いてもらえるのかの整理が重要である
        - 幸いにも3ラインモデルにより、第1線に責任者がいるため、各責任者がハンドリングを一任する合意形成を行った
    - 対象サービスへの事前共有・周知
        - 社内のコミュニケーションラインでの通知やサービスオーナーへの共有を実施
        - 更にASMツールによって、サービスサイドのエンジニアの負担が増加するのは本意では無いため、エンジニア向けにも周知や自動通知botの設定を実施
    - 脆弱性のリスクレベルの定義
        - 重視すべきリスクの整理（サービスサイドが対応すべきリスクの明確化、受容するリスクの整理）を実施
        - 発見された問題点全てに対応依頼をするのではなく、予め対応すべき問題点（レベル感）を決定し、合意形成を行った

### 導入後に整理した点

運用していく中で以下は継続して整理・改善を行う必要があった。

- 脆弱性のリスクレベルの見直し
    - 新規の脆弱性が発見された際の判断（基準への取り込み）
    - 最新の脅威動向を踏まえた対応レベルの見直し
    - 他施策とコンフリクトしないようなレベルの調整
- コミュニケーション・運用フローの改善
    - 担当者へ共有する情報粒度の見直し
    - 体制変更に伴う担当者整理
    - 関係者からのQAを反映
    - コミュニケーション履歴の保存

### 導入効果

上記の取り組みにより、ASMの導入・継続的な運用ができ、課題解決に寄与する結果となった。

- ガバナンス部門でのIT資産全体の可視性が向上
    - 緊急対応が必要な脆弱性の公表時など、必要な時にASM上でどのような状態を検索可能となった
    - 脆弱性スキャンなど個別の調査結果とASMの結果を組み合わせることで、状況を踏まえた対策を打てる状態になった
- 事業部門でも外部公開に伴うリスクへの理解が深まった
    - 事業部門も含めた運用プロセス（コミュニケーション、対策推進）が実現でき、調整過程で事業部門側の理解も深まった

一方、課題もある程度残っている。

- IT資産数が膨大だと運用負荷も膨大（管理対象数に比例して運用負荷も高まる）
    - 誤検知の処理
    - 再検知の処理
    - 関係者へのコミュニケーションなど
- ASMだけでは全ては解決しない
    - 検出できないIT資産も存在する（IT資産数が多すぎると漏れは必ず発生する）
    - 細かなIT資産管理をしたい場合には情報が不十分（例：EASMでは導入されているソフトウェアなどの情報は取得不可）
    - セキュリティ評価も実施する場合、対象となるアタックサーフェス（サービス）の負荷も考慮する必要がある
    - 他のセキュリティソリューションと同様、リスクが顕在化しない場合は費用対効果の説明が難しい