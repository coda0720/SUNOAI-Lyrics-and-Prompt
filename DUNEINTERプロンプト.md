# DUNE と Interstellar のスコア比較分析と SUNO 実装用プロンプト設計

## エグゼクティブサマリ

Hans Zimmer の **Interstellar** は、父娘関係というきわめて人間的な核から出発し、**パイプオルガン、極端に単純化された和声循環、3/4系の反復、長いブレスを伴う合唱**で「宇宙の巨大さ」と「呼吸する身体性」を同時に作るスコアです。Zimmer 自身はこのスコアを「三つのコード」に収斂するほど単純な和声で設計したと振り返り、Temple Church のオルガンを選んだ理由も「空気で鳴る、呼吸する機械」という点に置いています。さらに公開されたスコア断片では **♩=98**、合唱に **“chorisch atmen”** と明記されており、テンポの厳密さより**呼吸とフレージングの共同体感**が重要であることが読み取れます。 citeturn22view2turn29view0turn16view3turn14search2

これに対して **Dune: Part One / Part Two** は、メロディより**音色・質感・儀式性**を前面に出す設計です。Part One では Zimmer が「女性の声の強さ」と「スコア全体を貫く精神性」を重視し、Loire Cotler の声を中心に、**女声の叫び、変形されたチェロ、電子的に“人間離れ”させたドラム、共鳴体、風系の民族楽器、ギターやバグパイプ的音色**で世界を作っています。Part Two ではこの言語を継承しつつ、**Paul と Chani のラヴテーマ**が前景化し、公式系アレンジでは **“Freely, q=70”** と記されるなど、**定拍よりストレッチされた息遣い**が強くなります。 citeturn23view1turn23view0turn24view0turn31view0turn34search3turn34search15

実践的に言えば、**Interstellar を土台**にして **Dune の音色・叫び・低域・儀式感**を注入するのが、SUNO 用インスト・プロンプトとして最も再現性が高いです。設計の骨格は、**小さな arpeggio の導入 → 持続ドローン上の儀式的コール → 低域と合唱の巨大クレッシェンド → 余韻だけを残して解放**、という四段アークにするのが有効です。 citeturn22view2turn10view2turn23view1turn31view0turn16view1

## 調査範囲と前提

今回の対象は、ユーザー指定どおり **Hans Zimmer の Interstellar** を優先しつつ、**Dune: Part One の公式サウンドトラック、The Dune Sketchbook、Dune: Part Two の公式サウンドトラック**まで確認しています。Apple Music 上でも **Interstellar Expanded Edition は 30 曲・2 時間 19 分**、**Dune Part One は 2021 年の公式 OST**、**The Dune Sketchbook は 9 曲・1 時間 41 分**、**Dune: Part Two は 25 曲・1 時間 21 分**として確認でき、Hans Zimmer 公式サイトでも Interstellar および Dune: Part Two のクレジットとキュー名が確認できます。 citeturn7search0turn7search1turn7search2turn7search4turn16view3turn18view0

出典の優先順位は、依頼どおり **公式サウンドトラック／公式クレジット／作曲者インタビュー／楽譜・スコア断片／学術資料／信頼できる音楽制作サイト**を中心に置きました。とくに Interstellar では Hans Zimmer 公式サイト、Classic FM の Zimmer 直接談話、Temple Church 関係者情報、公開スコア断片を重視し、Dune では Hans Zimmer 公式サイト、TheWrap、Vanity Fair、EW、Spitfire、Musicnotes の公式ライセンス譜面情報を中心に整理しています。 citeturn16view3turn22view2turn23view1turn23view0turn16view0turn13search7turn31view0turn31view2

一方で、**Dune: Part One の合唱実人数、Dune/Interstellar の個別ミックスで使用された具体的なプラグイン名、プリディレイ値、IR 名称、フォルマント処理量**は、今回確認した一次資料では公開されていません。これらは本稿では **未指定** とし、必要箇所では **再現用の推奨設定**として分けて記述します。公式クレジットで判明するのは、スタッフ名・録音場所・主な演奏者・一部の楽器設計／追加音楽担当までです。 citeturn16view3turn24view0

## 比較分析

### 全体比較表

| 観点 | Dune Part One / Part Two | Interstellar |
|---|---|---|
| 確認した作品 | Part One 公式 OST、The Dune Sketchbook、Part Two 公式 OST を確認。Part Two は Hans Zimmer 公式サイトに詳細クレジットあり。 citeturn7search1turn7search2turn7search4turn18view0 | Expanded Edition と Hans Zimmer 公式サイトのブックレット・クレジットを確認。 citeturn7search0turn16view3 |
| 感情の中核 | 儀式、予言、女性性、霊性、異世界性。Part Two では Paul/Chani のラヴテーマが強化。 citeturn23view1turn23view0turn34search15turn34search19 | 父娘関係、家、距離、時間、呼吸。宇宙映画でありながら出発点は親子関係。 citeturn22view2turn21search18 |
| モチーフ設計 | **音色主導**。Part One は Cotler の**マイナー・キーの女声モチーフ**がチェロへ変奏される。Part Two はその語法の上にラヴテーマを追加。 citeturn11view3turn11view4turn34search3 | **和声主導＋短い反復細胞**。Zimmer は「三つのコードに収まる」と語り、公開譜面では G6–Fmaj7–Am–E を含む循環が確認できる。 citeturn22view2turn10view2 |
| 和声／モード | Part One の “Dune Main Theme” と “Herald of the Change” は公式ライセンス譜で **D minor**。機能和声より、**ドローン上でのモード感・上部クラスター移動**が主体。Part Two の “A Time of Quiet Between the Storms” は **D major** 系アレンジ情報がある。 citeturn31view2turn32search5turn31view0 | A minor / C major 圏の曖昧さが強く、G6・Fmaj7・Am・E などで**明暗の揺れ**を作る。New Yorker も major/minor の揺らぎを指摘。 citeturn10view2turn14news33 |
| リズム／テンポ感 | Part One 主題は **約 50〜75 BPM** の遅い脈動が多く、Part Two のラヴテーマは **Freely, q=70**。拍より**息・間・重心移動**が重要。 citeturn32search5turn31view2turn31view0 | “Day One” の公開譜面は **3/4、♩=98**。Musicnotes 系でも約 96 BPM。だが合唱には “chorisch atmen” が付くため、厳密クリックより**呼吸で揺れる体感**が重要。 citeturn29view0turn27search1 |
| 代表的編成 | Part Two 公式クレジットでは **world winds / duduks / contrabass flute / electric guitar / electric & double bass / electric cello / violin/viola / exotic instruments / bagpipes / percussion / featured vocalist Loire Cotler / named vocalists / synth design / resonators**。Part One も女性声・変形チェロ・電子ドラム・共鳴体が核。 citeturn24view0turn23view1 | **Temple Church organ、34 strings、24 woodwinds、4 pianos、60-voice mixed choir**。録音は Temple Church と AIR Lyndhurst。 citeturn16view3turn14search2turn14search14 |
| コーラスの質感 | Part One は「女性の声の強さ」と精神性が中心。Part Two は**大人数合唱というより、リード＋複数名ボーカリスト＋重ね録り推定**が実践的。公式の総人数は未指定。 citeturn23view1turn23view0turn24view0 | 明確に **mixed choir 60 voices**。低ダイナミクスで長いサステインを維持し、オルガンと一体化して“空間の空気”になる。 citeturn14search2turn29view0 |
| ミックス／空間 | Alan Meyerson の Dune 解説では **quadraphonic sound、weighted panning、stereo width、phase、bass、choir、organ、Haas effect、low-end harmonics**が主論点。公式の具体リバーブ値は未公開。 citeturn16view1 | Temple Church の実空間と AIR の録音が核。Zimmer は Temple Church を**音響と静寂**で選んだと述べる。後段のミックスは Alan Meyerson。 citeturn22view2turn16view3 |

### 代表キューの構造テンプレ

**Interstellar の実戦テンプレ**は、公開スコア断片から見るとかなり明快です。  
“Day One” は、**A: ピアノの arpeggio 導入**、**B: フルート対旋律の追加**、**C: ppp の合唱持続音が “chorisch atmen” で入る層拡張**、**D: ストリング／オルガン方向への拡大**、という積み上げ型です。しかも譜面上は 3/4・♩=98 でも、合唱の入り方が「テンポを押し進める」のではなく「息で面積を増やす」方向なので、打ち込み時には**グリッド精度よりアタックの遅れと release の長さ**が効きます。 citeturn29view0turn29view1

“Stay” と “Detach” は、同一ライトモティーフを**テンポ、ダイナミクス、ピッチ／モード感、文脈**で変形する例として学びやすいです。Interstellar らしさは「新しい主題を増やす」ことより、**同じ核を別のドラマ状態へ変換する**ことにあります。したがって SUNO でも、セクションごとに別曲化するより、**同じ和声セルとリズム細胞を、編成と密度だけ変える**ほうが近づきます。 citeturn12view1turn11view2

**Dune Part One のテンプレ**は、より**儀式的・音色的**です。  
“Herald of the Change” は公式ライセンス譜情報で **D minor・約 50 BPM**、主題レベルでは低速の宣告感が強く、Part One の “Dune Main Theme” も **D minor・約 75 BPM**。TheWrap で Zimmer 自身が語るように、女性声、変形チェロ、電子ドラム、共鳴体によって**西洋オーケストラ語法を避けた“異物感”**が狙われています。構造的には、**低域ドローン → ソロ女性声／叫び → 打楽器ないし低域パルス挿入 → 音色密度の爆発 → 余韻 cut** がプロトタイプです。 citeturn32search5turn31view2turn23view1

**Dune Part Two のテンプレ**は二系統あります。  
ひとつは “A Time of Quiet Between the Storms” のような**自由テンポの親密系**、もうひとつは “Kiss the Ring” や戦闘キューに見られる**巨大な低域・多層ボーカル・儀式コール型**です。Zimmer 自身は Part Two のメインテーマをツアーで先行演奏していたと述べ、Spitfire と Wallpaper の取材でも、Paul/Chani のラヴテーマがかなり早い段階から核だったことが分かります。したがって Part Two を混ぜるなら、**Part One の荒々しい異世界性に、Part Two の“息の長い親密さ”を挿す**のが最も効果的です。 citeturn34search19turn34search3turn34search15turn31view0

## テンポと MIDI 実装

### テンポに正解がない表現はあるか

あります。しかも **Dune と Interstellar で性格が違います。**  
Dune: Part Two の “A Time of Quiet Between the Storms” は、公式ライセンス・アレンジ情報で **Tempo: Freely / Metronome: q=70** と明記されています。これは「70 BPM ぴったりで刻め」という意味ではなく、**70 を重心にしながら呼吸で伸縮させる**設計として読むのが実践的です。 citeturn31view0

Interstellar の “Day One” は逆に、譜面上は **♩=98** と明確でありながら、合唱に **“chorisch atmen”**、ダイナミクスは **ppp**、そして Zimmer 自身はこのスコアを「三つのコード」による大きな帰還／喪失の循環として説明しています。つまり、拍はあるが、**感情の時間はブレスで決まる**タイプです。クリックを消す必要はありませんが、**拍の上で全員が同時に入ると、Interstellar らしさが消える**と考えた方がよいです。 citeturn29view0turn22view2

### 呼吸やフレージングによるテンポ揺らぎの作り方

制作上は、**グローバルテンポを 1 本、演奏ゆらぎを 2 本**に分けると再現しやすいです。  
一本目は **DAW のテンポマップ**。二本目は **MIDI ノート開始位置の微遅延**。三本目は **CC11 / Expression と release 調整**です。Interstellar 系はテンポマップをあまり大きく振らず、**入る音だけ 10〜30 ms 遅らせる**方が効きます。Dune 系は逆に、**セクション丸ごと 2〜6 BPM 伸縮**させたほうが“儀式が息を吸う”感じになります。これらは、Interstellar の “chorisch atmen”、Dune の “Freely”、そして Zimmer の「agogic tempo variation（アゴーギクなテンポ変形）」という作風説明と整合します。 citeturn29view0turn31view0turn12view3

### MIDI テンポマップ例

| 用途 | 小節例 | テンポ設計 | MIDI ノート実装 | ねらい |
|---|---|---|---|---|
| Interstellar 型の導入 | 1–8 | 3/4、96 BPM 固定 | ピアノ arpeggio はグリッド通り、合唱は 0〜12 ms 遅れ | 脈は一定、呼吸だけ遅れる |
| Interstellar 型の拡大 | 9–12 | 96 → 98 BPM を 4 小節で微上昇 | 合唱 release を 180〜260 ms、次音を 15〜25 ms 遅着 | “吸って膨らむ”感じ |
| Interstellar 型の句読点 | 13–16 | 98 → 92 BPM の短い rit.、次セクション頭で 96 に復帰 | 最終和音の gate を 88〜92% に短縮、無音を 120 ms 作る | 大きな空間で息継ぎする感覚 |
| Dune 型の親密部 | 1–4 | 4/4 または 2/2、70 BPM 基準、Freely | ソロ声は拍頭ちょうど、ドローンは前ノリ禁止 | 静止感を維持 |
| Dune 型の儀式化 | 5–8 | 70 → 66 → 72 BPM | 叫び doubles を ±9 cent、開始を 12〜35 ms ばらす | 人間と非人間の中間 |
| Dune 型の巨大化 | 9–16 | 72 → 74 → 68 BPM、最後はテンポよりサステインで伸ばす | 低打楽器をジャスト、上モノ声を遅らせる | 土台は機械、上層は生物 |

この表は一次資料のテンポ指定をそのまま写したものではなく、**Interstellar の “♩=98 + chorisch atmen”** と **Dune の “Freely q=70”** を DAW に落とすための再現用テンプレです。制作現場では、どちらも「テンポオートメーションだけ」で再現しようとせず、**ノートタイミングとExpression曲線をセット**で動かしてください。 citeturn29view0turn31view0turn12view3

### DAW での具体手順

| DAW | 手順 | 実践メモ |
|---|---|---|
| Ableton Live | Arrangement View で **Master track を展開**し、上段で **Mixer**、下段で **Song Tempo** を選んでテンポエンベロープを描く。leader clip 由来のテンポ追従は **Unfollow Tempo Automation** で編集可能にする。 citeturn36search1turn36search5 | Interstellar 型では急峻な階段より**長いランプ**、Dune 型では**セクション境界だけ折れ点**を置くと自然です。 |
| Logic Pro | **テンポリスト**で全テンポチェンジを管理し、新規テンポイベントを作成・編集する。必要に応じて **Smart Tempo** 解析や**プロジェクトテンポモード**を使う。 citeturn37search0turn36search6turn36search10 | ライブ録りの合唱やソロ vocal から逆算してテンポマップを立てる時に有効です。 |
| Cubase | **Project > Tempo Track > Open Tempo Track Editor** もしくは **Ctrl/Cmd + T** でテンポトラックを開き、テンポイベントを追加・編集する。 citeturn35search10 | “Freely” を作るなら、点を増やしすぎず**4〜8 小節単位の曲線**でまとめると劇伴らしくなります。 |

#### 参考用 MIDI スニペット

```text
Interstellar型 3/4
Bar 1-4: Piano arp only
  RH: 8分 arpeggio を均等
  LH: 和音はレガート保持
Bar 5-8: Choir in
  S/A: 全音符または付点2分の持続
  T/B: ルート or 5度を薄く
Timing:
  Choir start +18 ms
  Organ start +8 ms
CC11:
  42 -> 58 -> 74 over 8 bars
```

```text
Dune型 4/4 freely
Bar 1-2: low drone only
Bar 3-4: solo female cry
Bar 5-8: sub pulse + overdubbed doubles
Timing:
  Lead cry 0 ms
  Double L +22 ms / Double R +31 ms
Pitch:
  Double L -7 cent / Double R +9 cent
CC11:
  35 -> 92 in one long arc
```

## オーケストレーションとコーラス設計

### オーケストレーションの違い

**Interstellar** は、音数の多さではなく、**オルガンを中心に据えた持続・反復・層の増築**が本質です。公式クレジットでは Roger Sayer の Temple Church organ、34 strings、24 woodwinds、4 pianos、mixed choir 60 voices が示され、AIR Lyndhurst と Temple Church が録音場所です。したがって再現時は、フルオーケストラを全部鳴らすより、**オルガン／ピアノ・アルペジオ／弦パッド／木管の補助線／合唱持続**に絞る方が近づきます。 citeturn16view3turn14search2turn14search14

**Dune** は逆に、編成名より**加工された存在感**が重要です。Part Two の公式クレジットでは、Pedro Eustache の world winds / duduks / contrabass flute、Guthrie Govan の electric guitar、Tina Guo らの electric cello、Chas Smith の exotic instruments、bagpipes、複数の打楽器奏者、Loire Cotler をはじめとする vocalists、さらに synth design・resonators まで明記されており、Meyerson の解説でも stereo width、phase、bass、choir、organ、Haas effect、low-end harmonics が主要論点です。再現時は、**通常オーケストラの代用**ではなく、**楽器 + 処理 + 層構造**で考えるべきです。 citeturn24view0turn16view1

### コーラスの人数感と配置

Interstellar の場合、一次資料で **60-voice mixed choir** が確認できるため、SUNO や DAW 再現でも「少人数の女声パッド」では不足します。**中央奥にオルガン、左右に広がる合唱の early reflection、前景に細いピアノ**という絵作りが妥当です。母音は **mm / oo / ah** を中心にし、子音は少なくした方が organ と溶けます。 citeturn14search2turn16view3turn29view0

Dune は、Part One で Zimmer 自身が「映画は女性の声の強さに担われるべきで、スコア全体に精神性が通っているべきだった」と語っています。Part Two の公式クレジットでは **featured vocalist 1 名 + 複数の named vocalists** が確認できる一方、Interstellar のような 60 人規模は公開確認できません。したがって再現は、**mass choir** より **lead cry 1 本 + hard double 2〜4 本 + breath layer 2 本 + distant ritual pad 4〜8 本**という多層オーバーダブ型が近いです。 citeturn23view1turn23view0turn24view0

### コーラスのレイヤリングとボイシング例

**Interstellar 型ボイシング例**  
低声部は **root / fifth** を長く保持し、中声部は **3rd / 6th** を狭く、上声部は和音頭頂ではなく**空気の帯**として置くのが有効です。和音自体を歌わせるより、**持続音の面を重ねて organ の倍音に編み込む**感覚です。公開譜でも ppp と長大な持続が目立ちます。 citeturn29view0

**Dune 型ボイシング例**  
lead は **短い叫び／上昇 appoggiatura／持続／微下降**、doubles は 1 オクターブ上か完全 5 度下ではなく、**ユニゾン近傍を微分音的にずらす**方が“砂の揺れ”になります。理想は、  
- Lead: 0 cent  
- Double L: -7 ～ -12 cent  
- Double R: +7 ～ +12 cent  
- Air layer: 1 オクターブ上を薄く  
- Whisper layer: 子音多めで 2〜4 kHz を補う  
です。Part One の「minor key female motif を cello に変奏」という説明とも整合します。 citeturn11view3turn23view1

### 音響処理と空間表現

ここは **公式に分かっていること**と**再現のための推奨**を分ける必要があります。  
分かっていることとして、Interstellar は Temple Church と AIR で録音され、Temple Church は**音響と静寂のために選ばれた**こと、Dune Part Two は Alan Meyerson がミックスし、quadraphonic / weighted panning / low-end harmonics / Haas effect が解説テーマになっていることまでです。**具体的なプリディレイ値、IR 名、サチュレーター設定、EQ カーブは未指定**です。 citeturn22view2turn16view3turn16view1

再現用の推奨としては、**Interstellar** は  
- リバーブ: church または large scoring stage  
- decay: 4.5〜7 s  
- pre-delay: 20〜40 ms  
- 低域: return 側を 180〜250 Hz で整理  
- stereo: 原音はやや中央寄り、反射だけ広く  
- saturation: 極薄の tape/tube 程度  
が有効です。  
**Dune** は  
- lead voice を比較的ドライに保ち  
- 別送で非常に長い tail  
- pre-delay: 40〜80 ms  
- slap / short delay を薄く追加  
- low-end は 30〜60 Hz をセンター固定  
- widening は 2 kHz 以上の空気層のみ  
- scream doubles に formant shift と軽い drive  
が効きます。フォーム感変形は、MAutoPitch のような無料ツールでも可能で、Little AlterBoy のマニュアルでも**背景声に formant を -1.0〜-3.0 程度下げると深みが出る**と案内されています。 citeturn41search1turn41search14turn39search0turn39search1

#### 再現用スペクトラムとステレオの目安

```text
周波数バランスの目安
Interstellar
30-60Hz   ▂▂     80-250Hz  ▇▇▇   250-800Hz ▇▇
2-5kHz    ▅       8-12kHz   ▅
= 低域は“深いが暴れない”、中低域に organ/body、上は柔らかい

Dune
30-60Hz   ▇▇▇▇   80-250Hz  ▇▇     250-800Hz ▂~▅
2-5kHz    ▇▇▇     8-12kHz   ▇
= sub と scream の両端を強調し、中域を場面ごとにえぐる
```

```text
ステレオ配置の目安
Interstellar
[L choir reflections]   [C organ / piano / bass]   [R choir reflections]

Dune
[L overdub cry / FX]    [C sub / lead invocation]  [R winds / FX / doubles]
```

## SUNO 用インストロメンタル・プロンプト

以下の 3 案は、**固有名そのものを真似させる命令ではなく**、分析から抽出した**楽器・和声・構造・空間・ダイナミクス**をそのまま SUNO に与えるための記述です。Interstellar の「呼吸するオルガンと三和音循環」、Dune の「女声儀式・変形楽器・低域の砂嵐」を混ぜています。 citeturn22view2turn24view0turn23view1

### 短い案

**プロンプト**  
宇宙的で神聖なインストゥルメンタル。ゆっくりした 3/4 または自由拍、呼吸で揺れるテンポ。細いピアノのアルペジオ、巨大なパイプオルガン、低いドローン、遠い混声合唱、時々入る儀式的な女性の叫び声。和声は単純で反復的、明るさと暗さが揺れる三和音中心。ドラムは最小限、キックよりも低域の脈動。長い教会的残響、広大な空間、映画的クレッシェンド、最後は余韻を残して静かに解放。歌詞なし、完全インスト、壮大で内省的。

**期待される出力**  
2 分前後。主役は **piano / organ / choir / low drone**。テンポ体感は **70〜98 BPM 相当**だが、クリックよりフレーズ感が前に出るはずです。ダイナミクスは **ppp から一度だけ大きく膨らむ**構造、コーラスは **远景の混声パッド + 単発の女性コール**が出やすい設計です。 citeturn29view0turn31view0turn14search2

### 中くらいの案

**プロンプト**  
壮大な SF 映画のためのインストゥルメンタル。構成は「親密な導入 → 砂漠の儀式 → 巨大な覚醒 → 残響だけ残る終結」。冒頭は 3/4 のピアノアルペジオと柔らかなオルガン、低音は控えめ。中盤で遠い女性ボイスの呼び声、民族木管、変形チェロ、電子的な低域パルス、砂嵐のようなテクスチャを追加。終盤は混声合唱とオルガンが大きく広がり、しかし和声は単純な反復のまま。テンポは厳密に固定せず、息を吸うように少し遅くなり、句読点でわずかに戻る。極端に映画的、神秘的、宗教的、でもメロドラマではなく静かな威圧感。歌詞なし、インストのみ。

**期待される出力**  
3〜4 分。楽器構成は **piano / pipe organ / mixed choir / female solo vox texture / duduk-like winds / electric cello / low pulse percussion / synth drone** が理想形です。テンポ感は **冒頭安定 → 中盤自由化 → 終盤クレッシェンド**。ダイナミクスは **中盤まで溜めて後半で開く**タイプ。コーラスは **mass choir より air + ritual** と **cathedral pad** の二層で出ると成功です。 citeturn24view0turn23view1turn16view1turn22view2

### 長い案

**プロンプト**  
超大作 SF スコア風の長尺インストゥルメンタル。人間的な親密さと異世界の儀式性を同時に持つ。最初は小さなピアノとオルガンだけで始まり、3/4 の静かな反復、柔らかな混声ハミング、呼吸で揺れるフレージング。次に砂漠の予言のような女性ボイス、民族木管、共鳴する金属、変形チェロ、バグパイプ風の持続音、深いサブベースが少しずつ加わる。和声はシンプルで、同じ数個のコードを長く回し、モード感が揺れる。後半は巨大な混声合唱、パイプオルガン、低域打撃、空間を引き裂く女性の高い叫び、広いステレオ、長い教会／巨大ホール系リバーブ。中低域は濃く、低域はセンター、上モノだけ広い。ラストは勝利ではなく、畏怖と余韻で終わる。歌なし、セリフなし、完全インスト、アート系ではなく映画本編で使える密度。

**期待される出力**  
4〜6 分。**Interstellar 的な小さな核**から始まり、**Dune 的な質感の増殖**へ向かう構造が最も期待されます。テンポは **60〜100 BPM のどこかに重心があるが、局所的に“正解のない遅れ”が出る**のが望ましいです。ダイナミクスは **非常に長いクレッシェンド**、コーラス処理は **近接の lead cry をややドライ、背後の mass pad は長い tail** という形が理想です。 citeturn31view0turn29view0turn23view1turn24view0turn14search2

## 実践用のセットアップとプラグイン

### 推奨プラグインと開始設定

| 目的 | 無料候補 | 有料候補 | 開始設定の目安 |
|---|---|---|---|
| 巨大な残響 | Valhalla Supermassive、MConvolutionEZ citeturn38search0turn39search15 | Valhalla VintageVerb、Seventh Heaven、Altiverb citeturn40search9turn40search1turn40search2 | Interstellar: 4.5〜7 s、pre-delay 20〜40 ms。Dune: 5〜9 s、pre-delay 40〜80 ms。 |
| ステレオ幅の管理 | Ozone Imager、SPAN の correlation meter citeturn39search0turn39search1 | — | **120 Hz 以下は mono**、広げるのは choir air / FX 層だけ。 |
| スペクトラム監視 | SPAN citeturn39search1 | — | Dune では 30〜60 Hz を見すぎ、Interstellar では 100〜300 Hz 濁りを監視。 |
| 動的 EQ | TDR Nova citeturn39search2turn39search14 | FabFilter Pro-Q 系統、Saturn 2 の multiband 併用 citeturn40search0turn40search14 | 女声の 2.5〜5 kHz を暴れた瞬間だけ 1〜3 dB 抑制。 |
| サチュレーション | — | FabFilter Saturn 2、Little AlterBoy の drive citeturn40search0turn41search2 | Dune は parallel で 5〜15% 程度、Interstellar は 0〜5% の薄い warm 化。 |
| フォルマント変形 | MAutoPitch、Graillon citeturn41search1turn41search0 | Little AlterBoy citeturn41search2turn41search14 | doubles を -1.0 〜 -3.0 で少し下げると深さが出やすい。 |
| コンボリューション IR | MConvolutionEZ citeturn39search15 | Altiverb、Seventh Heaven citeturn40search2turn40search1 | Interstellar は church / scoring stage、Dune は hall / chamber + 別送 long tail の二段構え。 |

### 実装の優先順位

再現で重要なのは、**プラグインを増やすことではなく、順番を守ること**です。  
最初に **和声セルとセクション構造**、次に **テンポマップ**、その後に **レイヤー編成**、最後に **空間処理と帯域整理**を行ってください。Dune 系の失敗は、最初から歪みと巨大リバーブをかけて「ただのノイズ壁」になることです。Interstellar 系の失敗は、テンポを完全グリッド化して「壮大なだけのループ」にしてしまうことです。一次資料でも、Interstellar は呼吸、Dune は女性声・音色・低域処理が核だと読めます。 citeturn29view0turn23view1turn16view1

実践では、**Interstellar 側を“和声・呼吸・オルガン”の参照、Dune 側を“音色・叫び・低域・儀式”の参照**と分担させるのが最も安定します。もし 1 曲の中で両方を混ぜるなら、前半は Interstellar 寄りにして、後半で Dune のテクスチャを増やす方が自然です。最初から Dune の密度で始めると、後半の伸びしろがなくなります。 citeturn22view2turn23view1turn34search15

### 最終的な制作指針

最終的な一文にすると、**「Interstellar の単純和声と呼吸するオルガンに、Dune の女声儀式と加工低域を流し込み、テンポはクリックで固定せず phrase で揺らす」**がこの比較研究の結論です。これがそのまま、SUNO 用の最適なインスト・プロンプト設計にも、DAW / MIDI の実装指針にもなります。 citeturn22view2turn29view0turn23view1turn24view0
