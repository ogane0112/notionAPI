# notionAPI
notionAPIについてまとめてみました！
# そもそもnotionとは?
notionとは～簡単に言うと最強のmemoアプリであり,様々なカスタムをすることができ他のメモアプリではなかなかできないようなクールな機能を実装することができる！
# notionAPIを用いて作成した物まとめ
### 筋トレ記録をグラフ化する(spreadsheetを用いて)
テンプレートを用いてデータベースに筋トレ記録を付けていたがグラフにして目に見える形で記録を付けたいなと思い実装してみた！  
2022年ごろまではnotionchartsという別媒体を用いることで簡単にnotionにグラフを挿入することができていたが現在はサービス終了してしまいグラフを気軽に作成することができないので  updateで追加されるその日までこの実装したコードで代替品としようと思います～！
```javascript
function getNotionData() {
  const database_id = '省略';
  const url = 'https://api.notion.com/v1/databases/' + database_id + '/query';
  const token = 'もちろん省略';

  let headers = {
    'content-type': 'application/json; charset=UTF-8',
    'Authorization': 'Bearer ' + token,
    'Notion-Version': '2021-08-16',
  };

  let options = {
    'method': 'post',
    'headers': headers,
    "muteHttpExceptions": true
  };

  let notion_data = UrlFetchApp.fetch(url, options);
  notion_data = JSON.parse(notion_data);
   // 取得したデータから一番最後の行を取得
  let distance = notion_data.results[0].properties['距離(Km)'].number;
  let time = notion_data.results[0].properties['時間(分)'].number;
  console.log(distance)
  console.log(time)
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var sheet = ss.getSheetByName('シート1');  // スプレッドシートのシート名を適宜変更
  var today = new Date();
  let month = today.getMonth() + 1;
  let day = today.getDate();
  let date =  month + "/" + day 
  sheet.appendRow([date,distance,time])

}
```  
このコードヲ実行するとnotionのデータベースからデータを取得することができる。もしコピペして利用する場合は,jsonのデータ形式(notion_data)をconsole.logなどで確認してから取得するデータにアクセスしてそれぞれ書き込まれるようにして下さい！実はほとんど参考にしたサイトのコピペで公式ドキュメントなどはほとんど参考にしていないです（笑）.  
**参考サイト**  
[NotionAPI x GAS で出来ること](https://blog.shinonome.io/notion-gas/)

### Googleカレンダー連携機能

# APIの仕様について


