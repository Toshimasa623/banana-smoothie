test
flowchart TB
    %% アクターの定義
    ActorCustomer((顧客))
    ActorAdmin((管理者))
    
    %% 外部システムの定義
    subgraph External Systems [外部システム]
        SysLINE[公式LINE]
        SysBASE[BASE]
        SysStripe[Stripe]
        SysAWS[演算基盤: AWS等]
    end

    %% Phase 0: 初期マーケティング & 手動運用
    subgraph Phase0 [Phase 0: BASE販売と手動受注モデル]
        direction TB
        UC0_1(キャンペーンコード付き\nツールの受け取り)
        UC0_2(動画尺別処理代行\n商品の購入)
        UC0_3(ツール配布・\nプロモーション配信)
        UC0_4(手動AI処理・\n調整済み動画納品)
    end

    %% Phase 1: 自社直販と利益率改善
    subgraph Phase1 [Phase 1: 自社サイト構築と決済内製化]
        direction TB
        UC1_1(自社サイトでの\n企業・サービスPR閲覧)
        UC1_2(動画尺別処理代行\n商品の購入: 手数料減)
        UC1_3(受注管理・手動納品)
    end

    %% Phase 2: プラットフォーム化と完全自動化
    subgraph Phase2 [Phase 2: 自動処理プラットフォーム]
        direction TB
        UC2_1(時間単位の\n課金クレジット購入)
        UC2_2(動画アップロードと\n自動処理リクエスト)
        UC2_3(非同期での\nノード調整自動処理)
        UC2_4(処理完了通知と\n調整済み動画の取得)
    end

    %% アクターとユースケースの関連付け (Phase 0)
    ActorCustomer --> UC0_1
    ActorCustomer --> UC0_2
    ActorAdmin --> UC0_3
    ActorAdmin --> UC0_4
    UC0_1 -. 経由 .-> SysLINE
    UC0_3 -. 利用 .-> SysLINE
    UC0_2 -. 決済 .-> SysBASE

    %% アクターとユースケースの関連付け (Phase 1)
    ActorCustomer --> UC1_1
    ActorCustomer --> UC1_2
    ActorAdmin --> UC1_3
    UC1_2 -. 決済 .-> SysStripe

    %% アクターとユースケースの関連付け (Phase 2)
    ActorCustomer --> UC2_1
    ActorCustomer --> UC2_2
    ActorCustomer --> UC2_4
    UC2_1 -. 決済 .-> SysStripe
    UC2_2 -. トリガー .-> UC2_3
    UC2_3 -. 実行・スケール .-> SysAWS
