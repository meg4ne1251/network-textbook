# ネットワーク技術教科書

「入門書は卒業したが、次の体系的な一冊が欲しい」という読者のための、
ネットワーク技術の個人学習用教科書です。

VLAN やルーティングといった言葉の意味は分かるものの、体系立てて理論から学んだ
わけではない、という段階の学習者を想定しています。資格試験対策や設定コピペ集ではなく、
**RFC・プロトコルの動作原理まで踏み込んで「なぜそう動くのか」を説明すること**を
目標にしています。

- なぜ iBGP はフルメッシュが必要なのか、それを崩すと何が壊れるのか
- VXLAN のフラッディングはなぜ問題になり、EVPN はそれをどう解決するのか
- SLAAC でアドレスが付くとき、ワイヤ上では実際に何が流れているのか

こうした問いに、一次情報源(RFC・IEEE 規格)を引きながら答えていく構成です。

---

## ⚠️ 重要な注意 — このドキュメントは AI によって生成されています

**本教科書の本文は、大規模言語モデル(生成AI)を用いて執筆されています。**
そのため、内容の正確性は保証されません。以下の点に十分ご注意ください。

- **技術的な誤り・古い情報・不正確な記述が含まれている可能性があります。**
  もっともらしく書かれていても事実と異なる場合があります(いわゆるハルシネーション)。
- **RFC 番号・仕様の細部・数値(タイマー値、統計値など)は、必ず一次情報源
  (該当 RFC / IEEE 規格 / 公式ドキュメント)で裏取りしてください。**
  本文中でも、断定を避け「要検証」としている箇所があります。
- **業務・設計・試験対策など、正確性が求められる用途で本書の記述を
  そのまま根拠にしないでください。** あくまで学習の入り口・理解の足がかりとして、
  自分で検証しながら読む前提のものです。
- 誤りに気づいた場合の修正や、内容の網羅性・最新性は保証されません。

要するに、**「AI が書いた学習ノート」であり、査読された技術書ではありません。**
信頼できる情報源(RFC、書籍、ベンダー公式ドキュメント)と併用してください。

---

## 対象読者

- 「本当のネットワーク初学者」は卒業している(VLAN とは何か、ルーティングとは何か、
  といった言葉の意味は分かる)が、体系立てて理論から学んだわけではない人
- 実務や趣味(ホームラボ含む)で断片的な知識・勘は身についているが、
  「なぜそう動くのか」を RFC や仕様書レベルで説明できるほどではない人
- エンタープライズ〜ISP/データセンター規模の設計・運用を目指している人

## 扱わないこと

- 「ネットワークとは何か」レベルのゼロからの入門
- 特定ベンダー機器の設定手順の網羅・比較(設定例は理解を助ける最小限のみ)
- 資格試験(CCNA/CCNP 等)の出題範囲への準拠

---

## 構成

全5部・26章構成です。各分野は「そもそも何のための技術か」という基礎から立ち上げ、
理論(RFCベース)→ プロトコル動作の詳細 → トラブルシューティングの順に深めていきます。

まずは [`00_overview/01_roadmap.md`](00_overview/01_roadmap.md) で全体像と
読み進め方(標準ルート / 分野別 / 辞書的)を確認することをおすすめします。

### 第1部 ネットワーク基礎・ルーティング基礎 — [`01_fundamentals/`](01_fundamentals/)

スイッチング/ルーティングの原理を体系的に総復習します。

| 章 | 内容 |
|---|---|
| [`01_l2_l3_recap.md`](01_fundamentals/01_l2_l3_recap.md) | L2/L3 の役割分担、カプセル化の総復習 |
| [`02_routing_table_basics.md`](01_fundamentals/02_routing_table_basics.md) | ルーティングテーブル、経路選択の原理 |
| [`03_static_vs_dynamic.md`](01_fundamentals/03_static_vs_dynamic.md) | 静的ルーティング vs 動的ルーティング |
| [`04_distance_vector_link_state.md`](01_fundamentals/04_distance_vector_link_state.md) | ディスタンスベクタ vs リンクステート |
| [`05_igp_overview.md`](01_fundamentals/05_igp_overview.md) | IGP(OSPF等)の位置づけ、BGP への橋渡し |

### 第2部 VLAN / VXLAN / EVPN — [`02_vlan_vxlan_evpn/`](02_vlan_vxlan_evpn/)

L2 セグメンテーションからオーバーレイネットワークまで。

| 章 | 内容 |
|---|---|
| [`01_vlan_basics.md`](02_vlan_vxlan_evpn/01_vlan_basics.md) | VLAN とは何か、802.1Q タグ |
| [`02_trunking_native_vlan.md`](02_vlan_vxlan_evpn/02_trunking_native_vlan.md) | トランキング、ネイティブ VLAN、QinQ |
| [`03_vxlan_fundamentals.md`](02_vlan_vxlan_evpn/03_vxlan_fundamentals.md) | VXLAN の基礎(MAC-in-UDP、フラッド&ラーン) |
| [`04_vxlan_control_plane.md`](02_vlan_vxlan_evpn/04_vxlan_control_plane.md) | マルチキャスト vs EVPN コントロールプレーン |
| [`05_evpn_vxlan.md`](02_vlan_vxlan_evpn/05_evpn_vxlan.md) | EVPN/VXLAN の統合的理解(ルートタイプ、IRB) |

### 第3部 BGP — [`03_bgp/`](03_bgp/)

eBGP/iBGP 基礎からポリシー制御、MP-BGP、大規模設計まで。

| 章 | 内容 |
|---|---|
| [`01_bgp_basics.md`](03_bgp/01_bgp_basics.md) | BGP とは何か、IGP との違い、FSM |
| [`02_ibgp_ebgp.md`](03_bgp/02_ibgp_ebgp.md) | iBGP/eBGP の違い、フルメッシュ |
| [`03_path_attributes.md`](03_bgp/03_path_attributes.md) | パスアトリビュート、経路選択プロセス |
| [`04_policy_control.md`](03_bgp/04_policy_control.md) | route-map, prefix-list, community |
| [`05_mp_bgp.md`](03_bgp/05_mp_bgp.md) | MP-BGP(AFI/SAFI) |
| [`06_large_scale_design.md`](03_bgp/06_large_scale_design.md) | ルートリフレクタ、コンフェデレーション |

### 第4部 IPv6 — [`04_ipv6/`](04_ipv6/)

アドレッシングから NDP、移行技術まで。

| 章 | 内容 |
|---|---|
| [`01_why_ipv6.md`](04_ipv6/01_why_ipv6.md) | IPv4 枯渇問題からの導入 |
| [`02_addressing.md`](04_ipv6/02_addressing.md) | IPv6 アドレッシング |
| [`03_ndp_slaac.md`](04_ipv6/03_ndp_slaac.md) | NDP, SLAAC |
| [`04_routing_ipv6.md`](04_ipv6/04_routing_ipv6.md) | IPv6 ルーティング |
| [`05_transition_technologies.md`](04_ipv6/05_transition_technologies.md) | MAP-E, DS-Lite, NAT64 |

### 第5部 MPLS / SRv6 — [`05_mpls_srv6/`](05_mpls_srv6/)

ラベルスイッチングからセグメントルーティングまで。

| 章 | 内容 |
|---|---|
| [`01_mpls_basics.md`](05_mpls_srv6/01_mpls_basics.md) | なぜラベルスイッチングが必要か |
| [`02_ldp_rsvp_te.md`](05_mpls_srv6/02_ldp_rsvp_te.md) | LDP, RSVP-TE |
| [`03_l3vpn_l2vpn.md`](05_mpls_srv6/03_l3vpn_l2vpn.md) | L3VPN, L2VPN |
| [`04_srv6.md`](05_mpls_srv6/04_srv6.md) | セグメントルーティング(SRv6) |

### 補助ファイル

- [`shared/glossary.md`](shared/glossary.md) — 全章共通の用語集(初出時に追記)
- [`shared/style_guide.md`](shared/style_guide.md) — 執筆スタイルガイド

---

## 今後の拡張予定

第1部〜第5部(全26章)の完成後、以下の分野を追加の部として執筆予定です(現時点では未着手)。

| 部 | 内容 |
|---|---|
| 第6部 | FHRP(VRRP/HSRP)+ STP/RSTP/MSTM — L2/L3 冗長化の基礎 |
| 第7部 | QoS — マーキング(DSCP/CoS)、キューイング、シェーピング/ポリシング |
| 第8部 | セキュリティ基礎 — ACL、ゾーンベースファイアウォール、IPsec、Zero Trust |
| 第9部 | ネットワーク自動化 — NETCONF/YANG |

---

## 読み進め方

各分野は独立して読めるように書かれていますが、いくつかクロス依存があります
(例:EVPN は MP-BGP に依存するため BGP の後に読むのが自然、SRv6 は IPv6 に依存)。
推奨される読了ルートは [`00_overview/01_roadmap.md`](00_overview/01_roadmap.md) に
まとめてあります。

各章はおおむね次の流れで構成されています。

1. **概要 / 導入** — そもそも何のための技術か
2. **理論** — RFC・仕様書ベースの動作原理(なぜこう設計されているか)
3. **プロトコル動作の詳細** — パケットフォーマット、ステートマシン、タイマーなど
4. **(補助)設定例** — 理解を助ける場合のみ、最小限で
5. **トラブルシューティング** — tcpdump/Wireshark での見え方など
6. **演習・まとめ**

---

## ライセンス / 位置づけ

これは個人の学習用に作成した教科書です。学習の参考としてご自由にご覧いただけますが、
冒頭の注意のとおり **内容の正確性は保証されません。** 引用・再利用の際は、
必ずご自身で一次情報源にあたって検証してください。
