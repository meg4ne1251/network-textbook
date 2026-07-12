# 検証TODO一覧(要検証項目の棚卸し)

本書はAI生成で未査読のため、`progress.md` の各ステップに残した「未解決・要検証事項」を
一次情報源(RFC / IEEE 規格 / 公式ドキュメント)で裏取りするための作業リストである。
全34ステップ分の記録を抽出し、**検証の費用対効果**で4層に分類した。

- **Tier A(最優先)** — RFC/仕様の条文・数値で真偽が確定でき、誤っていれば「事実誤り」になる記述
- **Tier B(要再確認・時価)** — 統計値・時系列など、時間とともに変わる数値。現時点値で更新
- **Tier C(実装依存・免責済み)** — FRR/Linux の出力例やバージョン差。本文で既にhedge済みで、原則は個別修正不要
- **Tier D(解消済み)** — 執筆当時の保留だが、後続ステップで実質的に解消。確認のみ

進め方の推奨:**Tier A を上から順に潰す → Tier B を一括更新 → Tier C は免責注記の一貫性だけ確認**。
1項目=1確認を基本とし、確認後はこの表の「状態」を更新し、修正した章は `progress.md` にも追記する。

---

## Tier A — RFC/仕様で裏取りすべき事実(最優先)

| # | 章ファイル | 検証内容 | 対象一次情報源 | 状態 |
|---|---|---|---|---|
| A1 | `02_vlan_vxlan_evpn/01_vlan_basics.md` | CFI → DEI 再定義の正確な版(本文は「802.1ad(2005)でS-TAGに導入 → 802.1Q-2011でC-TAGのCFIをDEIに再定義」と記載) | IEEE 802.1Q-2011 / 802.1ad-2005 | 済(修正) — 802.1ad-2005 が S-TAG に DEI 導入、802.1Q-2011 が 802.1ad 統合と同時に C-TAG のビットを DEI に再定義。本文を版特定の記述に更新 |
| A2 | `03_bgp/01_bgp_basics.md` | ホールドタイム推奨値(Hold 90s / Keepalive 30s / ConnectRetry 120s)と、本文併記の「実装既定180秒/60秒」の整合 | RFC 4271 §10, §4.2 | 済(確認・注記) — §10 の推奨値 ConnectRetry 120 / Hold 90 / Keepalive 30 秒、§4.4 の「1/3 間隔・1 秒未満不可」、§4.2 の「双方の min を採用」すべて本文と一致。実装既定 180/60 は Cisco/FRR 慣例(本文でその旨明記済)。本文にセクション番号を補記 |
| A3 | `03_bgp/03_path_attributes.md` | 「MED 未設定は最小値0(最優先)として扱う」の条文文言(RFC 1771時代は未規定だった経緯も) | RFC 4271 §9.1.2.2 | 済(修正) — §9.1.2.2 は「MED なしは最小値0を割り当てる=最優先」で本文の断定は正しい。ただし RFC 4451 が「初期仕様は逆に最悪値 2³²−1 を割り当てた」経緯を明文化。本文にセクション番号と RFC 4451 の経緯を追記 |
| A4 | `03_bgp/05_mp_bgp.md` | 「BGP-4でIPv4固有は3箇所(NEXT_HOP・AGGREGATOR・NLRI)」の引用の正確性(AGGREGATORは拡張不要とした整理も) | RFC 4760 §1 | 済(確認・微修正) — §1原文「The only three pieces … are (a) NEXT_HOP (b) AGGREGATOR (c) NLRI」と本文引用は一致。AGGREGATOR非拡張の整理も妥当。系譜に中間版RFC 2858(2000)を補い、節番号明記 |
| A5 | `03_bgp/06_large_scale_design.md` | RFC 4456の経路選択修正の挿入位置(本文「BGP Identifier比較の直後」 vs 条文 Step f)/g)) | RFC 4456 §9 | 済(確認・注記) — §9原文「in Step f) the ORIGINATOR_ID SHOULD be treated as the BGP Identifier」「inserted between Steps f) and g): prefer shorter CLUSTER_LIST」で本文の位置づけは正確。本文にStep f)/g)の対応を明記 |
| A6 | `04_ipv6/05_transition_technologies.md` | (a) DS-Lite「アクセス網MTU ≥1540 を SHOULD」 (b) MAP-Eポート計算例(PSID=52→1232–1235, a=6) | RFC 6333 / RFC 7597 App.B | 済(確認・微修正) — (a) RFC 6333 §5.3は「B4-AFTR間リンクを少なくとも40オクテット増やす」SHOULD。"1540"は明記せず1500基準の帰結値なので本文をその旨に精密化。(b) a=6,k=8,m=2で Port=A×1024+208+j → A=1:1232-1235, A=63:64720-64723, 252ポート、すべて検算一致で正しい |
| A7 | `05_mpls_srv6/01_mpls_basics.md` | ICMPトンネリング(エラーをLSP先まで運んで返す)の規定 | RFC 3032 §2.3.2 | 済(確認・注記) — §2.3.2原文「copy the label stack from the original packet to the ICMP message, and then label switch the ICMP message」「proceed in the direction of the original packet's destination…end up, unlabeled, in a router which does know how to route to the source」で本文と一致。節番号を明記し表現を原文寄りに |
| A8 | `05_mpls_srv6/02_ldp_rsvp_te.md` | (a) LDPセッション能動側=transport address大小比較 (b) RSVPリフレッシュ R=30s | RFC 5036 §2.5.2 / RFC 2205 | 済(確認・注記) — (a) §2.5.2原文「If A1 > A2, LSR1 plays the active role」= 大きいほうが能動、ポート646も一致。(b) RFC 2205 §3.7 TIME_VALUES の R 既定30秒で一致(実送出は[0.5R,1.5R]ジッタ)。両方に節番号・原文を補記 |
| A9 | `05_mpls_srv6/03_l3vpn_l2vpn.md` | RT Constraint の SAFI 値 132 | RFC 4684 | 済(確認・修正不要) — IANA SAFI レジストリで値132 = "Route Target constrains"(RFC 4684)を確認。本文「AFI 1 / SAFI 132」は正しく RFC も明記済み |
| A10 | `05_mpls_srv6/04_srv6.md` | (a) 圧縮SID(C-SID/uSID)のRFC番号=9800 と包含関係 (b) SRv6 L3 Service TLVでSIDを配る記述 | RFC 9800 / RFC 9252 | 未 |
| A11 | `06_redundancy/03_fhrp_vrrp.md` | (a) RFC 9568のMaster→Active改称に伴うタイマー名(Master_Down_Interval等の新名称) (b) VRRPv3のIPv4でも擬似ヘッダ込みチェックサム (c) IPv6 RA送信の主体 | RFC 9568 | 未 |
| A12 | `07_qos/04_shaping_policing.md` | trTCMのgreenが「Tp と Tc の両方から引く」動作と変数名(Tp/Tc)対応 | RFC 2698 | 未 |
| A13 | `01_fundamentals/04_distance_vector_link_state.md` | マイクロループ対策(uloop delay)の標準化 — 網羅性の観点で追記要否を判断 | RFC 8333 | 未(要否判断) |

---

## Tier B — 時価・時系列(現時点値で再確認・更新)

| # | 章ファイル | 検証内容 | 参照先 | 状態 |
|---|---|---|---|---|
| B1 | `01_fundamentals/02_routing_table_basics.md` | 「2026年時点でIPv4フルルート約100万経路」の概数 | CIDR Report / 各RIRの統計 | 未 |
| B2 | `04_ipv6/01_why_ipv6.md` | (a) RIR枯渇日付(APNIC 2011-04, RIPE NCC 2019-11ほか) (b) IPv6到達可能率「半分近く」 (c) 移転市場「1アドレス数十ドル」 | Google IPv6統計 / NRO・各RIR | 未 |
| B3 | `04_ipv6/02_addressing.md` | ドキュメント用 3fff::/20(RFC 9637, 2024追加)の記載 | RFC 9637 | 未 |
| B4 | `04_ipv6/03_ndp_slaac.md` | (a) 一時アドレス寿命 TEMP_PREFERRED_LIFETIME=1日 (b) NUD既定値(BaseReachableTime 30s, DELAY 5s, ユニキャストNS 3回) | RFC 8981 / RFC 4861 | 未 |

---

## Tier C — 実装/バージョン依存の出力例(免責済み・原則修正不要)

これらは「FRR/Linux の show 出力の列・文言」「tcpdump フィルタ挙動」など、
バージョンで変わりうる箇所。本文で既に「代表的な形」「バージョンにより微差」と
hedgeしてあり、**動作原理の説明には影響しない**。個別の裏取りより、
**免責注記が全章で一貫しているかの通し確認**で足りる。

対象章(出力例・環境依存の記述):
`01_fundamentals/05_igp_overview.md`(show ip ospf neighbor)、
`02_vlan_vxlan_evpn/02`（tcpdump vlan）、`03`（tcpdump vxlan/`-T vxlan`）、
`04`（show bgp l2vpn evpn / autort）、`05`（同左, extern_learn FDB）、
`03_bgp/02`（inaccessible表示）、`03`（bestpath理由表示）、`04`（Community行）、
`05`（bgp default ipv4-unicast既定）、
`04_ipv6/03`（tcpdump RA / ip -6 neigh、MLDスヌーピングのall-nodes扱い）、
`04`（OSPFv3設定体系, ip -6 route proto ospf6）、`05`（dig dns64, ip6tnl）、
`05_mpls_srv6/01`（ip -f mpls, mpls_router）、`02`（ldpd, mpls ldp-sync）、
`03`（label vpn export auto）、`04`（seg6 mode encap / End.DT4 vrftable）、
`06_redundancy/01`（bridge link show cost）、`02`（mstpctl portmcheck）、
`04`（/proc/net/bonding, bonding既定値）、
`07_qos/03`（fq_codel統計出力）、`04`（tc police構文, tcp_ecn既定値）。

個別に確認しておくと安心な数値(Cへ入れたがA寄り):
- `06_redundancy/01`: Message Age 1秒/ホップ加算、TC保持 Max Age + Forward Delay = 35秒(802.1D条文)
- `06_redundancy/02`: TC While タイマー「HelloTime + 1秒」系(802.1Q条文)
- `06_redundancy/04`: LACPDU固定長110オクテット、WAITING約2秒(802.1AX条文)
- `03_bgp/04`: 空/未定義の prefix-list・community-list 参照時のFRR挙動

---

## Tier D — 後続ステップで解消済み(確認のみ・原則アクション不要)

| 元の保留(Step) | 解消先 |
|---|---|
| 隣接/ピアの用語登録(Step 4) | Step 6 で正式登録 |
| OSPF状態名の先出し(Step 5) | Step 6 で正式導入 |
| トランクポート用語登録(Step 7) | Step 8 で登録 |
| リーフ・スパイン図解(Step 9, 10) | Step 11 で図解追加 |
| RFC 7938 eBGPアンダーレイ(Step 10, 11) | Step 17 で本編化 |
| 各章の「(後述: …)」リンク(Step 12–15ほか) | 各対象章の執筆時にリンク化済み |
| STPの扱い(Step 8) | 第6部の追加で専用章化 |
| VXLANゲートウェイ/IRB(Step 9) | Step 11(IRB)で対応 |

※「解消済み」とした項目も、リンク切れ・用語ゆれが残っていないかは
パッケージング段階の通し確認でまとめてチェックするとよい。

---

## 補足:検証時のルール

- 数値・条文は必ず該当RFC/IEEE規格の原文で確認し、確認した版・セクション番号を本文か脚注に残す
- 確認の結果「概括で問題なし」と判断した場合も、この表の状態を「確認済(修正不要)」に更新する
- 1項目確認・修正するごとに、該当章と本ファイル・`progress.md` を1コミットにまとめてpushする
