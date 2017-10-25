### 前台
```javascript
function save(status) {
		var detailArray = new Array();
		var records = oTable.rows().data();
		records.each(function(data,i){
			detailArray.push(data);
		});
		var d = {"details" : detailArray, "journalState" : status, "journalId":$("#journalId").val()};
		var url = "plat/dataJournal/review/save";
		
		post({
			url : url,
			param : {paramJson : JSON.stringify(d)}, // 字符串格式的Json数据
			resultTitle : "复核结果",
			succMsg : "复核成功",
			callParentConfirm : true
		});
}
```

### 后台

```java
DataJournalVo vo = JsonUtil.json2Obj(paramJson, DataJournalVo.class); // 接收Json字符串，转化为对象

// DataJournalVo.java
public class DataJournalVo {
	private List<DataJournalDetail> details;
	private String journalState;
	private Long journalId;
}

// DataJournalDetail.java
public class DataJournalDetail implements Serializable {

	@DbField(column="JOURNAL_ID")
	private Long journalId;	//流水ID
	
	@DbField(column="DATA_COMMENT")
	private String dataComment;// 数据说明
	
	@DbField(column="DATA_ORIGINAL")
	private String originalValue;// 数据原值

	@DbField(column="DATA_NEWVALUE")
	private String newValue;// 数据新值
	
	@DbField(column="FIELD_NAME")
	private String fieldName;// java字段名称
  
}
```

