<!-- 罫線を表示 -->
<style type="text/css"> 
    table { border-collapse: collapse; } 
    table, th, td { border: 1px solid black; } 
</style>

<br><br>



参考 ： 
DataWedge 設定 - 
DataWedge 11.0  
https://techdocs.zebra.com/datawedge/dw-jp/11-0/guide/settings/

<br>

# 1. DataWedge機能の要点説明


---

Import/Exportに関する機能に関する記載についてZebra Tech Docsを読み解く時間がない人のための簡易指南書。
Import/Exportには2種類のプロファイルが存在するので間違えないように。

<br>  


- Import/Export
    - DWedge全体の設定と構成情報
    - 構成ファイルをインポートすると、デバイスに保存されているすべての DataWedge 設定とプロファイルが上書き可能。
    - Profile Filename : datawedge.db  
    ▲ DWedge全体の構成・設定に関する機能のInmport/Export

<br>  

- Import/Export Profile
    - 個別のプロファイルからの設定と構成情報。
    - Profile は複数のProfileとその他の DataWedge 設定を含めることが可能。
    - Profile Filename : dwprofile_\<profilename>.db  
    ▲ 個別のProfileに関する構成・設定ののInmport/Export

<br>  

---
### ディレクトリ構成

Android 端末上で関連するパスを列記。Import/Export先を混同しないように。

<br>  

|||
|-|-|
| /enterprise/device/settings/datawedge/enterprisereset/    | ProfileのImport先。エンタープライズリセット後にこのフォルダがチェックされ、構成ファイルや存在するプロファイルがインポートされる。
| /enterprise/device/settings/datawedge/autoimport          | ProfileのImport先。常時★ここに配置されている構成ファイル★が即時インポートされ、設定が上書きされる。Stagenow, MDM/EMM利用時のインポートではこちらが多用される。
| /storage/emulated/0/Android/data/com.symbol.datawedge/files | ProfileのExport 先。

<br>
<br>


    ET40L:/storage/emulated/0/Android/data/com.symbol.datawedge/files $ pwd
    /storage/emulated/0/Android/data/com.symbol.datawedge/files ★  

    ET40L:/storage/emulated/0/Android/data/com.symbol.datawedge/files $ ls -al
    total 425
    drwxrwxrwx 3 u0_a107 ext_data_rw   3452 2023-05-04 19:13 .
    drwxrws--- 3 u0_a107 ext_data_rw   3452 2023-02-27 09:27 ..
    drwxrws--- 2 u0_a107 ext_data_rw   3452 2023-02-27 09:27 autoimport
    -rwxrwxrwx 1 u0_a107 u0_a107     185344 2023-05-04 19:13 datawedge.db ★
    -rwxrwxrwx 1 u0_a107 u0_a107     237568 2023-05-04 19:09 dwprofile_chrome.db ★  

▲ Exportされた profile を表示。全体と個別（2種類）のプロファイルの確認が可能。

<br>


★ Dwedge起動・動作中のAutoImportはMX/Android Version によって、挙動が異なる。心配な方はTechDocsを熟読推奨。  
https://techdocs.zebra.com/datawedge/dw-jp/11-0/guide/settings/#importaconfig

<br>  

---
### ファイル構成

Android 端末上で関連するProfileを列記。

<br>  

|||
|-|-|
| datawedge.db | インポート対象の (エクスポートされた) DataWedge 構成データベース
| dwprofile_<profile_name>.db | インポート対象の (エクスポートされた) 個別のプロファイル

<br>
<br>
<br>

# 2. トラブルシューティング Tips - Logcat 取得方法 （上級者向け）
---

https://developer.android.com/tools/logcat

▼ サンプル

    PS C:\Users\mogetan> adb shell ★  

    ET40L:/ $ logcat |grep -i "datawedge" ★
    05-04 18:32:05.922   836   836 I GoogleInputMethodService: GoogleInputMethodService.onStartInput():1898 onStartInput(EditorInfo{inputType=0x0(NULL) imeOptions=0x0 privateImeOptions=null actionName=UNSPECIFIED actionLabel=null actionId=0 initialSelStart=-1 initialSelEnd=-1 initialCapsMode=0x0 hintText=null label=null packageName=com.symbol.datawedge fieldId=-1 fieldName=null extras=null hintLocales=[]}, false)
    05-04 18:32:11.459  1773 11142 I ActivityTaskManager: START u0 {flg=0x14000000 cmp=com.symbol.datawedge/.dwReporting (has extras)} from uid 10107
    05-04 18:32:11.472  1773  5020 I ActivityTaskManager: The Process com.symbol.datawedge Already Exists in BG. So sending its PID: 5834
    05-04 18:32:11.473  1773  5020 W WindowManager: debug rotationForOrientation topActivity:com.symbol.datawedge

<br>
<br>


# 付録：Stagenow を用いて自動インポート/ 設定例

| Configirations    | Values    |
|-|-|
| StgNow profile    | FileMgr-DataWedge-Profile-02-ftp-Auth
| Target path & file| /enterprise/device/settings/datawedge/autoimport/dwprofile_chrome.db
| Source URI        | ftp-p://zebra:zebra@192.168.4.55:21/home/zebra/stagenow/file/dwprofile_chrome.db
| Result            | Success


<br>


| Configirations    | Values    |
|-|-|
| StgNow profile    | FileMgr-DataWedge-Profile-04-http-NoAuth
| Target path & file| /enterprise/device/settings/datawedge/autoimport/dwprofile_chrome.db
| Source URI        | http://192.168.4.55/latest/datawedge/dwprofile_chrome.db
| Result            | Success















