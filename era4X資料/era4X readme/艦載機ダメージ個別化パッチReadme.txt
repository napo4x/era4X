艦載機ダメージ個別化パッチver1.1Readme

◇パッチの目的
艦載機ダメージを各スロットの艦載機個別に持てる様にしたい
艦載機の残存数の表示もできる様にしたい

◇注意点
AIR_SHIP_DAMAGEの要素数が(2023/05/21現在において)1500から30000まで増えるのでセーブデータの肥大化に繋がる実装法と思われます


◇変更点
艦載機ダメージを各スロットに当てはめる為にAIR_SHIP_DAMAGEを「MAX_SHIP, MAX_SLOT + MAX_SLOT」に変更
	@SWAP_SHIPDATAのスワップ処理をFOR文にして対応
	@GET_JOIN_AIR_SHIPで個別のスロットを確認する様に変更
	@DELETE_SHIPの初期化処理をFOR文にして対応
	@DOCK_REPAIRの確認方法を変更
	@REPAIR_SHIPの艦載機回復をFOR文に変更・ついでにメッセージに船の名称とスロット番号(主砲と副砲で連番)の表示を追加

艦載機の撃墜処理等を@CALC_ANTI_AIR_DAMAGEから@FIRE_TO_TARGETに移植
	引数を増やすよりもスロットの場所等を保持している場所でやってしまう事に

副砲枠艦載機の数を返す関数@GET_JOIN_AIR_SHIP_SUBを追加
	今までは副砲枠艦載機でも主砲を確認していたので処理が正常には成立していなかったと思われる

@CREATE_WEAPON_WINDOWの引数ににSHIP_IDとSLOT_IDを追加し、詳細画面から艦載機の残存数を表示出来る様に
	@GET_JOIN_AIR_SHIPおよび@GET_JOIN_AIR_SHIP_SUBを使用

その他バグ修正
@ABL_ACTIVATIONのバグを修正
	_COMMANDER:1の指揮官スキルの判定で_COMMANDER:0の目星を参照していた→被遠隔攻撃時など味方指揮官が存在して・敵艦隊指揮官が存在しない場合や、それ以外で敵艦隊指揮官のみが存在する場合などに(-1のキャラのステータスを見ようとして)エラーが出る

◇更新履歴
ver1.1
変数名の誤字を修正