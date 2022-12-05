era4X用建艦&装備更新時色々表示パッチver2.2

◇パッチの目的
色々と欲しかった表示を追加する為のパッチです。
艦船購入と装備更新の画面で(資材不足時ではなくとも)常に必要な資材が表示される様にするのと、装備更新時に装備の値段と武器の属性を表示する様にします。
ただし「テンプレートで購入」に限っては適用して購入する時に必要資材(と資材不足時の不足量)がずらっと・次々に表示される形式です。
(処理的には「テンプレートの内容に従って次々に買って行く処理」の様なので先に必要資材の総量を表示するとなると「購入処理以外を全て先に一回やって表示させる」などとしなければ実現できないと思います)

沢山の(ほぼ)同じ処理が別々にあったのを踏襲して「処理を作る時にその内の一か所で試しては他の全ても同じ内容に書き換える」を繰り返すのは辛そうだったので
いくつかの処理を集約して一か所だけ書き換えれば済む様にもしたかったですが、装備の方は処理速度が落ちる様だったので完全な集約は断念しました。



◇具体的な変更内容
艦船購入で艦船を選んだ際に建艦に必要な資材が表示される様に処理を追加・変更。(SHIP.ERB・USE_RESOURCE_CHECK.ERB)
装備更新で装備を選んだ際に更新に必要な資材が表示される様に処理を追加・変更。(EQUIP_UPDATE.ERB・USE_RESOURCE_CHECK.ERB・CHANGE_EQUIP.ERB)
	それに伴って資材の不足数の表示回りも変更。

装備更新で装備を選んだ際に装備更新時に装備の値段と武器の属性を表示する様に処理を追加(CHANGE_EQUIP.ERB)
新旧装備の性能比較を表示する様に処理を追加(CHANGE_EQUIP.ERB)
武器の性能表示にDPT(Damage Per Turn)を追加(CHANGE_EQUIP.ERB・EQUIP_STATUS_DESCRIPTION.ERB)

@GET_(装備種名)_DESCRIPTION_(ID)の内、性能の表示を分離した残りを@GET_(装備種名)_DESCRIPTION_F_(ID名)に名称変更・元々の@GET_(装備種名)_DESCRIPTION_(ID)は廃止((装備名).ERB)
@GET_(装備種名)_DESCRIPTION_(ID)の性能表示部分を@EQUIP_STATUS_DESCRIPTIONへと分離(EQUIP_STATUS_DESCRIPTION.ERB)
	@GET_(装備種名)_DESCRIPTION_(ID)を呼んでいた処理は置き換え済み。
		基本的には@EQUIP_STATUS_DESCRIPTIONで置き換え
		変数を得ようとしていたTRYCCALLFORM GET_(装備種類名)_DESCRIPTION_{SELECTED}に関してはTRYCCALLFORM GET_(装備種類名)_STATUS_{SELECTED}(STATUS)で置き換え。

@USE_RESOURCE_CHECK(USE_RESOURCE_CHECK.ERB)として必要な処理を関数化・装備更新が(装備種名).ERB分散していたのをCHANGE_EQUIP.ERBとして集約しつつ、資材表示処理と購入処理周りも都合のいい様に変更
	他勢力AI(AI_COUNTRY_(数値).ERB)に@USE_RESOURCE_CHECKを使用させるとターン経過時の処理がだいぶ重くなりがちだったので置き換えは断念

@CHANGE_(装備種名) はNPCの処理からしか呼び出されない様になったのでプレイヤーが呼び出した場合の処理を消して処理速度向上を狙う(効果があるかは不明)
	他勢力AIに@USE_RESOURCE_CHECKを使わせるのを断念したのでプレイヤー以外が呼び出した時の事を想定した処理を消して処理速度向上を狙う(効果があるかは不明)

各艦船の@GET_SHIP_DESCRIPTION_(ID名)の処理をSHIP.ERBでやる様に変更。
	@GET_SHIP_DESCRIPTION_(ID名)はSLG_SHOP.ERBのTRYCALLFORM GET_SHIP_DESCRIPTION_{SELECTED}からのみ呼ばれていたっぽい。
	SLG_SHOP.ERBのTRYCALLFORM GET_SHIP_DESCRIPTION_{SELECTED}をコメントアウト。
(全艦船で共通の表示処理はSHIP.ERBでやらせる事で共通の表示処理を変更するのに各艦船のデータを個別にいじる必要があるのを無くして、表示に関しては行数まで変わる≒共通処理にするのが面倒臭そうな個別の説明文だけ各艦船のERBでやらせる様にしたい)



◇備考
(装備種名).ERBは入れなくても機能します。

@GET_SHIP_DESCRIPTION_(ID名)を消した艦船データが入っていますが、これも入れなくても機能します。
(装備データ共々Grapで一括処理したので抜けがあるかもしれません)


◇履歴
2022/11/16版
建艦時の必要資材の表示機能のみで初公開

2022/11/19版
資材確認のミス(たぶんプレイヤー以外が呼んだ時でも一部の資材確認の表示が出る)を修正
他勢力の建艦時にも艦船の性能・説明が表示されたミスを修正
必要資材確認と表示をUSE_RESOURCE_CHECK.ERBの「@USE_RESOURCE_CHECK」として関数化
@BUILD_SHIP・@BUILD_ORIGIN_SHIPの資材関係の処理をUSE_RESOURCE_CHECKで置き換え
EQUIP_UPDATE.ERBにおける各種装備更新が装備種ごとに別ERB・別関数で実行されていたのををCHANGE_EQUIP.ERBの@CHANGE_EQIPに統合
	CHANGE_EQUIP.ERBは装備更新関数を統合するのが目的であるものの、他勢力AI(AI_COUNTRY_(数値).ERB)の処理は旧来の処理のまま
装備更新時に装備の値段・武器の属性の表示を追加

2022/11/25版
装備更新時の性能表示を@SPEC_DISPLAYとして分離
新旧装備の性能比較を追加(@SPEC_DISPLAY)
艦船購入の性能表示を@SHIP_STATUS_DISPLAYとして分離
艦船購入の性能表示に積載量とオプションスロットの項目を追加(@SHIP_STATUS_DISPLAY)
武器の性能にDPTの表示を追加
必要な資材・価格の隣に利用できる資材・所持金を表示する様に変更
自分の港で装備更新する時には必要価格・所持金を灰色に(無料で出来るので)
装備のデータ表示をEQUIP_STATUS_DESCRIPTION.ERBの@EQUIP_STATUS_DESCRIPTIONとして分離
バグ修正：装甲以外の装備更新時にプレイヤー以外の勢力は問答無用で金が取られていたのを「その勢力から見て他勢力の港で更新した場合のみ」(プレイヤーと同じ条件)へ修正(装備種名.ERB)

2022/11/25版(2.1)
文字色を戻す命令を欠けさせてしまっていた艦船データを修正

2022/11/25版(2.2)
@USE_RESOURCE_CHECKの文章非表示化をSKIPDISPに変更
MAIN_WEAPON_204_凝縮式速射波動砲の対空攻撃力の表示(削除漏れ)を削除
テンプレートで購入する際に@CHANGE_EQIPが購入確認をしない様に修正