---
title: "CYBERSEC 2022 台灣資安大會"
date: 2022-09-26T18:02:43+08:00
tags: ['conference']
---

## 議程
* Day 1
    ![image](https://imgur.com/GL8z3jN.jpeg)
* Day 2
    ![image](https://imgur.com/nUAsiDj.jpeg)
* Day 3
    ![image](https://imgur.com/feWLZWe.jpeg)

## 我參加的講座筆記
這次參加的是 Day 3 的議程，主要想聽關於紅隊演練的議程

### 701B "在家工作"到"隨時隨處工作" — 零信任方式下的後疫情時代數位化轉型, Zscaler, Aaron Tseng
* 零信任
    在家工作

    員工從家裡存取公司服務都是安全的嗎？

    因次提出了要讓員工使用vpn連進公司內網的需求

    但是多了vpn就安全了嗎？

    只要有vpn進入後就可以在公司內網中四處搜尋

    因次要加上綁定身份的功能

    HR 連上vpn後只能使用 HR 相關的系統

    RD 連上vpn後只能使用 RD 相關的系統

* 零攻擊面

    1. 換證後只能看到自己有權限的服務

    2. 檢查端點風險等級
        會檢查目前電腦的安全機制，需要購安全才能連線

    3. ZPA 取代 VPN
        (ZPA 是這家公司的產品)

        ![image](https://s4.itho.me/sites/default/files/images/ZPA%20Architecture%20Simple.png)

        你要先連上他的雲(broker)，由這個雲去轉發存取服務
        
        所以在這個平台上設定好權限和過濾等資料即可避免使用者想去亂撈其他服務的狀況

### 701F 打密碼太過時?安全地登入雲端與 SaaS 服務 的最新趨勢, HENNGE, 中込 剛
* 常見的攻擊手法
    * 社交工程攻擊
        只要你有憑證你被攻擊時就會被偷走

    * 信箱zip

    * 釣魚真人信件

* 解法
    1. 如何驗證有憑證的使用者？
        綁定裝置憑證
    2. 如果裝置被害怎麼辦？
        多裝置驗證
    
    如果PC端想要登入，此時需要手機驗證


* 搭配單一登入入口
    基礎的密碼 + 裝置憑證 ＋ 推播通知驗證

#### 結語
* 2022 的安全威脅趨勢是 『人』而不是系統

    案例: [Lapsas$](https://zh.wikipedia.org/zh-tw/Lapsus$), [Emotet](https://www.ithome.com.tw/news/149414), 網路釣魚

* 透過 裝置驗證 可以有效解決人為錯誤問題

### 701F 如何正確使用紅隊演練, 戴夫寇爾, 翁浩正
* RED TEAM 市場在哪？
    高科技業 政府網站

    每個企業都需要屬於自己的資安 

    不一定是直接抄別人就好

    每個公司都想做紅隊 

    但是品質問題 

    如果爛的紅隊就只是滲透測試報告而已

* 什麼是紅隊？
    紅隊是全方位打擊而非指定主機攻擊

    弱掃 + 滲透 + 紅隊

    信任紅隊並接受結果

    不要什麼都不給打

    開案結案績效如何計算

* 如何進行紅隊演練

    -|階段一: 初次合作| 階段二: 資安成熟度成長中 | 階段三: 資安成熟度高 
    -|-|-|-
    時間|揭露演練日期和時間| 揭露演練時間並拉長日期| 不限演練日期和時間
    網路|固定IP位置|可用動態IP位置|全程使用動態IP位置以及使用網路服務作為跳板
    範圍|以企業最常使用的網域，IP位置，雲端服務為主|除常見範圍，也納入設備，供應鏈，子公司，跨國辦公室|所有數位資產皆納入範圍
    溝通|諾藍隊有任何疑問，可由紅隊提出說明|諾藍隊提出疑問，紅隊視情況說明|紅隊僅需告知重大漏洞，其餘資訊不提供
    情境|以外網入侵為主|先以外網為主，後變換其他情境|視情況可從內網開始演練，並納入實體入侵，社交工程等方式
    籃隊|監控但不阻擋|發現攻擊行為可以阻擋，多次阻擋成功後，再採取監控不阻擋的放行機制|發現攻擊立即阻擋，不必放行

* 紅隊演練開演前準備
    1. 確認演練範圍
        * 公司授權演練範圍，如網域,主機,子公司等
        * 盤點高敏感資料主機
        * 需避開的範圍,時段或手法
    2. 確認演練情境
        * 從外部網路出發(外部系統)
        * 從內部網路出發(內部資安事件)
        * 演練之前遇到資安事件的情境類型
        * 特定攻擊手法，如WIFI入侵, 實體入侵等
    3. 確認演練目標
        * 盤點公司最在意的主機或資料是什麼
        * 與核心目標有相依性的主機有哪些
        * 曾經發生過事件的高危險主機
        * 盤點重要基礎設施,核心系統,機密資料,特權帳號權限

* 專案怎麼合作
    ![image](https://imgur.com/dzL2ALj.jpg)
    ![image](https://imgur.com/1iJC9PX.jpg)

* 如何結案驗收
    - 資安廠商
        - 攻擊的 `方法論`, 策略,  攻擊觀點的安全建議
        - 詳細的 `攻擊路徑`, 漏洞說明
        - `簡報交流` 雙方攻防的想法
    - 企業內部
        - `回顧`演練流程
        - 重新調整資安`策略`
        - 擬訂長期演練`計畫`
#### 結語
- 從需求出發，思考為什麼我需要這個服務，我想要解決什麼問題 
- 紅隊就是盟友，藉由演練盤點人員, 設備, 流程的問題
- 做好開案, 成案, 結案的準備，將資源發揮最大效益

### 701F 我真的適合執行紅隊演練嗎? 金融業的經驗分享, 金融業資安單位, 張哲銘

資通安全責任等級分級辦法 - [參考法條](https://law.moj.gov.tw/LawClass/LawAll.aspx?pcode=A0030304)

反正就是說分五等級 ABCDE 公司，每年要看自己哪一等做多少跟資安相關的檢查

* 紅隊演練的專案規劃階段概要
    ![image](https://lh3.googleusercontent.com/pw/AL9nZEVWBYZDdgbF-yI5RxKfFL7JrHUf3n1JEiZQA3zwgYLXzUg9nouXCsysD3IQpIqVm55XzAguaZ9TNqHkDU1VipQcccNnU5FXYSgOu7-60knDVTTXeGMRDNUQdwY9L40mWvOflIzYJZUKjXx2FFCSSyU=w2160-h1216-no?authuser=0)

* 廠商的遴選與合約要素
    ![image](https://lh3.googleusercontent.com/pw/AL9nZEXnRYwjSeWVViXOSJ8uZwocjlfeL9XRRAFGmS5DrrzZFVOl5qGyMjkNnzFMMnSQL9xz3Ms_DZhXhHMUm_EmyeCuHoA9kX6JyETGZeQtmgPJQdGvLJEtv8ApuGdUmz8c_ErwY-qPGtJONSwtmm-l410=w1679-h944-no?authuser=0)    

    多組廠商遴選
    ![image](https://lh3.googleusercontent.com/pw/AL9nZEX-dI5QujIRSYptIkNrlgtMO9TvOVRKcfapXpb_0B9SQ55k-os0-6offLdbCSQqYcWH-gW57xvkh9sSDiTHQIslJYcP2WOatuoLiU9HNF0UC1FAnjqXV8A7GD6rkpHLlLUffr5GZdzXsmHbCU8LGrc=w1679-h944-no?authuser=0)


* 演練原則
    ![image](https://lh3.googleusercontent.com/pw/AL9nZEV6-lCj2xLFO7LtSk1Az1bvVs7zGelyAShqXAAw7mwmtQ-ndew5hSg9RywuTZGKTa4VseXQU8m4MgKPd4HLa95bYRY26PWobeMzHR23trYVj_M8KP6D5NmkSyaV1lrGPCTZlTxsmpEFMYHhbGHz9jg=w1679-h944-no?authuser=0)

* 演練目標
    ![image](https://lh3.googleusercontent.com/pw/AL9nZEWViOoXPMMElt4M-l-CNvolimrdhMWrVtfgKZvaPz0DO0jNKgTgfdKl-wydxrAY2hnAitrpfRWsTtvU2wZ4dTOEszoLiVmARbEiYn_ZLX_eIRi23fy4XyGuYdB4yu6KAkxciIZa1K9xB7v0mq5NooA=w1679-h944-no?authuser=0)

* 紅隊演練人員組成
    ![image](https://lh3.googleusercontent.com/pw/AL9nZEWYs-pQLDBkbkfpJMzzf9N0YHhwbmqUVl9123LScsjpS3LFmpvy7DMeT3sk2jMKVSVTaSPffiLHy9nv6FSVDKzJIql8XimCUb8pkKOiiN-dCeP9yDfhgm725KXPUQfmC3-y3xbX8XxNWQ9i90ec5D0=w1679-h944-no?authuser=0)

* 演練後
    ![image](https://lh3.googleusercontent.com/pw/AL9nZEWYs-pQLDBkbkfpJMzzf9N0YHhwbmqUVl9123LScsjpS3LFmpvy7DMeT3sk2jMKVSVTaSPffiLHy9nv6FSVDKzJIql8XimCUb8pkKOiiN-dCeP9yDfhgm725KXPUQfmC3-y3xbX8XxNWQ9i90ec5D0=w1679-h944-no?authuser=0)

    記錄演練時發生的時間序列圖 非常重要 (下圖為模擬圖並非真實發生)
    ![image](https://lh3.googleusercontent.com/pw/AL9nZEVgb-0zGNmqS4LdDFFNBVixs2B-vnUaOAHhfvCJaaDKMSFjhppsWqI6LRObDMop4saLbJRKe5Z8EkEgSA---YHBWT-cBiV7Y_KDHeOKsxnufVOwzxZiNcNXVjwlZpYKEM4KEM1P2ANTKjEH-AGqZ5I=w1679-h944-no?authuser=0)

#### 結語
![image](https://lh3.googleusercontent.com/pw/AL9nZEVxvCFnSyqNAD6T_nqSK807Uaa1tAQCXn-Nm8jHwNrtpUNEPxfgeT2fYtCCCd77cjgj1tFLvxSCNokOMTIJZ172vya0mBORP07WZE7ZCvBAK9u-l2_PD3DiVRtXcTlbRnkqmQaQr4Ntx4EuFleNHCQ=w1679-h944-no?authuser=0)


### 701A 真實風險與ATT&CK的鴻溝, 戴夫寇爾, 翁浩正

* ATT&CK
    
    ATT&CK® 是一個紀錄資安攻擊與情資分享的資料庫，也是紅隊與藍隊之間溝通的橋樑，市面上許多端點防禦軟體都使用 ATT&CK® 作為防護的標準。

    https://attack.mitre.org/

    REF: [[ATT&CK®] 1 ATT&CK® 基本介紹](https://feifei.tw/attck-intro/)

* 金融業常見的 ATT&CK 手法

    ![image](https://lh3.googleusercontent.com/pw/AL9nZEUvA0RSw_Q9ZoQiNNB6HBBO9MYmYLUAMxz3tHpbyptnFE1NSzfmc8HIskv5148oOjI1XGxcnm3DUqRwth5KgbbglW0eUiOlsLg5fqJi3w8MTKqxRaSVLwaZdcxYLFYBwA6zoaXk1aRYjHw3kiHhnj4=w1679-h944-no?authuser=0)


    知名的 APT10 組織攻擊手法

    ![image](https://imgur.com/t1UYRcH.jpg)

* DEVCORE 最常見的五種攻擊型態
    ![image](https://lh3.googleusercontent.com/pw/AL9nZEUA-fyit3FfLLkP2Zm5_fnQ9sKfD4FuE5gjdF6D7oiDNfft3bG2_LDGzZDwT_NDt4fN8Af-RAT8NvsB7gxDpkOIUVGFPf2kwJ2aUHX92wVfaImQQ4vqiIFL-4NAdc5qwP4qSD7MRS0I49SkEWdwdA8=w1679-h944-no?authuser=0)

#### 結語
ATTACK MATRIC 只是一種考古題

讓我們可以理解別人研究過的攻擊手法

而防禦手法也並非去做到完全的防禦

而是能夠將其在中發現後阻斷

## 戰果 (?)

樓下參展廠商贈品超級多

![image](https://lh3.googleusercontent.com/pw/AL9nZEWRLcAQZ3opum_OKA-XPAMjdsh88Eg0UsrKsYhM8mDyCva2bLBBmaCDx71xCfY2nmQs-lCEjoLCVdoSGs6RDpCZflF-UHG4FhoJAjDCEKVZEXnvGZuFijKcOBcfqFQ2LMIzuLT5G44Ov2wen1ywo9U=w1679-h944-no?authuser=0)


## 相片網址
[CYBERSEC 2022 相簿網址](https://photos.google.com/share/AF1QipNQ142Bqb0-pleI7c81fnnyebHDDjyp3m92H_NxrNVkVSrz4Vw6gpwbX2MlA7LyGA?key=NFB6THQtbzhnX05BcF83WHlVbjhaUlFQbk5scXVn)