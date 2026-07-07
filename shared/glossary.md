# 用語集(Glossary)

この教科書全体で使用する用語の定義集です。
各章で用語が初出したときにここへ追記し、以後の章ではこの定義に統一します。

## 運用ルール

- 追記は**五十音順・アルファベット順のセクション内**に行う(セクションごと分類)
- 各エントリのフォーマット:

```markdown
### 用語名(英語表記 / 略語の場合はフルスペル)

- **定義**: 1〜3行の簡潔な定義
- **初出章**: `XX_ディレクトリ/YY_ファイル名.md`
- **関連RFC**: あれば記載(例: RFC 4271)
- **関連用語**: あれば記載
```

- 同じ概念に複数の呼び名がある場合(例: 「経路」と「ルート」)は、
  **どちらを本文の標準表記とするか**をここで決めて明記する

---

## 数字・記号

### 464XLAT

- **定義**: 端末内のステートレス変換器 CLAT(RFC 7915 による 4→6 変換)と網側のステートフル NAT64(PLAT)を組み合わせ、IPv6-only の網の上でアプリに通常の IPv4 環境を擬態して見せる方式。IPv4 リテラルや独自リゾルバなど DNS64 の射影が働かないケースを端末側で救う。モバイル網で世界的に採用され、スマートフォン OS は CLAT を標準実装する。CLAT の内部用 IPv4 アドレスには 192.0.0.0/29(RFC 7335)を使う。
- **初出章**: `04_ipv6/05_transition_technologies.md`
- **関連RFC**: RFC 6877、RFC 7915、RFC 7335
- **関連用語**: NAT64、DNS64、デュアルスタック

### 802.1ad(IEEE 802.1ad / QinQ、プロバイダブリッジング)

- **定義**: 802.1Q タグの外側にもう1段のタグを積む方式(2005 年の追補として標準化、現在は 802.1Q 本体に統合)。事業者網の入口で顧客識別用の S-TAG(TPID 0x88A8)を押し込み、顧客の C-TAG(TPID 0x8100)には触れない。識別空間は 4094×4094 に広がるが、転送は透過的ブリッジングのままであり、中継網が全顧客 MAC を学習する規模の壁は残る。
- **初出章**: `02_vlan_vxlan_evpn/02_trunking_native_vlan.md`
- **関連用語**: 802.1Q、トランクポート、VLAN

### 802.1Q(IEEE 802.1Q)

- **定義**: VLAN を含む L2 ブリッジング全体を定める IEEE 標準(現行版は IEEE 802.1Q-2022。IEEE 802.1D は 2014 年改訂で統合された)。フレームの所属 VLAN を示す 4 オクテットのタグ(TPID 0x8100 + TCI: PCP 3ビット / DEI 1ビット / VID 12ビット)を定義する。タグは送信元 MAC アドレスの直後に挿入され、EtherType の次ヘッダ連鎖を流用した後方互換な拡張になっている。
- **初出章**: `02_vlan_vxlan_evpn/01_vlan_basics.md`
- **関連用語**: VLAN、EtherType、アクセスポート、トランクポート、PVID

## A〜E

### A+P(Address plus Port)

- **定義**: IPv4 アドレス共有において、加入者に「アドレス+使ってよいポート範囲」を事前に静的に割り当てる考え方。NAPT の状態を網の中央(CGN)ではなく加入者宅の CPE に分散させ、中央装置を無状態化する。ポートから加入者が計算で一意に決まるため、フロー単位のログも不要になる。MAP-E / MAP-T / lw4o6 の理論的基盤。
- **初出章**: `04_ipv6/05_transition_technologies.md`
- **関連RFC**: RFC 6346
- **関連用語**: MAP-E、CGN、NAT

### AFI / SAFI(Address Family Identifier / Subsequent Address Family Identifier)

- **定義**: MP-BGP が運ぶ情報の型を宣言する番号の組(アドレスファミリ)。AFI はアドレスがどのネットワーク層プロトコルのものか(1 = IPv4、2 = IPv6、25 = L2VPN)、SAFI はその情報の用途・付加構造(1 = ユニキャスト、128 = MPLS ラベル付き VPN、70 = EVPN)を示し、(AFI, SAFI) の組が NLRI フィールドの解釈を決める。値は IANA のレジストリで管理される。交換するファミリはケイパビリティ広告で合意する。
- **初出章**: `03_bgp/05_mp_bgp.md`(言及は `02_vlan_vxlan_evpn/04_vxlan_control_plane.md` が先行)
- **関連RFC**: RFC 4760
- **関連用語**: MP-BGP、NLRI、MP_REACH_NLRI / MP_UNREACH_NLRI、ケイパビリティ広告

### AS(Autonomous System / 自律システム)

- **定義**: 単一かつ明確に定義されたルーティングポリシーの下で運用される、1つ以上のプレフィックスのまとまり。境界は物理ではなく意思決定(ポリシー)の境界。各 AS は AS 番号(ASN、RFC 6793 で32ビット化)で識別される。
- **初出章**: `01_fundamentals/05_igp_overview.md`
- **関連RFC**: RFC 1930(BCP 6)、RFC 6793
- **関連用語**: IGP、EGP、パスベクタ

### AS_PATH

- **定義**: BGP の経路広告に含まれる、その経路が通ってきた AS 番号の並び(必須パスアトリビュートの一つ)。受信側は自 AS 番号が含まれる広告を破棄することでループを検出する(パスベクタの本質)。同時に、ポリシー判断と経路選択(パス長の比較)の材料でもある。
- **初出章**: `03_bgp/01_bgp_basics.md`(詳細は `03_bgp/03_path_attributes.md`)
- **関連RFC**: RFC 4271
- **関連用語**: パスベクタ、BGP、AS、パスアトリビュート、AS_PATH プリペンド

### AS_PATH プリペンド(AS path prepending)

- **定義**: 自 AS 番号を AS_PATH に複数回積んで広告し、経路をわざと長く見せることで、他 AS からの流入トラフィックを別の経路へ誘導しようとする手法。遠くの AS にも伝わる唯一の入口制御の細工だが、経路選択プロセスでは AS_PATH 長より先に LOCAL_PREF が比較されるため、効果は相手のポリシー次第の「弱い希望」にとどまる。
- **初出章**: `03_bgp/03_path_attributes.md`
- **関連用語**: AS_PATH、経路選択プロセス、LOCAL_PREF

### Adj-RIB-In / Loc-RIB / Adj-RIB-Out

- **定義**: BGP スピーカーが経路を保持する概念上の3段のテーブル(RFC 4271 Section 3.2)。Adj-RIB-In はピアごとの「受信したままの経路」、Loc-RIB は入力ポリシーと経路選択を経て自分が使うと決めた経路、Adj-RIB-Out はピアごとの「広告すると決めた経路」。ピアとの受け渡しの前後に必ずポリシーの制御点が挟まる点が IGP との本質的な違い。
- **初出章**: `03_bgp/01_bgp_basics.md`
- **関連RFC**: RFC 4271
- **関連用語**: RIB、BGP

### BFD(Bidirectional Forwarding Detection)

- **定義**: 障害検知だけを専門に行う軽量プロトコル。小さなパケットを高頻度(数十ミリ秒間隔)で交換し、途絶を検知すると登録元のプロトコル(OSPF、BGP、静的経路)へ即座に通知する。検知を各プロトコルのタイマーから分離して高速化・共通化する。
- **初出章**: `01_fundamentals/03_static_vs_dynamic.md`
- **関連RFC**: RFC 5880
- **関連用語**: 収束、フローティングスタティックルート

### BGP(Border Gateway Protocol)

- **定義**: AS 間の経路制御を担うパスベクタ型の EGP(現行仕様は BGP-4)。TCP ポート 179 上で動作し、ピアは自動発見せず明示設定で確立する。セッション確立後は差分のみを更新し(定期再広告なし)、経路選択はメトリック最小化ではなく複数のパスアトリビュートの多段比較とポリシーによって行う。AS 内部で経路を運ぶ iBGP と AS 間の eBGP の2つの使い方がある。
- **初出章**: `03_bgp/01_bgp_basics.md`(言及は第1部・第2部にもあり)
- **関連RFC**: RFC 4271
- **関連用語**: EGP、AS、パスベクタ、ピア、AS_PATH、MP-BGP、eBGP / iBGP

### BUM トラフィック(Broadcast, Unknown unicast, Multicast traffic)

- **定義**: L2 でフラッディングによる「全員への配送」が必要になる3種のトラフィック(ブロードキャスト、未学習ユニキャスト、マルチキャスト)の総称。VXLAN などのオーバーレイでは「同じセグメントに参加する全 VTEP へ複製して届ける」仕組み(アンダーレイの IP マルチキャスト、またはヘッドエンドレプリケーション)が別途必要になり、コントロールプレーン設計の主要な論点になる。
- **初出章**: `02_vlan_vxlan_evpn/03_vxlan_fundamentals.md`
- **関連用語**: フラッディング、VXLAN、VTEP、フラッド&ラーン

### CGN(Carrier-Grade NAT / キャリアグレード NAT)

- **定義**: ISP が加入者宅の NAT のさらに外側・自網内に置く大規模な NAPT(NAT444 構成)。宅内との間には専用の共有アドレス空間 100.64.0.0/10(RFC 6598)を使う。グローバルアドレス1個のポート約6万を多数の加入者で分け合うため、加入者あたりポート数の逼迫・状態と障害の集中・発信元特定にポートと時刻のログを要するトレーサビリティ問題を抱える。IPv4 延命策の限界を示す装置でもある。
- **初出章**: `04_ipv6/01_why_ipv6.md`
- **関連RFC**: RFC 6888(BCP)、RFC 6598
- **関連用語**: NAT、プライベートアドレス、IPv6、DS-Lite、A+P

### CIDR(Classless Inter-Domain Routing)

- **定義**: クラスに依存せず任意長のプレフィックスで経路を表現・集約する方式。ルーティングテーブルの規模を抑える基盤。
- **初出章**: `01_fundamentals/02_routing_table_basics.md`
- **関連RFC**: RFC 4632
- **関連用語**: 経路集約、ロンゲストマッチ

### CSPF(Constrained Shortest Path First / 制約付き最短経路計算)

- **定義**: トラフィックエンジニアリングで、入口(ヘッドエンド)ルータが TE 拡張付きの LSDB から「要求帯域に足りないリンク」「アフィニティ(色)の条件に合わないリンク」を除外した上で SPF を回し、制約を満たす経路をホップ列として算出する計算。答えは網の合意ではなく入口ローカルの意思であり、結果は RSVP-TE の ERO(またはセグメントルーティングのセグメントリスト)として使われる。
- **初出章**: `05_mpls_srv6/02_ldp_rsvp_te.md`
- **関連RFC**: RFC 3630(OSPF-TE)、RFC 5305(IS-IS TE)
- **関連用語**: SPF、RSVP-TE、トラフィックエンジニアリング、LSDB

### DAD(Duplicate Address Detection / 重複アドレス検出)

- **定義**: IPv6 のすべてのユニキャストアドレスが使用開始前に通る重複検査(RFC 4862)。tentative 状態のアドレスを Target に、送信元 :: の NS をそのアドレスの要請ノードマルチキャストへ送り、応答の NA や同じ Target への他ノードの NS がなければ合格とする。リンクローカルの DAD 失敗はそのインタフェースの IPv6 を事実上停止させる。合格前に使い始める楽観的 DAD(RFC 4429)、ヘアピン誤検出対策の Enhanced DAD(RFC 7527)がある。
- **初出章**: `04_ipv6/03_ndp_slaac.md`(言及は `04_ipv6/02_addressing.md` が先行)
- **関連RFC**: RFC 4862、RFC 4429、RFC 7527
- **関連用語**: NDP、NS / NA、要請ノードマルチキャスト、SLAAC

### DHCPv6(Dynamic Host Configuration Protocol for IPv6)

- **定義**: IPv6 のステートフルなアドレス・構成情報配布プロトコル(RFC 8415)。RA の M フラグ(アドレスも DHCPv6 で)/ O フラグ(付加情報のみ DHCPv6 で)によりホストへ利用が促される。IPv4 の DHCP と異なりデフォルトゲートウェイを配るオプションが存在せず、デフォルト経路は常に RA が供給する。アドレス払い出しの台帳管理と、ルータへサブネット単位でプレフィックスを貸すプレフィックス委任(DHCPv6-PD)が主用途。
- **初出章**: `04_ipv6/03_ndp_slaac.md`
- **関連RFC**: RFC 8415
- **関連用語**: SLAAC、RS / RA、IPv6

### DNS64

- **定義**: IPv6-only ホストの名前解決で、AAAA レコードが存在しない名前について A レコードを引き、その IPv4 アドレスを NAT64 プレフィックス(既定 64:ff9b::/96)へ埋め込んだ合成 AAAA を返すリゾルバ機能。IPv4 の世界を IPv6 アドレス空間へ射影する NAT64 の共犯者であり、DNS を通らない通信(IPv4 リテラル、独自リゾルバ)は救えない。合成レコードは権威の署名と一致しないため DNSSEC 検証とは相性が悪い。
- **初出章**: `04_ipv6/05_transition_technologies.md`
- **関連RFC**: RFC 6147、RFC 6052
- **関連用語**: NAT64、464XLAT

### DR / BDR(Designated Router / Backup Designated Router、指定ルータ)

- **定義**: OSPF がブロードキャスト型ネットワークごとに選出する代表ルータとその控え。各ルータは DR/BDR とだけ隣接を結び(隣接数を O(n²)→O(n) に削減)、DR は当該セグメントの接続関係を Network-LSA(Type 2)として広告する。
- **初出章**: `01_fundamentals/05_igp_overview.md`
- **関連RFC**: RFC 2328
- **関連用語**: 隣接、LSA、OSPF

### DS-Lite(Dual-Stack Lite)

- **定義**: IPv6 アクセス網の上で IPv4 をサービスとして提供する(IPv4aaS)トンネル方式。宅内の B4 は NAT せず IPv4 パケットを IPv6 で包むだけ(Next Header = 4)、ISP 網内の AFTR がトンネル終端と NAPT44 を一手に担う。AFTR の NAPT テーブルは B4 の IPv6 アドレスを連結キーに含むため、加入者間でプライベートアドレスが重複してよい。状態が中央(AFTR)に集中するステートフル型で、NAT は AFTR の1回だけ(NAT444 を回避)。AFTR のアドレスは DHCPv6 オプション(RFC 6334)で配布される。
- **初出章**: `04_ipv6/05_transition_technologies.md`
- **関連RFC**: RFC 6333、RFC 6334
- **関連用語**: CGN、MAP-E、NAT、デュアルスタック

### ECMP(Equal-Cost Multi-Path)

- **定義**: 同一宛先への等コスト経路を複数同時にテーブルへ載せ、フロー単位(5タプルのハッシュ)でトラフィックを分散する仕組み。
- **初出章**: `01_fundamentals/02_routing_table_basics.md`
- **関連RFC**: RFC 2992
- **関連用語**: メトリック

### EGP(Exterior Gateway Protocol)

- **定義**: AS 間の経路制御を担うルーティングプロトコルの総称。最短経路ではなくポリシー(組織間の合意)に基づいて経路を選択・広告する。現在のインターネットで使われる EGP は事実上 BGP のみ。
- **初出章**: `01_fundamentals/05_igp_overview.md`
- **関連RFC**: RFC 4271(BGP-4)
- **関連用語**: IGP、AS、パスベクタ

### eBGP / iBGP(external / internal BGP)

- **定義**: BGP の2つの使い方。ピアの AS 番号が自分と異なれば eBGP(AS 間)、同じなら iBGP(AS 内部の経路運搬)であり、プロトコル自体は同一で広告規則だけが切り替わる。eBGP は広告時に AS_PATH へ自 AS を前置し NEXT_HOP を自分に書き換える(TTL 1 の直結慣例)。iBGP はどちらも変更せず、代わりに「iBGP から学んだ経路は iBGP へ再広告しない」規則(RFC 4271 Section 9.2)でループを防ぐため、フルメッシュが原則になる。
- **初出章**: `03_bgp/02_ibgp_ebgp.md`(言及は `03_bgp/01_bgp_basics.md` にもあり)
- **関連RFC**: RFC 4271
- **関連用語**: BGP、AS_PATH、フルメッシュ、ネクストホップセルフ、ピア

### ARP(Address Resolution Protocol)

- **定義**: IPv4アドレスから同一セグメント上のMACアドレスを解決するプロトコル。IPv6ではNDPが同じ役割を担う。
- **初出章**: `01_fundamentals/01_l2_l3_recap.md`
- **関連RFC**: RFC 826
- **関連用語**: NDP、デフォルトゲートウェイ

### ARP サプレッション(ARP suppression)

- **定義**: EVPN 環境で、VTEP が事前配布された MAC↔IP の対応(ルートタイプ2で運ばれる)を使い、配下の端末からの ARP 要求へその場で代理応答する機能。ARP ブロードキャストがトンネルに入る前に消えるため、BUM トラフィックの主成分が削減される。
- **初出章**: `02_vlan_vxlan_evpn/04_vxlan_control_plane.md`
- **関連RFC**: RFC 7432、RFC 9161(Proxy-ARP/ND の運用詳細)
- **関連用語**: EVPN、BUM トラフィック、ARP

### ESI(Ethernet Segment Identifier)

- **定義**: 端末やスイッチを複数の VTEP へ同時接続(マルチホーミング)するとき、その接続リンクの束(イーサネットセグメント)を識別する 10 オクテットの値。同じ ESI を RT-4 で広告し合う VTEP どうしが冗長グループを自動発見し、DF(Designated Forwarder)選出・エイリアシング・マスウィズドローの基盤になる。単一接続では 0。
- **初出章**: `02_vlan_vxlan_evpn/05_evpn_vxlan.md`
- **関連RFC**: RFC 7432、RFC 8365
- **関連用語**: EVPN、VTEP、ルートタイプ

### EtherType

- **定義**: Ethernetヘッダ内でペイロードの上位プロトコルを示す2オクテットのフィールド。0x0800=IPv4、0x86DD=IPv6、0x0806=ARP、0x8100=VLANタグ。
- **初出章**: `01_fundamentals/01_l2_l3_recap.md`
- **関連用語**: カプセル化

### EVPN(Ethernet VPN)

- **定義**: MAC アドレスの所在や VTEP のメンバーシップといった L2 の到達性情報を、MP-BGP の経路情報(AFI 25 = L2VPN / SAFI 70 = EVPN)として配布するコントロールプレーン。本体仕様は RFC 7432(MPLS 網向け)で、RFC 8365 が VXLAN 等のオーバーレイへの適用を定める。所在の事前配布により未知ユニキャストのフラッディングをほぼ解消し、ARP サプレッションと RT-3 によるフラッディングリスト自動構築を実現する。
- **初出章**: `02_vlan_vxlan_evpn/04_vxlan_control_plane.md`(詳細は `02_vlan_vxlan_evpn/05_evpn_vxlan.md`)
- **関連RFC**: RFC 7432、RFC 8365
- **関連用語**: MP-BGP、VXLAN、VTEP、ヘッドエンドレプリケーション、ARP サプレッション、ルートタイプ、IRB、MAC-VRF

## F〜J

### Fast Reroute(FRR、高速迂回 / RFC 4090)

- **定義**: 障害の検知後に経路を計算するのではなく、リンク/ノードを迂回するバックアップ経路を**事前に**用意しておき、検知の瞬間に迂回点のルータ(PLR)がローカル判断でトラフィックを切り替える 50 ミリ秒級の保護機構。RSVP-TE では bypass tunnel(RFC 4090)、セグメントルーティングでは TI-LFA が同じ思想を実装する。本書で設定例に用いるソフトウェア FRRouting と略称(FRR)が衝突するため、本文では「FRR(RFC 4090)」等と明記して区別する。
- **初出章**: `05_mpls_srv6/02_ldp_rsvp_te.md`
- **関連RFC**: RFC 4090
- **関連用語**: RSVP-TE、収束、BFD

### FEC(Forwarding Equivalence Class / 転送等価クラス)

- **定義**: 転送の観点から同じ扱いを受けるパケットの集合(RFC 3031)。IP 転送では各ホップが宛先アドレスから毎回分類し直すのに対し、MPLS では網の入口(LER)が一度だけ分類し、結果をラベルとしてパケットに焼き付ける。FEC は宛先プレフィックスに限らず、「BGP ネクストホップが同じ経路の集合」「特定 VPN 宛て」など入口が識別できるものなら何でもよい — この分類の自由度と一回性が BGP フリーコア・VPN・TE の源泉である。
- **初出章**: `05_mpls_srv6/01_mpls_basics.md`
- **関連RFC**: RFC 3031
- **関連用語**: MPLS、LSP、ロンゲストマッチ

### FIB(Forwarding Information Base)

- **定義**: RIB の最良経路を転送用に最適化したテーブル。パケット転送(データプレーン)はこれを引いて行われ、再帰的ルックアップの解決結果が展開される。
- **初出章**: `01_fundamentals/02_routing_table_basics.md`
- **関連用語**: RIB、再帰的ルックアップ

### GTSM(Generalized TTL Security Mechanism)

- **定義**: TTL(IPv6 では Hop Limit)を最大値の 255 で送信し、受信側が「TTL がほぼ 255 のまま届いたか」を検査することで、遠隔からの偽造パケットを排除する保護機構。TTL は転送で減ることはあっても増えることはないため、ネットワークの遠くにいる攻撃者には原理的に偽造できない。直結 eBGP ピアの保護が代表的な用途。
- **初出章**: `03_bgp/02_ibgp_ebgp.md`
- **関連RFC**: RFC 5082
- **関連用語**: TTL、eBGP / iBGP

### GUA(Global Unicast Address / グローバルユニキャストアドレス)

- **定義**: インターネット全体で一意・到達可能な IPv6 ユニキャストアドレス。現在は 2000::/3(全空間の 1/8)から割り当てられる。構造は「グローバルルーティングプレフィックス+サブネット ID+64 ビットのインタフェース ID」の3層で、IANA → RIR → ISP(/32 程度)→ 組織(/48〜/56)→ サブネット(/64)の階層割り当てにより経路集約が構造的に効く。
- **初出章**: `04_ipv6/02_addressing.md`
- **関連RFC**: RFC 4291、RFC 3587、RFC 6177
- **関連用語**: IPv6、ULA、リンクローカルアドレス、インタフェース ID、経路集約

### IVL / SVL(Independent / Shared VLAN Learning、独立/共有 VLAN 学習)

- **定義**: 802.1Q が定める MAC 学習空間の分離方式。IVL は VLAN ごとに独立の学習空間(FID)を割り当て、MAC アドレステーブルのキーが (VLAN, MAC) の組になる。SVL は複数 VLAN で学習空間を共有する。現在のスイッチは原則 IVL で動作し、同一 MAC が VLAN ごとに別ポートで学習されても正常である。
- **初出章**: `02_vlan_vxlan_evpn/01_vlan_basics.md`
- **関連用語**: VLAN、透過的ブリッジング

### IGMP(Internet Group Management Protocol)

- **定義**: 受信希望のホストが「このマルチキャストグループのトラフィックが欲しい」と最寄りのルータへ表明(join / leave)するプロトコル。現行版は IGMPv3。VXLAN のマルチキャストモードでは、各 VTEP が VNI 対応グループへの参加表明に使う。
- **初出章**: `02_vlan_vxlan_evpn/04_vxlan_control_plane.md`(言及は `02_vlan_vxlan_evpn/03_vxlan_fundamentals.md`)
- **関連RFC**: RFC 3376
- **関連用語**: PIM、BUM トラフィック、VTEP

### IGP(Interior Gateway Protocol)

- **定義**: AS 内部の経路制御を担うルーティングプロトコルの総称(OSPF、IS-IS、RIP、EIGRP)。全ルータを信頼できる範囲で、メトリック最小の最短経路と速い収束を目的とする。実務では顧客経路を載せず、ルータ間リンクとループバックだけを運ぶ土台として使うのが定石。
- **初出章**: `01_fundamentals/05_igp_overview.md`
- **関連用語**: EGP、AS、リンクステート、ディスタンスベクタ

### IPv6(Internet Protocol version 6)

- **定義**: 128 ビットアドレスを持つ現行の次世代 IP(現行仕様は RFC 8200、STD 86)。IPv4 とは相互互換のない別プロトコルであり、Ethernet 上では EtherType 0x86DD で運ばれる(移行はデュアルスタックによる並走が基本)。基本ヘッダは固定 40 オクテット・8 フィールドで、ヘッダチェックサムと経路上のフラグメンテーションを廃止(最小リンク MTU 1280、PMTUD は事実上必須)、オプションは拡張ヘッダへ移設された。ブロードキャストはなく、ARP の役割は NDP(マルチキャストベース)が担う。
- **初出章**: `04_ipv6/01_why_ipv6.md`(言及は第1部・第3部にもあり)
- **関連RFC**: RFC 8200(旧 RFC 2460、RFC 1883)
- **関連用語**: 拡張ヘッダ、デュアルスタック、NAT、TTL、MTU、GUA、リンクローカルアドレス、NDP、SLAAC

### IRB(Integrated Routing and Bridging / 統合ルーティング&ブリッジング)

- **定義**: EVPN オーバーレイで、ルーティングとブリッジングを各 VTEP(リーフ)が一体的に行う構成。全リーフが同一ゲートウェイを持つエニーキャストゲートウェイと組み合わせる。テナントの L3VNI を介して入口・出口の双方がルーティングする対称(symmetric)方式が主流で、入口だけがルーティングし宛先 L2VNI へ直接送り込む非対称(asymmetric)方式もある。両方式はファブリック内で混在できない。
- **初出章**: `02_vlan_vxlan_evpn/05_evpn_vxlan.md`
- **関連RFC**: RFC 9135
- **関連用語**: エニーキャストゲートウェイ、L2VNI / L3VNI、EVPN、SVI

### IS-IS(Intermediate System to Intermediate System)

- **定義**: リンクステート型 IGP の一つ。OSPF とアルゴリズムはほぼ同型だが、IP ではなく L2 上で直接動作し、情報を TLV 形式で運ぶため拡張が容易。IPv6 対応も TLV の追加だけで行われた(RFC 5308。OSPF が OSPFv3 を新造したのと対照的)。大規模 ISP・データセンターで広く使われる。
- **初出章**: `01_fundamentals/04_distance_vector_link_state.md`(位置づけは `01_fundamentals/05_igp_overview.md`)
- **関連RFC**: ISO/IEC 10589、RFC 1195、RFC 5308
- **関連用語**: OSPF、リンクステート、LSDB、マルチトポロジ

## K〜O

### L2VNI / L3VNI

- **定義**: EVPN/VXLAN における VNI の役割区分。L2VNI は端末を収容するブロードキャストドメイン(サブネット)に対応する通常の VNI。L3VNI はテナント(IP-VRF)ごとに1つ割り当てるサブネット間ルーティング専用の VNI で、対称 IRB においてオーバーレイ上のルータ間リンクとして機能する。RT-2 は Label1 に L2VNI、Label2 に L3VNI を載せて L2/L3 両方の知識を1本で運ぶ。
- **初出章**: `02_vlan_vxlan_evpn/05_evpn_vxlan.md`
- **関連RFC**: RFC 9135、RFC 8365
- **関連用語**: VNI、IRB

### LDP(Label Distribution Protocol)

- **定義**: IGP の最短経路に沿った LSP を全 FEC について自動的に作る、MPLS の基本のラベル配布プロトコル(RFC 5036)。UDP 646 のマルチキャスト Hello で隣人を発見し、TCP 646 のセッション(transport address = ループバックどうし)でラベルを交換する。経路は一切決めず IGP に従属する。フレームモードの定番は DU + liberal retention + independent 制御(メモリを使って収束を速くする)。IGP との収束のずれが生む穴は LDP-IGP 同期(RFC 5443)で塞ぐ。
- **初出章**: `05_mpls_srv6/02_ldp_rsvp_te.md`(言及は `05_mpls_srv6/01_mpls_basics.md` が先行)
- **関連RFC**: RFC 5036、RFC 5443
- **関連用語**: MPLS、LSP、FEC、LFIB、IGP、RSVP-TE

### LFIB(Label Forwarding Information Base)

- **定義**: LSR が持つラベル転送用のテーブル。「入ラベル → ラベル操作(swap/pop)+送り先」の対応表であり、RFC 3031 の用語では ILM(Incoming Label Map)が対応する(入口 LER 側の「FEC → push」の表は FTN)。IP の FIB と並ぶデータプレーンのテーブルで、中身を書くのはラベル配布のコントロールプレーン(LDP、RSVP-TE、BGP、SR)である。
- **初出章**: `05_mpls_srv6/01_mpls_basics.md`
- **関連RFC**: RFC 3031
- **関連用語**: FIB、MPLS、LSR / LER

### LOCAL_PREF(Local Preference)

- **定義**: 経路を AS としてどれくらい好むかを表す well-known 属性(4 オクテット、大きいほど優先)。iBGP ピアへの広告には必ず付け、eBGP ピアへは付けない(RFC 4271 Section 5.1.5)— AS の外に出ない、出口選択の意思統一の道具である。経路選択プロセスの最初の比較段であり、AS_PATH 長など他のすべての材料に優先する。既定値 100 は実装慣例(RFC は既定値を定めない)。
- **初出章**: `03_bgp/03_path_attributes.md`(言及は `03_bgp/02_ibgp_ebgp.md` にもあり)
- **関連RFC**: RFC 4271
- **関連用語**: パスアトリビュート、経路選択プロセス、MED、eBGP / iBGP

### LSA(Link-State Advertisement)

- **定義**: OSPF において LSDB に格納される情報の1単位。生成ルータ・シーケンス番号・寿命(MaxAge 3600秒、リフレッシュ 1800秒)を持ち、フラッディングの規律(新旧判定・ACK・エージング)は LSA 単位で適用される。主要タイプ: Type 1(Router)/2(Network)はエリア内の一次情報、Type 3(Summary)はエリア間の距離情報、Type 5(AS-External)は再配送された外部経路。
- **初出章**: `01_fundamentals/05_igp_overview.md`(言及のみは `01_fundamentals/04_distance_vector_link_state.md`)
- **関連RFC**: RFC 2328
- **関連用語**: LSDB、フラッディング、エリア、DR / BDR

### LSDB(Link-State Database / リンクステートデータベース)

- **定義**: リンクステート型プロトコルで、各ルータが保持するリンク状態情報の集合。フラッディングにより全ルータで同一に保たれ、「ネットワークの地図」として SPF 計算の入力になる。全ルータの LSDB が一致していることがリンクステート型の大前提。
- **初出章**: `01_fundamentals/04_distance_vector_link_state.md`
- **関連RFC**: RFC 2328(OSPFv2)
- **関連用語**: リンクステート、SPF、フラッディング

### LSP(Label Switched Path / ラベルスイッチドパス)

- **定義**: ある FEC のパケットが MPLS 網の入口から出口までたどる、ラベルの張り替え(swap)の連鎖が作る道。ラベルはリンクごとのローカルな合意なので、LSP は「隣接間の約束の連鎖」として成立する。**一方向**であり、往復は別の LSP になる。
- **初出章**: `05_mpls_srv6/01_mpls_basics.md`
- **関連RFC**: RFC 3031
- **関連用語**: MPLS、FEC、LSR / LER

### LSR / LER(Label Switching Router / Label Edge Router)

- **定義**: MPLS 網の2つの役割。LSR は網内部でラベルの swap だけを行うルータ、LER は網の縁でラベルの push(入口 = ingress)/ pop(出口 = egress)を行うルータ。ISP の文脈では LER を PE(Provider Edge)、内部 LSR を P(Provider)、MPLS を関知しない顧客ルータを CE(Customer Edge)と呼ぶ(RFC 4364 由来の慣用)。
- **初出章**: `05_mpls_srv6/01_mpls_basics.md`
- **関連RFC**: RFC 3031、RFC 4364
- **関連用語**: MPLS、LSP、LFIB

### MAC-VRF

- **定義**: VTEP(仕様の用語では PE)がテナントごとに持つ独立の MAC テーブル(仮想スイッチインスタンス)。EVPN の経路は RT の一致により対応する MAC-VRF へ選択的に取り込まれる。VXLAN の VLAN-based サービスでは 1 MAC-VRF = 1 VNI の対応が基本。
- **初出章**: `02_vlan_vxlan_evpn/05_evpn_vxlan.md`
- **関連RFC**: RFC 7432
- **関連用語**: RD、RT、VNI、EVPN

### MAC モビリティ(MAC mobility)

- **定義**: 端末(VM)が別 VTEP 配下へ移動したとき、EVPN が所在情報を全 VTEP で正しく更新する仕組み。RT-2 に付与する MAC Mobility 拡張コミュニティのシーケンス番号で広告の新旧を順序付け、番号の大きい広告が勝ち、旧 VTEP は自広告を撤回する。番号の急増(推奨既定: 180 秒間に 5 回超)は同一 MAC の二重存在(クローン事故・L2 ループ)の兆候として検出され、広告更新が凍結される(重複 MAC 検出)。
- **初出章**: `02_vlan_vxlan_evpn/05_evpn_vxlan.md`
- **関連RFC**: RFC 7432
- **関連用語**: EVPN、VTEP、ルートタイプ

### MAP-E(Mapping of Address and Port with Encapsulation)

- **定義**: IPv4aaS のトンネル方式のうち、A+P によるステートレス分散型の代表。共有ルール(BMR)の下で、DHCPv6-PD の委任プレフィックス内の EA-bits に IPv4 アドレスの下位ビットと PSID(ポートセット番号)を埋め込み、CE が自分の持ちポート範囲内で NAPT44 してから BR へカプセル化する。BR は完全に無状態で、戻りの配送先を宛先アドレス+ポートから算術的に導出する(エニーキャスト冗長・ログ不要)。代償としてポート割り当ては静的で、共有比で決まった数(既定オフセット a=6 では (2^6−1)×2^m 個)しか使えない。変換版の MAP-T(RFC 7599)、中間型の lw4o6(RFC 7596)が兄弟仕様。
- **初出章**: `04_ipv6/05_transition_technologies.md`
- **関連RFC**: RFC 7597、RFC 7598(DHCPv6 オプション)
- **関連用語**: A+P、DS-Lite、DHCPv6、CGN

### MED(Multi-Exit Discriminator / MULTI_EXIT_DISC)

- **定義**: 複数の地点で接続する隣接 AS に対して「入ってくるならこの入口を使ってほしい」という希望を伝える optional non-transitive 属性(4 オクテット、小さいほど優先)。eBGP で隣接 AS へ渡り、その AS 内では iBGP で運ばれるが、さらに先の AS へは伝搬してはならない(RFC 4271 Section 5.1.4)。既定では同一の隣接 AS から受けた経路どうしでしか比較されない。属性を持たない経路は最小値 0(最優先)として扱うのが RFC の規定だが、解釈を変える実装オプション(missing-as-worst 等)が多数あり、関係者の設定合わせが前提の属性である。
- **初出章**: `03_bgp/03_path_attributes.md`(言及は `03_bgp/02_ibgp_ebgp.md` にもあり)
- **関連RFC**: RFC 4271
- **関連用語**: パスアトリビュート、経路選択プロセス、LOCAL_PREF

### MP-BGP(Multiprotocol BGP)

- **定義**: BGP が IPv4 ユニキャスト経路以外の情報(IPv6 経路、VPN 経路、EVPN の MAC 情報など)を運べるようにする拡張。運ぶ情報の種類をアドレスファミリの番号組(AFI/SAFI)で区別する。実体は2つのパスアトリビュート(MP_REACH_NLRI / MP_UNREACH_NLRI)であり、AS_PATH・コミュニティ・ポリシーの枠組みは全ファミリ共通に働く。EVPN はこの枠組みの1ファミリとして定義されている。
- **初出章**: `02_vlan_vxlan_evpn/04_vxlan_control_plane.md`(詳細は `03_bgp/05_mp_bgp.md`)
- **関連RFC**: RFC 4760
- **関連用語**: EVPN、ピア、パスベクタ、AFI / SAFI、MP_REACH_NLRI / MP_UNREACH_NLRI

### MP_REACH_NLRI / MP_UNREACH_NLRI

- **定義**: MP-BGP の本体である2つのパスアトリビュート(Type 14 / 15、いずれも optional non-transitive)。従来の UPDATE メッセージの NLRI・撤回リストフィールドに置かれていた到達性情報を属性の中へ移設したもので、先頭の AFI/SAFI が中身の型を宣言する。MP_REACH_NLRI はネクストホップも内包し、ファミリに応じた形(IPv6 のグローバル+リンクローカルの2個、EVPN の VTEP アドレス等)を運べる。non-transitive のため、理解しない実装のところで情報は静かに止まり、処理できない者へ中継されない。
- **初出章**: `03_bgp/05_mp_bgp.md`(言及は `03_bgp/03_path_attributes.md` にもあり)
- **関連RFC**: RFC 4760
- **関連用語**: MP-BGP、AFI / SAFI、NLRI、パスアトリビュート

### MPLS(Multi-Protocol Label Switching)

- **定義**: 網の入口でパケットを FEC に分類して固定長のラベル(4 オクテット: Label 20 / TC 3 / S 1 / TTL 8 ビット)を押し、以後はラベルの張り替えだけで転送する方式(アーキテクチャは RFC 3031、エンコーディングは RFC 3032。Ethernet 上は EtherType 0x8847)。転送の根拠を宛先 IP から切り離せるため、BGP フリーコア・VPN・トラフィックエンジニアリングの基盤になる。ラベルはスタックでき(S ビットが底の印)、「出口まで運ぶトンネルラベル+着後の仕分けを担うサービスラベル」の分業が VPN の骨格。ラベル配布のコントロールプレーンはアーキテクチャから分離されている。
- **初出章**: `05_mpls_srv6/01_mpls_basics.md`(言及は第1部・第3部にもあり)
- **関連RFC**: RFC 3031、RFC 3032
- **関連用語**: FEC、LSP、LSR / LER、LFIB、PHP、ラベルスタック、カプセル化

### Null ルート(null route / 廃棄経路)

- **定義**: マッチしたパケットを廃棄する経路。経路集約時に集約プレフィックスの「底」として置き、実在しない宛先のパケットがデフォルトルートへ流れてループすることを防ぐ。
- **初出章**: `01_fundamentals/02_routing_table_basics.md`
- **関連用語**: 経路集約、デフォルトルート

### MTU(Maximum Transmission Unit)

- **定義**: あるリンクで一度に送信できるL3パケットの最大サイズ(オクテット)。標準Ethernetでは1500。
- **初出章**: `01_fundamentals/01_l2_l3_recap.md`
- **関連RFC**: RFC 1191(Path MTU Discovery)
- **関連用語**: フラグメンテーション、PMTUD

### NAT(Network Address Translation / NAPT)

- **定義**: 境界のルータが IP アドレス(実用上はほぼ常にポート番号も = NAPT、RFC 3022)を書き換え、プライベートアドレスの内部ネットワークをグローバルアドレス少数でインターネットへ接続する仕組み。フローごとの変換状態を発信時に作るため「内から外への発信」は透過だが、網の中間に状態を持ち込み(無状態転送の原則の例外)、外からの着信・アドレスをペイロードに埋めるプロトコル・経路の非対称を壊す。IPv4 アドレス枯渇の延命策であり、セキュリティ機構ではない。
- **初出章**: `04_ipv6/01_why_ipv6.md`(言及は第1部にもあり)
- **関連RFC**: RFC 3022、RFC 2663(用語)、RFC 1631(初提案)
- **関連用語**: プライベートアドレス、CGN、IPv6、ホップバイホップ転送、NAT64

### NAT64

- **定義**: IPv6-only ホストと IPv4 サーバの間で、IPv6 ↔ IPv4 のヘッダを RFC 7915 の規則で書き直すステートフル変換器。IPv6 側の宛先は NAT64 プレフィックス(既定は well-known prefix 64:ff9b::/96、RFC 6052。プライベート IPv4 は埋め込み不可)に IPv4 アドレス 32 ビットを埋め込んで表現し、変換器は NAPT44 相当の状態テーブルを持つ。宛先アドレスの供給は DNS64 が担う。明示対応表によるステートレス変換(SIIT、RFC 7915 + RFC 7757)もある。
- **初出章**: `04_ipv6/05_transition_technologies.md`(64:ff9b::/96 の言及は `04_ipv6/02_addressing.md` が先行)
- **関連RFC**: RFC 6146、RFC 6052、RFC 7915
- **関連用語**: DNS64、464XLAT、NAT、IPv6

### NDP(Neighbor Discovery Protocol / 近隣探索プロトコル)

- **定義**: IPv6 がリンク上の隣人との対話 — アドレス解決、ルータ発見、リダイレクト、到達性確認(NUD)、重複検出(DAD)— をすべて担わせる体系(RFC 4861)。実体は ICMPv6 の5メッセージ(RS 133 / RA 134 / NS 135 / NA 136 / Redirect 137)で、全メッセージが Hop Limit 255 検査(リンク内発の証明。GTSM と同原理)と TLV オプション形式を共有し、要請ノードマルチキャストの上で動く。土台はリンクローカルアドレスであり、ICMPv6 を過剰にフィルタすると全機能が止まる。
- **初出章**: `04_ipv6/03_ndp_slaac.md`(言及は第1部・`04_ipv6/01〜02` が先行)
- **関連RFC**: RFC 4861
- **関連用語**: ARP、NS / NA、RS / RA、NUD、DAD、SLAAC、リンクローカルアドレス、GTSM

### NLRI(Network Layer Reachability Information)

- **定義**: BGP の UPDATE メッセージで広告される経路(プレフィックス)の集合。1つの UPDATE に載る NLRI は、すべて同じパスアトリビュート一式を共有する(属性が異なる経路は別の UPDATE で送られる)。MP-BGP では AFI/SAFI ごとに NLRI の形式が拡張定義され、EVPN のルートタイプもその一種。
- **初出章**: `03_bgp/01_bgp_basics.md`
- **関連RFC**: RFC 4271、RFC 4760
- **関連用語**: BGP、MP-BGP、ルートタイプ

### NS / NA(Neighbor Solicitation / Neighbor Advertisement、近隣要請/近隣広告)

- **定義**: NDP のアドレス解決・到達性確認・重複検出を担う対のメッセージ(ICMPv6 Type 135 / 136)。NS は Target アドレスの要請ノードマルチキャスト(NUD の確認時はユニキャスト)へ送られ、NA が MAC アドレス(Target Link-Layer Address オプション)を返す。NA は3フラグを持つ — R(送信者はルータ)、S(要請への応答 = 到達性の証拠。NUD の確認に数えられるのは S=1 のみ)、O(キャッシュの既存エントリを上書きしてよい)。
- **初出章**: `04_ipv6/03_ndp_slaac.md`
- **関連RFC**: RFC 4861
- **関連用語**: NDP、ARP、要請ノードマルチキャスト、NUD、DAD

### NUD(Neighbor Unreachability Detection / 近隣不到達性検出)

- **定義**: 近隣キャッシュ(ARP テーブルに相当)の各エントリを到達性の状態機械(INCOMPLETE / REACHABLE / STALE / DELAY / PROBE)で管理する NDP の機構。S=1 の NA と上位層の前進(TCP の ACK 等)だけを「生きている証拠」と数え、確認用トラフィックを最小化する(STALE は正常な状態であり、送信再開時に DELAY → 必要なら PROBE で確かめ直す)。ARP に欠けていた「教わった対応がまだ有効か」への答え。
- **初出章**: `04_ipv6/03_ndp_slaac.md`
- **関連RFC**: RFC 4861
- **関連用語**: NDP、NS / NA、ARP

### ORIGINATOR_ID / CLUSTER_LIST

- **定義**: ルートリフレクタ環境で AS 内のループを検出する2つのパスアトリビュート(Type 9 / 10、いずれも optional non-transitive)。ORIGINATOR_ID は経路を AS 内に持ち込んだ発信者の BGP Identifier(最初に反射する RR が付与)、CLUSTER_LIST は経路が通ってきたクラスタ ID の列(RR が反射のたびに自 ID を前置)。自分の ID を見つけた者が経路を無視する — AS_PATH と同じパスベクタの原理を AS 内部で再演する仕組みである。経路選択では BGP Identifier の比較に ORIGINATOR_ID を代用し、CLUSTER_LIST の短い経路を優先する段が加わる。
- **初出章**: `03_bgp/06_large_scale_design.md`(言及は `03_bgp/03_path_attributes.md` にもあり)
- **関連RFC**: RFC 4456
- **関連用語**: ルートリフレクタ、AS_PATH、パスベクタ、パスアトリビュート

### ORIGIN

- **定義**: 経路情報が最初に BGP に入ったときの由来を示す well-known mandatory 属性。IGP(0、network 文などによる明示的な注入)、EGP(1、旧 EGP プロトコル由来の歴史的値)、INCOMPLETE(2、再配送が典型)の3値で、経路選択では値の小さいほうが好まれる。
- **初出章**: `03_bgp/03_path_attributes.md`
- **関連RFC**: RFC 4271
- **関連用語**: パスアトリビュート、経路選択プロセス、再配送

### OSPF(Open Shortest Path First)

- **定義**: リンクステート型 IGP の代表。IP プロトコル番号 89 で直接動作し、Hello によるネイバー自動発見、DR/BDR による隣接の削減、エリアによる LSDB の分割・要約を備える。OSPFv2(IPv4)は RFC 2328、OSPFv3(IPv6)は RFC 5340。
- **初出章**: `01_fundamentals/05_igp_overview.md`(言及は 03〜04 章にもあり)
- **関連RFC**: RFC 2328、RFC 5340
- **関連用語**: リンクステート、LSA、LSDB、SPF、エリア、DR / BDR、隣接、OSPFv3

### OSPFv3(OSPF version 3)

- **定義**: IPv6 に対応するために新造された OSPF の版(RFC 5340)。OSPFv2 とは互換性のない別プロトコルで、デュアルスタックでは両者を並走させる。設計の主題は「プロトコル処理からアドレス意味論を取り除く」こと — 足回りはリンクローカル(送信元は常にリンクローカル、宛先 ff02::5 / ff02::6)、ネイバー識別は常に 32 ビットのルータ ID、参加はサブネットではなくリンク単位(Instance ID により1リンク複数インスタンスも可)。Router/Network-LSA からプレフィックスを排除してトポロジ専用とし、アドレス情報は新設の Link-LSA(リンクスコープ)と Intra-Area-Prefix-LSA(エリアスコープ)へ分離したため、アドレスの追加・削除が SPF 再計算に波及しない。認証はプロトコル外(IPsec または Authentication Trailer = RFC 7166)。RFC 5838 により IPv4 ファミリも運べる。
- **初出章**: `04_ipv6/04_routing_ipv6.md`(言及は `01_fundamentals/05_igp_overview.md` が先行)
- **関連RFC**: RFC 5340、RFC 7166、RFC 5838
- **関連用語**: OSPF、LSA、リンクローカルアドレス、ルータ ID、IPv6

## P〜T

### PHP(Penultimate Hop Popping)

- **定義**: 出口 LER の1つ手前の LSR がラベルを pop してから渡すことで、出口での 2 回ルックアップ(LFIB → FIB)を 1 回にする最適化。出口 LER が予約ラベル implicit NULL(値 3)を配布することで「剥がしてから送ってくれ」と依頼する。ラベル内の TC(QoS 印)も一緒に消えるため、TC を出口まで届けたい場合は explicit NULL(IPv4 = 0、IPv6 = 2)を使う。
- **初出章**: `05_mpls_srv6/01_mpls_basics.md`
- **関連RFC**: RFC 3031、RFC 3032
- **関連用語**: MPLS、LSR / LER、LFIB

### PIM(Protocol Independent Multicast)

- **定義**: マルチキャストの配送木(送信元から全受信希望者へ届く木)をルータ間で構築するマルチキャストルーティングプロトコル。既存のユニキャスト経路表を利用するため protocol independent と呼ばれる。実用の主流は PIM-SM(Sparse Mode)で、RP(Rendezvous Point)と呼ぶ集合点を経由する共有木から始め、送信元への最短経路木へ切り替える。全員が送信者かつ受信者の場合向けに双方向共有木だけで動く PIM-BiDir もある。
- **初出章**: `02_vlan_vxlan_evpn/04_vxlan_control_plane.md`
- **関連RFC**: RFC 7761(PIM-SM)、RFC 5015(PIM-BiDir)
- **関連用語**: IGMP、BUM トラフィック

### PMTUD(Path MTU Discovery / 経路 MTU 探索)

- **定義**: 送信元が宛先までの経路上の最小 MTU を学習し、そのサイズに収めてパケットを送る仕組み(IPv4 = RFC 1191、IPv6 = RFC 8201)。経路上のルータが「大きすぎて転送できない」ときに返すエラー(IPv4 は Fragmentation Needed、IPv6 は ICMPv6 Packet Too Big = Type 2)への依存が本質。経路上フラグメンテーションを廃止した IPv6 では事実上必須であり、Packet Too Big をフィルタしてはならない(RFC 4890)。エラーが途中で落とされると「小さいパケットは通るのに大きいパケットだけ黙って消える」PMTUD ブラックホールになる。
- **初出章**: `04_ipv6/04_routing_ipv6.md`(言及は `01_fundamentals/01_l2_l3_recap.md`・`04_ipv6/01_why_ipv6.md` が先行)
- **関連RFC**: RFC 8201、RFC 1191、RFC 4890
- **関連用語**: MTU、IPv6、フラグメンテーション

### prefix-list(プレフィックスリスト)

- **定義**: 経路を宛先プレフィックスの値と長さの範囲で選別するフィルタ。各行は「プレフィックス/L + ge/le」の形で、「上位 L ビットの一致」と「プレフィックス長が範囲内(ge/le 省略時はちょうど L)」の2条件の AND を検査する。行は番号順の先勝ちで評価され、末尾は暗黙の deny。長さの範囲指定は、ロンゲストマッチの世界で「この集約の配下のどの細かさまで許すか」を直接記述するための構文である。
- **初出章**: `03_bgp/04_policy_control.md`
- **関連用語**: route-map、ロンゲストマッチ、経路集約
- **表記**: 実装の設定語彙に合わせ、英字のまま「prefix-list」と書く

### PVID(Port VLAN Identifier)

- **定義**: ポートに設定される、受信したタグなしフレームをどの VLAN に分類するかを決める値(入方向の分類規則)。出方向のタグ除去(untagged 設定)とは独立の属性であり、アクセスポートは「PVID と untagged を同じ VLAN に設定したポート」の慣用名である。
- **初出章**: `02_vlan_vxlan_evpn/01_vlan_basics.md`
- **関連用語**: VLAN、802.1Q、アクセスポート

### RD(Route Distinguisher / ルート識別子)

- **定義**: MP-BGP で運ぶ経路の先頭に付ける 8 オクテットの値。テナントごとのテーブルから出た「同じ中身」の経路(同一 MAC・同一プレフィックス)を BGP 上で別経路として共存させるための一意化の接頭辞であり、それ自体に仕分けの意味はない(仕分けは RT が担う)。実装は「VTEP アドレス:内部番号」等の形式で自動生成することが多い。
- **初出章**: `02_vlan_vxlan_evpn/05_evpn_vxlan.md`(詳細は第3部・第5部)
- **関連RFC**: RFC 4364、RFC 7432
- **関連用語**: RT、MP-BGP、MAC-VRF

### RIB(Routing Information Base)

- **定義**: 各情報源(直結・静的・動的プロトコル)が経路を登録する制御用テーブル。管理距離・メトリックによる選択はここで行われ、勝者が FIB へ展開される。本書で単に「ルーティングテーブル」と言う場合は RIB を指す。
- **初出章**: `01_fundamentals/02_routing_table_basics.md`
- **関連用語**: FIB、管理距離、メトリック

### route-map(ルートマップ)

- **定義**: マッチ条件(prefix-list、AS_PATH フィルタ、コミュニティ等の参照)とアクション(属性の書き換え)を束ねる、番号付きの規則列。番号順に評価し、全 match 句(AND)を満たした最初のエントリで確定する(permit なら set 句を適用して通し、deny なら落とす)。どのエントリにもマッチしなければ暗黙の deny。「route-map」は RFC に登場しない Cisco 由来の実装語彙であり、RFC 4271 が求めるのは「ローカルポリシーによる経路の選別と属性操作」という抽象のみ(Junos の policy-statement 等、表現形式は実装ごとに異なる)。
- **初出章**: `03_bgp/04_policy_control.md`
- **関連用語**: prefix-list、コミュニティ、パスアトリビュート

### RS / RA(Router Solicitation / Router Advertisement、ルータ要請/ルータ広告)

- **定義**: NDP のルータ発見と構成情報配布を担う対のメッセージ(ICMPv6 Type 133 / 134)。RA はルータが全ノード(ff02::1)へ周期的に放送し、デフォルトルータとしての残り寿命(Router Lifetime。0 = デフォルトルータではない)、プレフィックス情報(L / A フラグと valid / preferred lifetime)、M / O フラグ、MTU、RDNSS(RFC 8106)を運ぶ。RS はホストが全ルータ(ff02::2)へ送り、周期を待たず即時の RA を促す。RA の送信元はルータのリンクローカルアドレスで、そのままデフォルト経路のネクストホップになる。無認証の放送であるため、不正 RA への運用対策が RA Guard(RFC 6105)。
- **初出章**: `04_ipv6/03_ndp_slaac.md`(言及は `04_ipv6/01〜02` が先行)
- **関連RFC**: RFC 4861、RFC 8106、RFC 6105
- **関連用語**: NDP、SLAAC、DHCPv6、リンクローカルアドレス、デフォルトゲートウェイ

### RSVP-TE(Resource Reservation Protocol - Traffic Engineering)

- **定義**: 明示経路と帯域予約を持つ TE 用の LSP を張るシグナリングプロトコル(RFC 3209。ホスト向け資源予約プロトコル RSVP = RFC 2205 の転生)。入口の CSPF が決めた道を ERO に載せた PATH が行きで各ホップに状態を作り、RESV が帰りに帯域を予約しつつ下流割当のラベルを上流へ伝える。状態はソフトステート(周期リフレッシュ、軽減は RFC 2961)。Fast Reroute(RFC 4090)による高速迂回も担う。代償は網の中間に LSP ごとの状態が積み上がること(フルメッシュで N²)で、セグメントルーティングの動機になった。
- **初出章**: `05_mpls_srv6/02_ldp_rsvp_te.md`(言及は `05_mpls_srv6/01_mpls_basics.md` が先行)
- **関連RFC**: RFC 3209、RFC 2205、RFC 4090
- **関連用語**: トラフィックエンジニアリング、CSPF、LDP、MPLS、ソフトステート

### RT(Route Target / ルートターゲット)

- **定義**: 経路の取り込み先を決める仕分けラベル(BGP 拡張コミュニティの一種)。各 VRF / MAC-VRF は export RT(広告に付ける)と import RT(一致する経路を取り込む)を持ち、同じ RT を共有する VTEP 群が1つの仮想ネットワークの参加者集合になる。EVPN/VXLAN では「AS 番号:VNI」の自動導出(RFC 8365)が既定のため、AS 番号が揃わない設計では不一致に注意。
- **初出章**: `02_vlan_vxlan_evpn/05_evpn_vxlan.md`(拡張コミュニティとしての位置づけは `03_bgp/04_policy_control.md`)
- **関連RFC**: RFC 4360、RFC 8365
- **関連用語**: RD、ルートタイプ、EVPN、MAC-VRF、拡張コミュニティ
- **表記**: 本書では「RT-数字」(RT-2 等)はルートタイプを、単独の「RT」はルートターゲットを指すと使い分ける

### SLAAC(StateLess Address AutoConfiguration / ステートレスアドレス自動構成)

- **定義**: RA で放送されたプレフィックス(A フラグ = 1 の PIO)に、ホストが自分で生成した IID を接合し、DAD を経てアドレスを完成させる自動構成方式(RFC 4862)。ネットワーク側は個々のホストへの割り当てを配らず・覚えない(ステートレス = 台帳を持たない)ため、IID の生成方式(安定不透明 ID、一時アドレス)はホストが自由に選べる。64 ビット IID が前提であり、/64 より長いプレフィックスのサブネットでは動かない。アドレスの寿命(valid_lft / preferred_lft)は RA の受信のたびに更新され続ける。
- **初出章**: `04_ipv6/03_ndp_slaac.md`(言及は `04_ipv6/01〜02` が先行)
- **関連RFC**: RFC 4862
- **関連用語**: RS / RA、DAD、インタフェース ID、DHCPv6、NDP

### SPF(Shortest Path First / 最短経路優先計算)

- **定義**: リンクステート型プロトコルにおいて、各ルータが LSDB(地図)上で自分を起点とする最短経路木をダイクストラ(Dijkstra)法により計算する処理。LSDB の変化を契機に再実行され、結果が RIB へ差し出される。
- **初出章**: `01_fundamentals/04_distance_vector_link_state.md`
- **関連RFC**: RFC 2328(OSPFv2)
- **関連用語**: LSDB、リンクステート、メトリック

### SVI(Switched Virtual Interface)

- **定義**: L3 スイッチが VLAN ごとに持つ仮想的な L3 インタフェース。その VLAN に「内側から挿さったルータのポート」として振る舞い、端末のデフォルトゲートウェイになる。VLAN 間ルーティングの現在の主流形態(外付けルータ方式 router-on-a-stick に対して)。
- **初出章**: `02_vlan_vxlan_evpn/01_vlan_basics.md`
- **関連用語**: VLAN、デフォルトゲートウェイ

### TTL(Time to Live)

- **定義**: IPv4ヘッダのフィールドで、ルータ通過ごとに1減算され、0で破棄される。ルーティングループの安全弁。IPv6ではHop Limitに改称。
- **初出章**: `01_fundamentals/01_l2_l3_recap.md`
- **関連RFC**: RFC 791
- **関連用語**: ホップバイホップ転送

## U〜Z

### VLAN(Virtual LAN)

- **定義**: 1台の物理スイッチを複数の論理スイッチに分割する仕組み(IEEE 802.1Q)。学習・転送・フラッディングが VLAN ごとに分離され、1 VLAN = 1 ブロードキャストドメインとなる。識別子は 12 ビットの VID(使用可能範囲 1〜4094)。VLAN 間の通信は必ず L3(ルータ/SVI)を経由する。設計の定石は 1 VLAN = 1 IP サブネットの 1:1 対応。
- **初出章**: `02_vlan_vxlan_evpn/01_vlan_basics.md`(言及は `01_fundamentals/01_l2_l3_recap.md` にもあり)
- **関連用語**: 802.1Q、ブロードキャストドメイン、アクセスポート、SVI、VNI

### ULA(Unique Local Address / ユニークローカルアドレス)

- **定義**: 組織内利用のための IPv6 ユニキャスト空間 fc00::/7(実際に使えるのは L ビット = 1 の fd00::/8)。「fd + 40 ビットの乱数グローバル ID + 16 ビットサブネット ID + 64 ビット IID」の構造を持ち、乱数により登録機関なしで組織統合時の重複をほぼ回避する(RFC 1918 の重複問題への答え)。インターネットへは経路広告されない。NAT との併用ではなく GUA との複数アドレス併存が設計意図であり、RFC 6724 の宛先優先度は IPv4 より低い(ULA しか持たないホストは AAAA があっても IPv4 を選ぶ)。
- **初出章**: `04_ipv6/02_addressing.md`
- **関連RFC**: RFC 4193
- **関連用語**: GUA、プライベートアドレス、IPv6

### uRPF(unicast Reverse Path Forwarding)

- **定義**: 受信パケットの送信元アドレスをルーティングテーブルで引き、経路が受信インタフェースを向いていなければ破棄する送信元検証機構。アドレス偽装対策の基本。
- **初出章**: `01_fundamentals/02_routing_table_basics.md`
- **関連RFC**: RFC 3704
- **関連用語**: ロンゲストマッチ

### VNI(VXLAN Network Identifier)

- **定義**: VXLAN ヘッダ内の 24 ビットのセグメント識別子(約 1677 万)。オーバーレイ上の L2 セグメントを識別し、MAC 学習のキーは (VNI, MAC) の組になる。VLAN の VID と相似だが、意味を解釈するのは VTEP だけであり端末は関知しない。端末収容側の境界で VLAN と対応付けられる(例: VLAN 10 ↔ VNI 5000)。
- **初出章**: `02_vlan_vxlan_evpn/03_vxlan_fundamentals.md`
- **関連RFC**: RFC 7348
- **関連用語**: VXLAN、VTEP、VLAN

### VTEP(VXLAN Tunnel End Point)

- **定義**: VXLAN トンネルの出入口となる機能・装置(ハイパーバイザ内の仮想スイッチ、リーフスイッチなど)。端末フレームのカプセル化/デカプセル化と MAC 学習を担う。オーバーレイで MAC を学習するのは VTEP だけであり、アンダーレイの中継ルータは外側 IP しか見ない。VTEP のアドレスにはループバックを使うのが定石。
- **初出章**: `02_vlan_vxlan_evpn/03_vxlan_fundamentals.md`
- **関連RFC**: RFC 7348
- **関連用語**: VXLAN、VNI、アンダーレイネットワーク、フラッド&ラーン

### VXLAN(Virtual eXtensible Local Area Network)

- **定義**: L3 アンダーレイの上に仮想 L2 セグメント(オーバーレイ)を構築する MAC-in-UDP カプセル化。宛先 UDP ポートは 4789(IANA)、オーバーヘッドは 50 オクテット(外側 IPv4・タグなし時)。外側 UDP 送信元ポートに内側フローのハッシュを載せてアンダーレイの ECMP を活かす。中継網を端末 MAC の学習から解放することで、QinQ が越えられなかった L2 の規模の壁を越える。
- **初出章**: `02_vlan_vxlan_evpn/03_vxlan_fundamentals.md`
- **関連RFC**: RFC 7348(Informational)
- **関連用語**: VTEP、VNI、オーバーレイネットワーク、BUM トラフィック、フラッド&ラーン、EVPN

## あ〜わ(日本語見出し語)

### アクセスポート(access port)

- **定義**: タグなしフレームだけを扱い、単一の VLAN に属するポート(802.1Q の用語では untagged なメンバーポート)。入方向は PVID によりタグなしフレームを VLAN に分類し、出方向はタグを外して送出する。端末に 802.1Q を意識させない境界として機能する。
- **初出章**: `02_vlan_vxlan_evpn/01_vlan_basics.md`
- **関連用語**: PVID、VLAN、802.1Q、トランクポート

### アンダーレイネットワーク(underlay network)

- **定義**: オーバーレイのトンネルパケットを実際に運ぶ、下位の物理ネットワーク。VXLAN では VTEP 間の IP 到達性を提供する L3 網を指し、ECMP・IGP の高速収束といった L3 の道具立てをそのまま使える。中継ルータはオーバーレイの中身(端末の MAC など)を一切関知しない。単に「アンダーレイ」とも呼ぶ。
- **初出章**: `02_vlan_vxlan_evpn/03_vxlan_fundamentals.md`
- **関連用語**: オーバーレイネットワーク、VXLAN、VTEP、ECMP

### インタフェース ID(interface identifier / IID)

- **定義**: IPv6 ユニキャストアドレスの下位 64 ビット。サブネット内でインタフェースを識別する(不透明なビット列として扱う — RFC 7136)。生成方式は Modified EUI-64(MAC 由来。ネットワークをまたいで同一になる追跡問題を持つ)、一時アドレス(RFC 8981、乱数で短寿命に世代交代)、安定不透明 ID(RFC 7217、同一網では安定・別網では別値。RFC 8064 が新規実装の既定に勧告)。64 ビットという大きさは収容数ではなく、端末が衝突をほぼ心配せず自己生成するための名前空間である。
- **初出章**: `04_ipv6/02_addressing.md`
- **関連RFC**: RFC 4291、RFC 7217、RFC 8981、RFC 8064
- **関連用語**: IPv6、GUA、リンクローカルアドレス

### エニーキャスト(anycast)

- **定義**: 同一のユニキャストアドレスを複数のノードに割り当て、経路制御によって「最寄りの1台」へ届ける配送形態。IPv6 では体系に明示的に含まれるが専用フォーマットはなく、ワイヤ上ではユニキャストと区別できない。RFC 4291 はサブネットルータエニーキャスト(IID 全ゼロ)を予約する。広域では同一プレフィックスの複数拠点からの BGP 広告として実現される(DNS ルートサーバ等)。
- **初出章**: `04_ipv6/02_addressing.md`
- **関連RFC**: RFC 4291
- **関連用語**: エニーキャストゲートウェイ、GUA

### エニーキャストゲートウェイ(anycast gateway)

- **定義**: 全リーフ(VTEP)が、各サブネットの SVI を同一のゲートウェイ IP アドレスと同一の MAC アドレスで持つ分散ゲートウェイ構成。端末から見るとデフォルトゲートウェイが常に「手元のリーフ」になり、ライブマイグレーションで収容先が変わってもゲートウェイの再解決が不要で通信が途切れない。
- **初出章**: `02_vlan_vxlan_evpn/05_evpn_vxlan.md`
- **関連用語**: IRB、SVI、デフォルトゲートウェイ

### エリア(area / OSPF エリア)

- **定義**: OSPF が LSDB と SPF 計算の範囲を分割する単位。詳細な地図(Type 1/2 LSA)のフラッディングはエリア内に閉じ、エリア境界ルータ(ABR)が他エリアへ距離情報(Type 3)だけを渡す。すべてのエリアはバックボーンエリア(エリア 0)に接続しなければならず、この制約がエリア間ループを防ぐ。
- **初出章**: `01_fundamentals/05_igp_overview.md`
- **関連RFC**: RFC 2328
- **関連用語**: LSA、LSDB、OSPF

### エンドツーエンド原則(end-to-end principle)

- **定義**: 「網の中間は状態を持たずにパケットを運ぶことに徹し、知性と状態は末端(エンドホスト)に置く」というインターネットの設計原則(アーキテクチャ原則としては RFC 1958、NAT による透明性喪失の分析は RFC 2775)。中間の単純さが規模と新プロトコルの導入容易性を支える。NAT はこの原則の重大な例外であり、IPv6 の潤沢なアドレス空間はこの原則の回復を意図している。
- **初出章**: `04_ipv6/01_why_ipv6.md`
- **関連RFC**: RFC 1958、RFC 2775
- **関連用語**: NAT、IPv6、ホップバイホップ転送

### オーバーレイネットワーク(overlay network)

- **定義**: 既存のネットワーク(アンダーレイ)の上に、トンネルによって構築される仮想ネットワーク。VXLAN では端末に見せる仮想 L2 セグメントを指す。物理トポロジと論理トポロジを分離し、物理網の設計(L3)と端末への提供形態(L2)を独立に選べるようにする。単に「オーバーレイ」とも呼ぶ。
- **初出章**: `02_vlan_vxlan_evpn/03_vxlan_fundamentals.md`
- **関連用語**: アンダーレイネットワーク、VXLAN、カプセル化

### オンリンク(on-link)

- **定義**: ある宛先(プレフィックス)が同一リンク上にあり、ルータを介さず NDP で直接アドレス解決してよいという知識。IPv6 では「自アドレスと同じプレフィックス = 同一サブネット = 直接届く」という IPv4 の暗黙の等式が廃止され(RFC 5942)、RA の PIO の L フラグ等で明示的に教わったものだけがオンリンクになる。既定はオフリンク(デフォルトルータへ送る)であり、アドレス構成の可否(A フラグ)とは独立の知識である。
- **初出章**: `04_ipv6/03_ndp_slaac.md`
- **関連RFC**: RFC 5942、RFC 4861
- **関連用語**: NDP、RS / RA、デフォルトゲートウェイ

### 拡張コミュニティ(extended community)

- **定義**: コミュニティを 8 オクテットへ拡張し、先頭に型フィールドを持たせた属性(RFC 4360。型ごとに transitive 性も定義される)。型により構造と意味が機械的に定まる「構造化された荷札」であり、VPN 系機能の基盤になった。RT(ルートターゲット)はその Route Target 型である。
- **初出章**: `03_bgp/04_policy_control.md`(RT としての利用は `02_vlan_vxlan_evpn/05_evpn_vxlan.md` が先行)
- **関連RFC**: RFC 4360
- **関連用語**: コミュニティ、RT、ラージコミュニティ

### 拡張ヘッダ(extension header / IPv6 拡張ヘッダ)

- **定義**: IPv6 が IPv4 の可変長オプションに代えて採用した仕組み。基本ヘッダの Next Header から数珠つなぎに連なる独立のヘッダ群(Hop-by-Hop Options = 0、Routing = 43、Fragment = 44、ESP = 50、AH = 51、Destination Options = 60)で、原則として宛先ノードだけが処理し、中継ルータは基本ヘッダのみを見る(例外は基本ヘッダ直後に限定される Hop-by-Hop)。Routing ヘッダの Type 4 が SRv6 の SRH(RFC 8754)。中間機器による拡張ヘッダ付きパケットの廃棄が観測されている点(RFC 7872)には注意。
- **初出章**: `04_ipv6/01_why_ipv6.md`
- **関連RFC**: RFC 8200
- **関連用語**: IPv6、カプセル化、EtherType

### カプセル化(encapsulation)

- **定義**: 上位レイヤのデータを下位レイヤのヘッダで包んで伝送する仕組み。各レイヤは自レイヤのヘッダのみを解釈する。
- **初出章**: `01_fundamentals/01_l2_l3_recap.md`
- **関連RFC**: RFC 1122
- **関連用語**: EtherType、フレーム/パケット/セグメント、MPLS

### 管理距離(Administrative Distance / AD)

- **定義**: 同一プレフィックスへの経路を複数の情報源が主張するとき、どれを RIB に載せるかを決める「情報源の信頼度」。小さいほど優先。RFC 標準ではなくベンダー実装上の慣例(Juniper では route preference)。
- **初出章**: `01_fundamentals/02_routing_table_basics.md`
- **関連用語**: メトリック、RIB

### ケイパビリティ広告(capability advertisement)

- **定義**: BGP の OPEN メッセージの Optional Parameters で、対応する拡張機能(MP-BGP のアドレスファミリ、Route Refresh、32 ビット AS 番号など)を双方が申告する仕組み。双方が申告した機能(積集合)だけがセッションで有効になる。交換は OPEN でしか行えないため、アドレスファミリの後付けはセッションの再確立を伴う。BGP が後方互換を保って拡張されてきた基盤。
- **初出章**: `03_bgp/01_bgp_basics.md`(詳細は `03_bgp/05_mp_bgp.md`)
- **関連RFC**: RFC 5492
- **関連用語**: BGP、MP-BGP、AFI / SAFI、ピア

### 経路集約(route aggregation)

- **定義**: 複数の具体的な経路を1つの広いプレフィックスにまとめて扱う・広告すること。ルーティングテーブルの規模と経路変動の波及を抑える。集約ルータには Null ルートを併置するのが定石。
- **初出章**: `01_fundamentals/02_routing_table_basics.md`
- **関連RFC**: RFC 4632
- **関連用語**: CIDR、Null ルート
- **表記**: route の訳語は本書では「経路」に統一する(「ルート」とは書かない。ただし「デフォルトルート」「Null ルート」など複合語の慣用名は除く)

### 経路選択プロセス(Decision Process)

- **定義**: BGP スピーカーが同一プレフィックスへの複数の経路から Loc-RIB に載せる1本を決める手続き(RFC 4271 Section 9.1)。単一メトリックの最小化ではなく、LOCAL_PREF → AS_PATH 長 → ORIGIN → MED → eBGP/iBGP → ネクストホップへの IGP コスト → BGP Identifier という多段の属性比較で、差がついた段で終了する。管理者はポリシーで属性を書き換えることにより判定結果を操作する。Weight や「古い経路の優先」は RFC 外の実装慣例。ルートリフレクタ環境では終盤に修正が入る(ORIGINATOR_ID の代用と CLUSTER_LIST 長比較、RFC 4456)。
- **初出章**: `03_bgp/03_path_attributes.md`(言及は `03_bgp/01_bgp_basics.md` にもあり)
- **関連RFC**: RFC 4271
- **関連用語**: パスアトリビュート、LOCAL_PREF、MED、Adj-RIB-In / Loc-RIB / Adj-RIB-Out、ORIGINATOR_ID / CLUSTER_LIST

### 経路リーク(route leak)

- **定義**: 経路広告が意図した範囲を超えて伝搬すること(RFC 7908 が類型を定義)。典型は、複数の上流を持つ AS が上流 A から学んだ経路を上流 B へ広告してしまい、他人どうしのトラフィックを自腹で運ぶ・細い回線に大量のトラフィックを呼び込む事故。出口での関係タグ(コミュニティ)による選別と、RFC 8212 の既定(eBGP は明示的なポリシーなしに経路を受けず・広告しない)が防御になる。
- **初出章**: `03_bgp/04_policy_control.md`
- **関連RFC**: RFC 7908、RFC 8212
- **関連用語**: コミュニティ、prefix-list、eBGP / iBGP

### コミュニティ(COMMUNITY / BGP community)

- **定義**: 経路に付ける 32 ビット値の荷札の集合(optional transitive 属性、RFC 1997)。表記は上位・下位 16 ビットを区切った「AS:値」が慣例。プロトコルは値の意味を定めず、意味は貼る者と読む者の合意で決まる — 入口で受信の文脈(関係・拠点など)を記録する情報タグと、相手の公開ポリシーに処理を依頼するアクション依頼の2用法がある。例外として動作が世界共通で予約された well-known コミュニティ(NO_EXPORT、NO_ADVERTISE、BLACKHOLE = RFC 7999、GRACEFUL_SHUTDOWN = RFC 8326)がある。
- **初出章**: `03_bgp/04_policy_control.md`(言及は第2部・`03_bgp/03_path_attributes.md` にもあり)
- **関連RFC**: RFC 1997、RFC 7999、RFC 8326
- **関連用語**: パスアトリビュート、拡張コミュニティ、ラージコミュニティ、route-map

### コンフェデレーション(confederation / BGP コンフェデレーション)

- **定義**: iBGP フルメッシュの規模問題への解の一つ(RFC 5065)。AS を内部専用の AS 番号を持つ複数のメンバー AS に分割し、メンバー AS 内は iBGP、メンバー AS 間は eBGP の変種(confederation eBGP)で接続する。メンバー AS 境界では AS_CONFED_SEQUENCE でループを検出しつつ、LOCAL_PREF・NEXT_HOP・MED は変更せずに運び、パス長比較でも数えない。真の AS 境界で内部セグメントはすべて取り除かれ、外部からは単一の AS に見える。導入が全ルータに及ぶため、実務ではルートリフレクタが主流。
- **初出章**: `03_bgp/06_large_scale_design.md`(言及は `03_bgp/02_ibgp_ebgp.md` にもあり)
- **関連RFC**: RFC 5065
- **関連用語**: フルメッシュ、ルートリフレクタ、AS_PATH、eBGP / iBGP

### 再帰的ルックアップ(recursive lookup)

- **定義**: ネクストホップが直結でない経路について、そのネクストホップ自身への経路をテーブルで引き直し、直結インタフェースに行き着くまで解決する処理。解決できない経路は無効となり FIB に載らない。
- **初出章**: `01_fundamentals/02_routing_table_basics.md`
- **関連用語**: ネクストホップ、FIB

### 再配送(redistribution)

- **定義**: ある情報源の経路(静的経路や他プロトコルの経路)を動的ルーティングプロトコルに注入し、他のルータへ広めてもらうこと。誤経路が広域拡散する入口にもなるため、注入点でのプレフィックスフィルタが定石。
- **初出章**: `01_fundamentals/03_static_vs_dynamic.md`
- **関連用語**: 静的経路、動的ルーティング

### 収束(convergence)

- **定義**: トポロジ変化の検知後、全ルータのルーティングテーブルが新しいトポロジを反映した一貫状態に落ち着くまでの過程。完了までは損失やループが起こりうる。収束の速さと正確さはルーティングプロトコル設計の中心的評価軸。
- **初出章**: `01_fundamentals/03_static_vs_dynamic.md`
- **関連用語**: 動的ルーティング、BFD

### スプリットホライズン(split horizon)

- **定義**: ディスタンスベクタ型で、ある隣接ルータから学んだ経路をその隣接ルータへ広告し返さない規則。2台間の無限カウントを防ぐ。到達不能(距離無限)として明示的に広告し返す変種をポイズンリバース(poison reverse)と呼ぶ。3台以上の環状の誤情報伝搬は防げない。
- **初出章**: `01_fundamentals/04_distance_vector_link_state.md`
- **関連RFC**: RFC 2453
- **関連用語**: ディスタンスベクタ、無限カウント

### 静的経路(static route)

- **定義**: 管理者が手で設定する経路。決定的でプロトコルのオーバーヘッドがない一方、直結リンク断とネクストホップ解決不能にしか反応せず、その先の障害・構成変化には追従しない。
- **初出章**: `01_fundamentals/02_routing_table_basics.md`(主題としては `01_fundamentals/03_static_vs_dynamic.md`)
- **関連用語**: 動的ルーティング、管理距離、ブラックホール

### ソフトステート(soft state)

- **定義**: 周期的なリフレッシュによって維持され、リフレッシュが途絶えるとタイムアウトで自然消滅する状態の持ち方(RSVP の PATH/RESV が代表)。障害や設定漏れで残骸が残らない自己清掃性が利点だが、状態数に比例した周期処理のコストを網の中間に強いる。BGP の「TCP + Keepalive の上で差分だけ送り、明示的に撤回するまで有効」というハードステートと対をなす設計選択。
- **初出章**: `05_mpls_srv6/02_ldp_rsvp_te.md`(ハードステートの言及は `03_bgp/01_bgp_basics.md` が先行)
- **関連用語**: RSVP-TE、ホールドタイム、BGP

### ゾーン ID(zone ID / スコープ ID)

- **定義**: スコープを持つアドレス(実用上はリンクローカル)がどのゾーン(どのリンク = どのインタフェース)のものかを指定する識別子(RFC 4007)。`fe80::1%eth0` の `%eth0`。リンクローカルアドレスはリンクごとに重複しうるため、アドレス単体では宛先が一意に定まらない — この指定漏れがリンクローカル宛て操作の典型的な失敗原因になる。
- **初出章**: `04_ipv6/02_addressing.md`(表記の言及は `04_ipv6/01_why_ipv6.md` が先行)
- **関連RFC**: RFC 4007
- **関連用語**: リンクローカルアドレス、IPv6

### 直結経路(connected route)

- **定義**: インタフェースへの IP アドレス設定に伴い自動的に RIB へ載る、そのサブネットへの経路。インタフェース断で即座に消え、依存する経路を連鎖的に無効化する。
- **初出章**: `01_fundamentals/02_routing_table_basics.md`
- **関連用語**: 管理距離、再帰的ルックアップ

### ディスタンスベクタ(distance vector)

- **定義**: 動的ルーティングの方式の一つ。各ルータが「宛先までの距離と方向」(計算結果)を隣接ルータへ周期的に広告し、分散ベルマンフォード計算で経路を決める。単純で軽量だが、経路の中身が広告に含まれないため無限カウントの病理を持つ。代表は RIP。
- **初出章**: `01_fundamentals/04_distance_vector_link_state.md`
- **関連RFC**: RFC 1058(RIPv1)、RFC 2453(RIPv2)
- **関連用語**: リンクステート、パスベクタ、無限カウント、スプリットホライズン

### デフォルトゲートウェイ

- **定義**: ホストが自サブネット外宛てのパケットを託すルータ。ホストはゲートウェイのIPをARP/NDPで解決してフレームを送る。
- **初出章**: `01_fundamentals/01_l2_l3_recap.md`
- **関連用語**: ARP、ホップバイホップ転送

### デフォルトルート(default route)

- **定義**: プレフィックス長 0 の経路(IPv4 では 0.0.0.0/0)。すべての宛先にマッチするが、ロンゲストマッチにより常に最後の受け皿として機能する。
- **初出章**: `01_fundamentals/02_routing_table_basics.md`
- **関連用語**: ロンゲストマッチ、デフォルトゲートウェイ

### デュアルスタック(dual stack)

- **定義**: 1つのノード・ネットワークが IPv4 と IPv6 の両プロトコルスタックを同時に動かす構成。相互互換のない両プロトコルをつなぐ移行戦略の基本形で、通信ごとに使うプロトコルを選ぶ(名前解決の A / AAAA と Happy Eyeballs = RFC 8305 が実質の選択機構)。片方だけが壊れている環境では「なんとなく遅い」型の障害を生む。
- **初出章**: `04_ipv6/01_why_ipv6.md`(移行技術の全体像は `04_ipv6/05_transition_technologies.md`)
- **関連RFC**: RFC 8305
- **関連用語**: IPv6、NAT、DS-Lite、MAP-E、464XLAT

### 透過的ブリッジング(transparent bridging)

- **定義**: L2スイッチの動作原理。送信元MACの学習・既知宛先の転送・未知宛先/ブロードキャストのフラッディングの3動作から成る。端末側はスイッチの存在を意識しない。
- **初出章**: `01_fundamentals/01_l2_l3_recap.md`
- **関連用語**: フラッディング、ブロードキャストドメイン

### 動的ルーティング(dynamic routing)

- **定義**: ルータどうしがルーティングプロトコルで経路情報を交換し、経路を自動計算・維持する方式。トポロジ変化への追従と規模への対応を実現するが、資源消費・複雑さ・「伝聞を信じる」リスクを伴う。
- **初出章**: `01_fundamentals/02_routing_table_basics.md`(主題としては `01_fundamentals/03_static_vs_dynamic.md`)
- **関連用語**: 静的経路、収束、ネイバー

### トラフィックエンジニアリング(traffic engineering / TE)

- **定義**: IGP の最短経路が生む偏り(最短の道だけが混み、迂回路が遊ぶ)を避け、トラフィックの流れを帯域・ポリシーの制約に基づいて意図的に配置する技術体系。部品は3つ — 資源つきの地図(IGP の TE 拡張: OSPF-TE = RFC 3630 / IS-IS TE = RFC 5305)、入口での制約付き経路計算(CSPF)、道の設置(RSVP-TE またはセグメントルーティング)。転送の根拠を宛先から切り離せる MPLS/SR が前提になる。
- **初出章**: `05_mpls_srv6/02_ldp_rsvp_te.md`(言及は `05_mpls_srv6/01_mpls_basics.md` が先行)
- **関連RFC**: RFC 3630、RFC 5305、RFC 3209
- **関連用語**: RSVP-TE、CSPF、MPLS、メトリック、ECMP

### トランクポート(trunk port)

- **定義**: 複数 VLAN の tagged メンバーであるポートの慣用名(802.1Q の仕様に「トランク」という語はなく、Cisco 由来の業界用語。ベンダーによっては tagged port)。1本のリンクに複数 VLAN のタグ付きフレームを多重し、VLAN のブロードキャストドメインをスイッチ間に延長する。運ぶ VLAN の集合は許可 VLAN リストで明示的に絞るのが定石。
- **初出章**: `02_vlan_vxlan_evpn/02_trunking_native_vlan.md`
- **関連用語**: 802.1Q、ネイティブ VLAN、アクセスポート、PVID

### ネイティブ VLAN(native VLAN)

- **定義**: トランク上でタグなしのまま運ばれる VLAN。実体はトランクポートの PVID(かつ untagged 送出に設定された VLAN)。所属情報がフレームに載らず両端の設定一致だけに依存するため、不一致は VLAN の混線を、アクセス VLAN との重複は二重タグ攻撃(VLAN ホッピング)の前提を生む。定石は未使用 VID の割り当て、または全フレームのタグ付き送出。
- **初出章**: `02_vlan_vxlan_evpn/02_trunking_native_vlan.md`
- **関連用語**: トランクポート、PVID、802.1Q

### ネイバー(neighbor / 隣接ルータ)

- **定義**: 動的ルーティングプロトコルにおいて経路情報を交換する相手のルータ。専用パケットで自動発見するプロトコル(OSPF)と、管理者が明示設定するプロトコル(BGP、この場合はピアと呼ぶ)がある。OSPF ではネイバーのうち LSDB 同期まで行う関係を隣接(アジャセンシー)と呼んで区別する。
- **初出章**: `01_fundamentals/03_static_vs_dynamic.md`
- **関連用語**: 動的ルーティング、収束、隣接、ピア

### ネクストホップ(next hop)

- **定義**: ホップバイホップ転送において、パケットを次に引き渡す隣接ルータ(のアドレス)。直結でないアドレスが指定された場合は再帰的ルックアップで解決される。
- **初出章**: `01_fundamentals/02_routing_table_basics.md`
- **関連用語**: ホップバイホップ転送、再帰的ルックアップ

### ネクストホップセルフ(next-hop-self)

- **定義**: 境界ルータが eBGP で学んだ経路を iBGP へ広告するとき、NEXT_HOP を(変更しないという原則を破って)自分のループバックアドレスに書き換える設定。AS 外部のアドレスがネクストホップとして内部に案内され再帰的ルックアップに失敗する問題を、「外の知識は境界で打ち切る」ことで解決する。IGP がループバックを常に運んでいる定石とセットで機能する。
- **初出章**: `03_bgp/02_ibgp_ebgp.md`
- **関連RFC**: RFC 4271(Section 5.1.3 の NEXT_HOP 規則)
- **関連用語**: ネクストホップ、再帰的ルックアップ、eBGP / iBGP

### パスアトリビュート(path attribute)

- **定義**: BGP の UPDATE で NLRI とともに運ばれる経路の付帯情報(TLV 形式)。well-known / optional × transitive / non-transitive の4分類が「未知の属性を受け取ったときの動作」まで規定しており(未知の optional transitive は Partial ビットを立てて中継)、これが BGP の後方互換な拡張性の基盤になっている。経路選択の材料とポリシーの荷札という2つの役割を持つ。
- **初出章**: `03_bgp/03_path_attributes.md`(言及は `03_bgp/01_bgp_basics.md` にもあり)
- **関連RFC**: RFC 4271
- **関連用語**: AS_PATH、LOCAL_PREF、MED、ORIGIN、NLRI、経路選択プロセス、コミュニティ

### パスベクタ(path vector)

- **定義**: 動的ルーティングの方式の一つで、BGP が採用する。広告に距離ではなく「経路が通ってきた道筋(AS のリスト)」を丸ごと載せ、受信側は道筋に自分が含まれていれば広告を破棄する。ループ検出を情報自体に内蔵することで、ディスタンスベクタの無限カウント問題を解決した。
- **初出章**: `01_fundamentals/04_distance_vector_link_state.md`(詳細は `03_bgp/01_bgp_basics.md`)
- **関連RFC**: RFC 4271
- **関連用語**: ディスタンスベクタ、無限カウント、AS_PATH、BGP

### ピア(peer)

- **定義**: BGP における経路交換の相手(ネイバー)の呼称。OSPF と異なり自動発見せず、管理者が明示的に設定して確立する(ピアの明示設定は AS 境界での意図しない経路交換を防ぐ安全装置でもある)。セッション確立の仕組みは `03_bgp/01_bgp_basics.md` で扱う。
- **初出章**: `01_fundamentals/05_igp_overview.md`(詳細は `03_bgp/01_bgp_basics.md`)
- **関連RFC**: RFC 4271
- **関連用語**: ネイバー、隣接、BGP、ホールドタイム

### プライベートアドレス(private address)

- **定義**: 組織内部で自由に再利用してよいと予約された IPv4 アドレス空間(RFC 1918: 10.0.0.0/8、172.16.0.0/12、192.168.0.0/16)。グローバルな一意性を「インターネットに出る瞬間だけ」に緩めることでアドレス消費を圧縮する、IPv4 枯渇延命策の一つ。インターネットへは NAT による変換なしには到達できず、インターネット上の経路表に現れてはならない。
- **初出章**: `04_ipv6/01_why_ipv6.md`(言及は第1部にもあり)
- **関連RFC**: RFC 1918
- **関連用語**: NAT、CGN

### ブラックホール(blackhole)

- **定義**: 実際には届かない宛先への経路がテーブルに残り続け、トラフィックを吸い込んで黙って失われる状態。静的経路の典型的な故障モード。意図的な廃棄(Null ルート)とは区別する。
- **初出章**: `01_fundamentals/03_static_vs_dynamic.md`
- **関連用語**: 静的経路、Null ルート

### フラッディング(flooding)

- **定義**: (L2)スイッチが宛先MAC未学習のフレーム(unknown unicast)やブロードキャストを、受信ポート以外の全ポートへ送出する動作。(リンクステート)リンク状態情報を改変せず全ルータへ確実に中継する動作。後者はシーケンス番号による新旧判定・受信確認・寿命(エージング)の規律の下で行われる点で、前者の無秩序な溢れ出しとは異なる。
- **初出章**: `01_fundamentals/01_l2_l3_recap.md`(リンクステートの用法は `01_fundamentals/04_distance_vector_link_state.md`)
- **関連用語**: 透過的ブリッジング、ブロードキャストドメイン、リンクステート、LSDB

### フラッド&ラーン(Flood-and-Learn)

- **定義**: RFC 7348 が定める VXLAN のデータプレーン学習方式。デカプセル化時に内側フレームの送信元 MAC と外側 IP の送信元アドレス(送信元 VTEP)の対応を学習し、BUM トラフィックは VNI に対応付けたアンダーレイのマルチキャストグループへ送る。コントロールプレーンを持たない単純さが利点だが、初回の必然的なフラッディングとマルチキャスト運用への依存が限界(EVPN への移行動機)。
- **初出章**: `02_vlan_vxlan_evpn/03_vxlan_fundamentals.md`
- **関連RFC**: RFC 7348
- **関連用語**: BUM トラフィック、VTEP、透過的ブリッジング、ヘッドエンドレプリケーション、EVPN

### フルメッシュ(full mesh / iBGP フルメッシュ)

- **定義**: AS 内のすべての iBGP スピーカーが互いに直接ピアを張る構成(n 台で n(n-1)/2 セッション)。iBGP から学んだ経路を iBGP へ再広告できない規則の下で全員に経路を行き渡らせるための原則形。物理リンクではなく TCP セッションの網であり、規模とともに設定・運用が2乗で破綻するため、大規模ではルートリフレクタまたはコンフェデレーションで緩和する。
- **初出章**: `03_bgp/02_ibgp_ebgp.md`
- **関連RFC**: RFC 4271、RFC 4456(ルートリフレクタ)、RFC 5065(コンフェデレーション)
- **関連用語**: eBGP / iBGP、ピア、ルートリフレクタ、コンフェデレーション

### フレーム / パケット / セグメント

- **定義**: 各レイヤの伝送単位の呼称。L2はフレーム、L3はパケット、L4(TCP)はセグメント。本書ではこの区別を厳密に用いる(スイッチはフレームを、ルータはパケットを転送する)。
- **初出章**: `01_fundamentals/01_l2_l3_recap.md`
- **関連用語**: カプセル化

### フローティングスタティックルート(floating static route)

- **定義**: 管理距離を動的プロトコルよりわざと弱く設定した静的経路。通常時はテーブルに載らず待機し、動的経路が撤回されると浮上してバックアップ経路として機能する。主経路がテーブルから消えない障害では切り替わらない点に注意。
- **初出章**: `01_fundamentals/03_static_vs_dynamic.md`
- **関連用語**: 管理距離、静的経路、BFD

### ブロードキャストドメイン

- **定義**: ブロードキャストフレーム(およびフラッディング)が届く範囲。ルータ(またはVLAN)で分割される。
- **初出章**: `01_fundamentals/01_l2_l3_recap.md`
- **関連用語**: フラッディング、VLAN

### ヘッドエンドレプリケーション(Head-End Replication / HER、ingress replication)

- **定義**: BUM フレームを、入口(head-end)の VTEP が宛先 VTEP の数だけユニキャストで複製して送る方式。アンダーレイからマルチキャストの運用体系を不要にする代わりに、複製コストが送信側 VTEP に集中し、宛先一覧(フラッディングリスト)を別途持つ必要がある。一覧を静的設定する形は静的経路と同じ限界を持ち、EVPN のルートタイプ3で自動構築する形が実務の主流。
- **初出章**: `02_vlan_vxlan_evpn/04_vxlan_control_plane.md`(言及は `02_vlan_vxlan_evpn/03_vxlan_fundamentals.md`)
- **関連用語**: BUM トラフィック、VTEP、EVPN、フラッド&ラーン

### ホールドタイム(hold time)

- **定義**: BGP のセッション生存判定のタイムアウト値。OPEN メッセージで双方が提案し、小さいほうが採用される(値は 0 = 生存監視無効、または 3 秒以上。RFC 4271 の推奨値は 90 秒だが実装既定は 180 秒が多い)。この時間 UPDATE も KEEPALIVE も受信しないとピアは死んだとみなされセッションが破棄される。KEEPALIVE はホールドタイムの 1/3 の間隔で送るのが規定。
- **初出章**: `03_bgp/01_bgp_basics.md`
- **関連RFC**: RFC 4271
- **関連用語**: BGP、ピア、BFD

### ホップバイホップ転送

- **定義**: 各ルータが宛先への全経路を知らずに「次の一歩(ネクストホップ)」だけを決めて転送するL3の動作原理。各ホップでL2ヘッダは書き換えられ、L3アドレスは不変。
- **初出章**: `01_fundamentals/01_l2_l3_recap.md`
- **関連RFC**: RFC 1812
- **関連用語**: ロンゲストマッチ、TTL

### マルチトポロジ(multi-topology / MT)

- **定義**: 1つの IS-IS の中でアドレスファミリ(トポロジ)ごとに独立のリンク集合と SPF 計算を持たせる拡張(RFC 5120)。素朴に IPv6 用 TLV を足しただけのシングルトポロジ構成では IPv4 と IPv6 が同一の最短経路木を共有するため、片方のファミリ未対応のリンクが混ざるとそのファミリの経路が壊れる — この不一致問題を、地図を分けることで解く。OSPFv2/v3 並走が「プロトコルが別」という形で最初から2枚の地図に分かれているのと対比される。
- **初出章**: `04_ipv6/04_routing_ipv6.md`
- **関連RFC**: RFC 5120
- **関連用語**: IS-IS、SPF、デュアルスタック

### 無限カウント(counting to infinity)

- **定義**: ディスタンスベクタ型で、失われた宛先への経路をルータどうしが互いの古い広告から学び直し、距離だけが際限なく増えていく現象。進行中は転送ループを伴う。広告に「経路がどこを通るか」が含まれないことが根本原因で、距離上限(RIP では 16 = 無限)・スプリットホライズン等で緩和する。
- **初出章**: `01_fundamentals/04_distance_vector_link_state.md`
- **関連RFC**: RFC 2453
- **関連用語**: ディスタンスベクタ、スプリットホライズン、TTL

### メトリック(metric)

- **定義**: 同一ルーティングプロトコル内で経路の優劣を測る値(小さいほど良い)。意味と計算方法はプロトコル固有であり(RIP はホップ数、OSPF はコスト)、プロトコル間では比較できない。
- **初出章**: `01_fundamentals/02_routing_table_basics.md`
- **関連用語**: 管理距離、ECMP

### 要請ノードマルチキャスト(solicited-node multicast address)

- **定義**: ユニキャスト/エニーキャストアドレスの下位 24 ビットを `ff02::1:ff00:0/104` に接合して作るマルチキャストアドレス(RFC 4291)。ノードは自分が持つ各アドレスについて対応するグループに参加し、NDP のアドレス解決はブロードキャストの代わりにこのグループ宛てに送られる — 下位 24 ビットが一致するごく少数のノードしか受信せず、Ethernet 上でも 33:33 + 下位 32 ビットのマルチキャスト MAC(RFC 2464)により NIC の受信フィルタ段階で「全員を起こさない」が実現される。
- **初出章**: `04_ipv6/02_addressing.md`
- **関連RFC**: RFC 4291、RFC 2464
- **関連用語**: リンクローカルアドレス、ARP、IPv6、NDP、NS / NA、DAD

### ラージコミュニティ(large community)

- **定義**: 4 オクテット × 3(グローバル管理者 = AS 番号:値1:値2)で構成されるコミュニティ(RFC 8092)。32 ビット AS 番号(RFC 6793)が RFC 1997 の「上位 16 ビットに AS 番号」という慣例に収まらない問題への対応で、32 ビット AS 番号を持つ組織が名前空間付きの荷札を使うための形式。
- **初出章**: `03_bgp/04_policy_control.md`
- **関連RFC**: RFC 8092、RFC 6793
- **関連用語**: コミュニティ、拡張コミュニティ

### ラベルスタック(label stack)

- **定義**: MPLS ラベルを LIFO で複数積み重ねる仕組み(RFC 3032)。転送は常に先頭(最外)のラベルだけで行われ、S ビット = 1 が底を示す。役割の違うラベルの分業 — 出口 PE まで運ぶトンネルラベル+着後にテナント/サービスを仕分けるサービスラベル — が典型で、MPLS VPN の骨格。VXLAN の「外側 IP(VTEP まで運ぶ)+ VNI(仕分け)」と同型の2層構造である。
- **初出章**: `05_mpls_srv6/01_mpls_basics.md`
- **関連RFC**: RFC 3032
- **関連用語**: MPLS、LSP、VNI

### リーフ・スパイン(leaf-spine / Clos 網)

- **定義**: 全リーフが全スパインへ接続し、リーフ同士・スパイン同士は接続しない、データセンターの標準トポロジ。端末収容とカプセル化(VTEP)・EVPN への参加はリーフが担い、スパインはアンダーレイの中継専用でオーバーレイ(VNI・MAC)を関知しない。任意のリーフ間が等しく2ホップになり、スパイン本数分の ECMP が常に効く。
- **初出章**: `02_vlan_vxlan_evpn/05_evpn_vxlan.md`(言及は `02_vlan_vxlan_evpn/03_vxlan_fundamentals.md`)
- **関連用語**: VTEP、アンダーレイネットワーク、ECMP

### リンクステート(link state)

- **定義**: 動的ルーティングの方式の一つ。各ルータが自分の直結リンクの状態(一次情報)を全ルータへフラッディングし、全員が同一の地図(LSDB)を共有した上で、各自がダイクストラ法(SPF)で最短経路を計算する。伝搬と計算の分離により収束が速いが、メモリ・CPU と複雑さが代償。代表は OSPF と IS-IS。
- **初出章**: `01_fundamentals/04_distance_vector_link_state.md`
- **関連RFC**: RFC 2328(OSPFv2)、RFC 1195(IS-IS for IP)
- **関連用語**: ディスタンスベクタ、LSDB、SPF、フラッディング

### リンクローカルアドレス(link-local address)

- **定義**: fe80::/10(実用上は fe80::/64)から生成される、そのリンク上でのみ有効な IPv6 アドレス。IPv6 が有効なインタフェースは外部の設定なしに必ず1つ自己生成する。NDP・RA・ルーティングプロトコル(OSPFv3 のネイバー、BGP の IPv6 ネクストホップ併記 = RFC 2545)の土台であり、RA で配られるデフォルト経路のネクストホップも典型的にはリンクローカルである(fe80 のゲートウェイは正常)。ルータは転送せず、リンクごとに重複しうるためゾーン ID の指定を要する。
- **初出章**: `04_ipv6/02_addressing.md`(言及は `04_ipv6/01_why_ipv6.md` が先行)
- **関連RFC**: RFC 4291
- **関連用語**: ゾーン ID、インタフェース ID、GUA、デフォルトゲートウェイ、NDP、RS / RA

### 隣接(adjacency / アジャセンシー)

- **定義**: OSPF において、ネイバー(Hello で相互認識した相手)のうち LSDB の同期まで行う関係。Down→Init→2-Way→ExStart→Exchange→Loading→Full と遷移し、Full が同期完了。ブロードキャスト型ネットワークでは DR/BDR とだけ隣接を結び、DROther どうしは 2-Way で止まる(正常動作)。
- **初出章**: `01_fundamentals/05_igp_overview.md`
- **関連RFC**: RFC 2328
- **関連用語**: ネイバー、DR / BDR、LSDB、ピア

### ルータ ID(Router ID)

- **定義**: OSPF ルータを識別する 32 ビットの値。IPv4 アドレスと同じ表記だがアドレスではなく、到達可能である必要もない。実務では安定性のためループバックアドレスを流用する慣例がある。
- **初出章**: `01_fundamentals/05_igp_overview.md`
- **関連RFC**: RFC 2328
- **関連用語**: OSPF、隣接

### ルートタイプ(EVPN route type)

- **定義**: EVPN が MP-BGP で運ぶ情報の型。RT-1(Ethernet Auto-Discovery、冗長化)、RT-2(MAC/IP Advertisement、MAC の所在)、RT-3(Inclusive Multicast Ethernet Tag、VNI メンバーシップ)、RT-4(Ethernet Segment、冗長グループの発見と DF 選出)、RT-5(IP Prefix、MAC を伴わない L3 経路。RFC 9136)。
- **初出章**: `02_vlan_vxlan_evpn/04_vxlan_control_plane.md`(RT-2/RT-3)、全体像は `02_vlan_vxlan_evpn/05_evpn_vxlan.md`
- **関連RFC**: RFC 7432、RFC 9136
- **関連用語**: EVPN、RT
- **表記**: 「RT-数字」と略記する(単独の「RT」はルートターゲットを指す)

### ルートリフレクタ(route reflector / RR)

- **定義**: iBGP の再広告禁止規則に例外を開け、iBGP から学んだ経路を iBGP へ再広告(反射)できる指定ルータ(RFC 4456)。反射してもらうルータをクライアント、RR と配下のクライアントの集合をクラスタと呼ぶ。ループは ORIGINATOR_ID / CLUSTER_LIST で検出する。クライアント側は無改造でよく漸進導入が容易なため、フルメッシュ緩和の実務の主流。反射では NEXT_HOP 等の属性を変更しないため、RR 自身はデータパス上にいなくてよい(EVPN ファブリックのスパイン RR が典型)。代償として、RR が選んだ最良1本しか配らないことによる経路多様性の喪失と出口選択の歪み(パスハイディング)がある(緩和は ADD-PATH、RFC 7911)。
- **初出章**: `03_bgp/06_large_scale_design.md`(言及は `03_bgp/02_ibgp_ebgp.md`・第2部にもあり)
- **関連RFC**: RFC 4456
- **関連用語**: フルメッシュ、ORIGINATOR_ID / CLUSTER_LIST、コンフェデレーション、eBGP / iBGP

### ロンゲストマッチ(longest prefix match)

- **定義**: ルーティングテーブル検索で、宛先アドレスに一致するプレフィックスのうち最も長い(具体的な)ものを選ぶ規則。
- **初出章**: `01_fundamentals/01_l2_l3_recap.md`
- **関連用語**: ホップバイホップ転送
