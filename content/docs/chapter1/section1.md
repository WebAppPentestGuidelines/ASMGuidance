---
title: "1.1 ASM（Attack Surface Management）の必要性"
weight: 1
# bookFlatSection: false
bookToc: false
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
# 1.1 ASM（Attack Surface Management）の必要性
現代の企業や組織では、DX推進やクラウド利用の拡大、テレワークの普及により、IT資産が急速に増加しています。Webサイトの構築、公開が手軽にできたり、社内のストレージ増強が難しいときにはクラウドを一時的に利用できたりと、企業や組織が保有するIT資産は変化する頻度が高い状況にあります。  

このような状況においては、以下の課題があります。  

**昨今のIT資産管理の課題**

- 企業や組織のIT資産は変化する頻度が高く、従来どおりの管理（ルールベースでの管理）には限界がある
- IT資産の把握や管理がしきれず、管理の抜けや漏れが発生する

管理されていない（管理しきれていない）インターネット上のIT資産は攻撃者にとって絶好の標的です。例えば攻撃者が標的組織に侵入したいという目的で攻撃を仕掛ける場合、どこか1か所でも侵入可能な綻びを見つけられれば攻撃者の目的が達成できてしまうため、インターネットから見える「隙」を作るのは、守る側としては望ましくない状況です。  

上記課題への打ち手として、Attack Surface Management（ASM）が注目を集めています。  
国内では経済産業省、海外では米国CISA（CISA:Cybersecurity and Infrastructure Security Agency、サイバーセキュリティ・インフラストラクチャセキュリティ庁）など、権威ある組織からASM活用を後押しするドキュメントが発行されています。  

{{% hint info %}}
**補足**  
上記ドキュメントへのリンクは参考資料をご参照ください。  
{{% /hint %}}

なお、経済産業省が発行したドキュメントはEASM（External Attack Surface Management）に注目しており、米国CISAが発行したドキュメントは直接ASMに言及しているものではありませんが、ASMが得意とする領域に言及しているため、ASMの活用検討時に役立つドキュメントとなっています。  
（ASMが得意とする領域＝インターネットに公開すべきでないインターフェース、管理対象とすべき機器やサーバーやプロトコルの種別など）
