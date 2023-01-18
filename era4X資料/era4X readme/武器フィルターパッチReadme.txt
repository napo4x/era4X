era4X用武器フィルターパッチ

◇パッチの目的
使用しうる武器が増えれば増えた分だけ肥大化していく武器一覧から目当ての武器を見つけやすくしたい。
その為に属性・有効距離・サイズをそれぞれ絞り込める様にしたい。

◇具体的な変更内容
フィルタ設定関数@WEAPON_FILTER_CONFIGとフィルタ判定関数@WEAPON_FILTER_CHECKを追加
@EQUIP_UPDATE_0および@EQUIP_UPDATE_1にフィルタ設定用変数WEAPON_FILTERを追加
@EQUIP_UPDATE_0および@EQUIP_UPDATE_1の武器一覧を表示する際の処理で@WEAPON_FILTER_CHECKを呼んでSTATUSとWEAPON_FILTERでフィルターに合致するかどうかを確認して合致しない場合には非表示に。
同武器一覧の末尾の[戻る]の手前に[フィルタ設定]ボタンを設置。フィルタ設定を選んだ場合には@WEAPON_FILTER_CONFIGを呼ぶ様に。
(以上全てEQUIP_UPDATE.ERB)
武器の持ちうるそれぞれのパラメータの数を指定する定数「範囲_有効距離」と「範囲_サイズ」を追加(EQUIP.ERH)