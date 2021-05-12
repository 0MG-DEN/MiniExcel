<div align="center">
<a href="https://www.nuget.org/packages/MiniExcel"><img src="https://img.shields.io/nuget/v/MiniExcel.svg" alt="NuGet"></a>  <a href="https://www.nuget.org/packages/MiniExcel"><img src="https://img.shields.io/nuget/dt/MiniExcel.svg" alt=""></a>  <a href="https://ci.appveyor.com/project/shps951023/miniexcel/branch/master"><img src="https://ci.appveyor.com/api/projects/status/b2vustrwsuqx45f4/branch/master?svg=true" alt="Build status"></a>
</div>

<div align="center">
<strong><a href="README.md">English</a> | <a href="README.zh-Hant.md">繁體中文</a> | <a href="README.zh-CN.md">简体中文</a></strong>
</div>

<div align="center">
🙌 您的 <a href="https://github.com/shps951023/MiniExcel">Github Star</a> ，能幫助 MiniExcel 讓更多人看到 🙌
</div>

---

### 簡介

MiniExcel 簡單、高效避免OOM的.NET處理Excel查、寫、填充工具。

目前主流框架大多需要將資料全載入到記憶體方便操作，但這會導致記憶體消耗問題，MiniExcel 嘗試以 Stream 角度寫底層算法邏輯，能讓原本1000多MB占用降低到幾MB，避免記憶體不夠情況。

![image](https://user-images.githubusercontent.com/12729184/113084691-1804d000-9211-11eb-9b08-cbb89d9ecdc2.png)

### 特點
- 低記憶體耗用，避免OOM(out of memoery)、頻繁 Full GC 情況
- 支持`即時`操作每行資料
![miniexcel_lazy_load](https://user-images.githubusercontent.com/12729184/111034290-e5588a80-844f-11eb-8c84-6fdb6fb8f403.gif)
- 兼具搭配 LINQ 延遲查詢特性，能辦到低消耗、快速分頁等複雜查詢  
圖片:與主流框架對比的消耗、效率差  
![20210419](https://user-images.githubusercontent.com/12729184/114679083-6ef4c400-9d3e-11eb-9f78-a86daa45fe46.gif)
- 輕量，不需要安裝 Microsoft Office、COM+、不依賴任何套件，DLL小於100KB
- 簡便操作的 API 風格



### 快速開始

- [讀 Excel](#getstart1)
- [寫 Excel](#getstart2)
- [模板填充 Excel](#getstart3)
- [Excel Column Name/Index/Ignore Attribute](#getstart4)
- [範例](#getstart5)


### 安裝

請查看 [NuGet](https://www.nuget.org/packages/MiniExcel)

### 更新日誌

請查看 [Release Notes](docs)

### TODO 

請查看 [TODO](https://github.com/shps951023/MiniExcel/projects/1?fullscreen=true)

### 性能測試

以 [**Test1,000,000x10.xlsx**](benchmarks/MiniExcel.Benchmarks/Test1%2C000%2C000x10.xlsx) 做基準與主流框架做性能測試，總共 1千萬筆 "HelloWorld"，檔案大小 23 MB   

Benchmarks  邏輯可以在 [MiniExcel.Benchmarks](benchmarks/MiniExcel.Benchmarks/Program.cs) 查看或是提交 PR，運行指令

```
dotnet run -p .\benchmarks\MiniExcel.Benchmarks\ -c Release -f netcoreapp3.1 -- -f * --join
```

最後一次運行結果 :  

```
BenchmarkDotNet=v0.12.1, OS=Windows 10.0.19042
Intel Core i7-7700 CPU 3.60GHz (Kaby Lake), 1 CPU, 8 logical and 4 physical cores
  [Host]     : .NET Framework 4.8 (4.8.4341.0), X64 RyuJIT
  Job-ZYYABG : .NET Framework 4.8 (4.8.4341.0), X64 RyuJIT
IterationCount=3  LaunchCount=3  WarmupCount=3  
```

| Method                       | 最大記憶體耗用 |         平均時間 |        Gen 0 |       Gen 1 |      Gen 2 |
| ---------------------------- | -------------: | ---------------: | -----------: | ----------: | ---------: |
| 'MiniExcel QueryFirst'       |       0.109 MB |         726.4 us |            - |           - |          - |
| 'ExcelDataReader QueryFirst' |       15.24 MB |  10,664,238.2 us |  566000.0000 |   1000.0000 |          - |
| 'MiniExcel Query'            |        17.3 MB |  14,179,334.8 us |  367000.0000 |  96000.0000 |  7000.0000 |
| 'ExcelDataReader Query'      |        17.3 MB |  22,565,088.7 us | 1210000.0000 |   2000.0000 |          - |
| 'Epplus QueryFirst'          |       1,452 MB |  18,198,015.4 us |  535000.0000 | 132000.0000 |  9000.0000 |
| 'Epplus Query'               |       1,451 MB |  23,647,471.1 us | 1451000.0000 | 133000.0000 |  9000.0000 |
| 'OpenXmlSDK Query'           |       1,412 MB |  52,003,270.1 us |  978000.0000 | 353000.0000 | 11000.0000 |
| 'OpenXmlSDK QueryFirst'      |       1,413 MB |  52,348,659.1 us |  978000.0000 | 353000.0000 | 11000.0000 |
| 'ClosedXml QueryFirst'       |       2,158 MB |  66,188,979.6 us | 2156000.0000 | 575000.0000 |  9000.0000 |
| 'ClosedXml Query'            |       2,184 MB | 191,434,126.6 us | 2165000.0000 | 577000.0000 | 10000.0000 |


| Method                   | 最大記憶體耗用 |         平均時間 |        Gen 0 |        Gen 1 |      Gen 2 |
| ------------------------ | -------------: | ---------------: | -----------: | -----------: | ---------: |
| 'MiniExcel Create Xlsx'  |          15 MB |  11,531,819.8 us | 1020000.0000 |            - |          - |
| 'Epplus Create Xlsx'     |       1,204 MB |  22,509,717.7 us | 1370000.0000 |   60000.0000 | 30000.0000 |
| 'OpenXmlSdk Create Xlsx' |       2,621 MB |  42,473,998.9 us | 1370000.0000 |  460000.0000 | 50000.0000 |
| 'ClosedXml Create Xlsx'  |       7,141 MB | 140,939,928.6 us | 5520000.0000 | 1500000.0000 | 80000.0000 |



### 讀 Excel <a name="getstart1"></a>

- 支持任何 stream 类型 : FileStream,MemoryStream



#### 1. Query 查詢 Excel 返回`強型別` IEnumerable 資料 [[Try it]](https://dotnetfiddle.net/w5WD1J)

```csharp
public class UserAccount
{
    public Guid ID { get; set; }
    public string Name { get; set; }
    public DateTime BoD { get; set; }
    public int Age { get; set; }
    public bool VIP { get; set; }
    public decimal Points { get; set; }
}

var rows = MiniExcel.Query<UserAccount>(path);

// or

using (var stream = File.OpenRead(path))
    var rows = stream.Query<UserAccount>();
```

![image](https://user-images.githubusercontent.com/12729184/111107423-c8c46b80-8591-11eb-982f-c97a2dafb379.png)


#### 2. Query 查詢 Excel 返回`Dynamic` IEnumerable 資料 [[Try it]](https://dotnetfiddle.net/w5WD1J)

* Key 系統預設為 `A,B,C,D...Z`

| MiniExcel     | 1     |
| -------- | -------- |
| Github     | 2     |

```csharp

var rows = MiniExcel.Query(path).ToList();

// or 
using (var stream = File.OpenRead(path))
{
    var rows = stream.Query().ToList();
                
    Assert.Equal("MiniExcel", rows[0].A);
    Assert.Equal(1, rows[0].B);
    Assert.Equal("Github", rows[1].A);
    Assert.Equal(2, rows[1].B);
}
```

#### 3. 查詢資料以第一行數據當Key [[Try it]](https://dotnetfiddle.net/w5WD1J)

注意 : 同名以右邊數據為準   

Input Excel :    

| Column1 | Column2 |
| -------- | -------- |
| MiniExcel     | 1     |
| Github     | 2     |


```csharp

var rows = MiniExcel.Query(useHeaderRow:true).ToList();

// or

using (var stream = File.OpenRead(path))
{
    var rows = stream.Query(useHeaderRow:true).ToList();

    Assert.Equal("MiniExcel", rows[0].Column1);
    Assert.Equal(1, rows[0].Column2);
    Assert.Equal("Github", rows[1].Column1);
    Assert.Equal(2, rows[1].Column2);
}
```

#### 4. Query 查詢支援延遲加載(Deferred Execution)，能配合LINQ First/Take/Skip辦到低消耗、高效率複雜查詢

舉例 : 查詢第一筆資料

```csharp
var row = MiniExcel.Query(path).First();
Assert.Equal("HelloWorld", row.A);

// or

using (var stream = File.OpenRead(path))
{
    var row = stream.Query().First();
    Assert.Equal("HelloWorld", row.A);
}
```

與其他框架效率比較 :  

![queryfirst](https://user-images.githubusercontent.com/12729184/111072392-6037a900-8515-11eb-9693-5ce2dad1e460.gif)

#### 5. 查詢指定 Sheet 名稱

```csharp
MiniExcel.Query(path, sheetName: "SheetName");
//or
stream.Query(sheetName: "SheetName");
```

#### 6. 查詢所有 Sheet 名稱跟資料

```csharp
var sheetNames = MiniExcel.GetSheetNames(path).ToList();
foreach (var sheetName in sheetNames)
{
    var rows = MiniExcel.Query(path, sheetName: sheetName);
}
```

#### 7. 查詢所有欄(列)

```csharp
var columns = MiniExcel.GetColumns(path); // e.g result : ["A","B"...]

var cnt = columns.Count;  // get column count
```

#### 8. Dynamic Query 轉成 `IDictionary<string,object>` 資料

```csharp
foreach(IDictionary<string,object> row in MiniExcel.Query(path))
{
    //..
}
```

![image](https://user-images.githubusercontent.com/12729184/116673475-07917200-a9d6-11eb-947e-a6f68cce58df.png)



#### 9. Query 讀 Excel 返回 DataTable

提醒 : 不建議使用，因為DataTable會將數據`全載入內存`，失去MiniExcel低記憶體消耗功能。

```C#
var table = MiniExcel.QueryAsDataTable(path, useHeaderRow: true);
```

![image](https://user-images.githubusercontent.com/12729184/116673475-07917200-a9d6-11eb-947e-a6f68cce58df.png)

#### 10. 指定單元格開始讀取資料

```csharp
MiniExcel.Query(path,useHeaderRow:true,startCell:"B3")
```

![image](https://user-images.githubusercontent.com/12729184/117260316-8593c400-ae81-11eb-9877-c087b7ac2b01.png)

#### 11. 向下填充合併的單元格

注意 : 效率相對於`沒有使用合併填充`來說差
底層原因 : OpenXml 標准將 mergeCells 放在文件最下方，導致需要遍歷兩次 sheetxml

```csharp
	var config = new OpenXmlConfiguration()
	{
		fillDownMergedCells = true
	};
	var rows = MiniExcel.Query(path, useHeaderRow: true, configuration: config);
```

![image](https://user-images.githubusercontent.com/12729184/117941845-0e58a700-b33d-11eb-8dbc-a18af0752c4f.png)





### 寫 Excel  <a name="getstart2"></a>

1. 必須是非abstract 類別有公開無參數構造函數
2. MiniExcel SaveAs 支援 `IEnumerable參數延遲查詢`，除非必要請不要使用 ToList 等方法讀取全部資料到記憶體

圖片 : 是否呼叫 ToList 的記憶體差別  

#### ![image](https://user-images.githubusercontent.com/12729184/112587389-752b0b00-8e38-11eb-8a52-cfb76c57e5eb.png)1. Anonymous or strongly type [[Try it]](https://dotnetfiddle.net/w5WD1J)

```csharp
var path = Path.Combine(Path.GetTempPath(), $"{Guid.NewGuid()}.xlsx");
MiniExcel.SaveAs(path, new[] {
    new { Column1 = "MiniExcel", Column2 = 1 },
    new { Column1 = "Github", Column2 = 2}
});
```

#### 2. Datatable:  

- 優先使用 Caption 當欄位名稱

```csharp
var path = Path.Combine(Path.GetTempPath(), $"{Guid.NewGuid()}.xlsx");
var table = new DataTable();
{
    table.Columns.Add("Column1", typeof(string));
    table.Columns.Add("Column2", typeof(decimal));
    table.Rows.Add("MiniExcel", 1);
    table.Rows.Add("Github", 2);
}

MiniExcel.SaveAs(path, table);
```

#### 3. Dapper

```csharp
using (var connection = GetConnection(connectionString))
{
    var rows = connection.Query(@"select 'MiniExcel' as Column1,1 as Column2 union all select 'Github',2");
    MiniExcel.SaveAs(path, rows);
}
```

#### 4. `IEnumerable<IDictionary<string, object>>`

```csharp
var values = new List<Dictionary<string, object>>()
{
    new Dictionary<string,object>{{ "Column1", "MiniExcel" }, { "Column2", 1 } },
    new Dictionary<string,object>{{ "Column1", "Github" }, { "Column2", 2 } }
};
MiniExcel.SaveAs(path, values);
```

output : 

| Column1 | Column2 |
| -------- | -------- |
| MiniExcel     | 1     |
| Github     | 2     |

#### 5. SaveAs 支持 Stream，生成文件不落地 [[Try it]](https://dotnetfiddle.net/JOen0e)

```csharp
using (var stream = new MemoryStream()) //支持 FileStream,MemoryStream..等
{
    stream.SaveAs(values);
}
```

像是 API 導出 Excel

```csharp
public IActionResult DownloadExcel()
{
    var values = new[] {
        new { Column1 = "MiniExcel", Column2 = 1 },
        new { Column1 = "Github", Column2 = 2}
    };

    var memoryStream = new MemoryStream();
    memoryStream.SaveAs(values);
    memoryStream.Seek(0, SeekOrigin.Begin);
    return new FileStreamResult(memoryStream, "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet")
    {
        FileDownloadName = "demo.xlsx"
    };
}
```

#### 6. 支持 IDataReader 參數

```csharp
MiniExcel.SaveAs(path, reader);
```



### 模板填充 Excel <a name="getstart3"></a>

- 宣告方式類似 Vue 模板 `{{變量名稱}}`, 或是集合渲染 `{{集合名稱.欄位名稱}}`
- 集合渲染支持 IEnumerable/DataTable/DapperRow



#### 1. 基本填充

模板:  
![image](https://user-images.githubusercontent.com/12729184/114537556-ed8d2b00-9c84-11eb-8303-a69f62c41e5b.png)

最終效果:  
![image](https://user-images.githubusercontent.com/12729184/114537490-d8180100-9c84-11eb-8c69-db58692f3a85.png)

代碼:  
```csharp
// 1. By POCO
var value = new
{
    Name = "Jack",
    CreateDate = new DateTime(2021, 01, 01),
    VIP = true,
    Points = 123
};
MiniExcel.SaveAsByTemplate(path, templatePath, value);


// 2. By Dictionary
var value = new Dictionary<string, object>()
{
    ["Name"] = "Jack",
    ["CreateDate"] = new DateTime(2021, 01, 01),
    ["VIP"] = true,
    ["Points"] = 123
};
MiniExcel.SaveAsByTemplate(path, templatePath, value);
```



#### 2. IEnumerable/DataTable 數據填充

> Note1: 同行從左往右以第一個 IEnumerableUse 當列表來源 (不支持同列多集合)

模板:   
![image](https://user-images.githubusercontent.com/12729184/114564652-14f2f080-9ca3-11eb-831f-09e3fedbc5fc.png)

最終效果:   
![image](https://user-images.githubusercontent.com/12729184/114564204-b2015980-9ca2-11eb-900d-e21249f93f7c.png)

代碼:

```csharp
//1. By POCO
var value = new
{
    employees = new[] {
        new {name="Jack",department="HR"},
        new {name="Lisa",department="HR"},
        new {name="John",department="HR"},
        new {name="Mike",department="IT"},
        new {name="Neo",department="IT"},
        new {name="Loan",department="IT"}
    }
};
MiniExcel.SaveAsByTemplate(path, templatePath, value);

//2. By Dictionary
var value = new Dictionary<string, object>()
{
    ["employees"] = new[] {
        new {name="Jack",department="HR"},
        new {name="Lisa",department="HR"},
        new {name="John",department="HR"},
        new {name="Mike",department="IT"},
        new {name="Neo",department="IT"},
        new {name="Loan",department="IT"}
    }
};
MiniExcel.SaveAsByTemplate(path, templatePath, value);
```



#### 3. 複雜數據填充

> Note: 支持多 sheet 填充,並共用同一組參數

模板: 

![image](https://user-images.githubusercontent.com/12729184/114565255-acf0da00-9ca3-11eb-8a7f-8131b2265ae8.png)

最終效果: 

![image](https://user-images.githubusercontent.com/12729184/114565329-bf6b1380-9ca3-11eb-85e3-3969e8bf6378.png)

代碼:  

```csharp
// 1. By POCO
var value = new
{
    title = "FooCompany",
    managers = new[] {
        new {name="Jack",department="HR"},
        new {name="Loan",department="IT"}
    },
    employees = new[] {
        new {name="Wade",department="HR"},
        new {name="Felix",department="HR"},
        new {name="Eric",department="IT"},
        new {name="Keaton",department="IT"}
    }
};
MiniExcel.SaveAsByTemplate(path, templatePath, value);

// 2. By Dictionary
var value = new Dictionary<string, object>()
{
    ["title"] = "FooCompany",
    ["managers"] = new[] {
        new {name="Jack",department="HR"},
        new {name="Loan",department="IT"}
    },
    ["employees"] = new[] {
        new {name="Wade",department="HR"},
        new {name="Felix",department="HR"},
        new {name="Eric",department="IT"},
        new {name="Keaton",department="IT"}
    }
};
MiniExcel.SaveAsByTemplate(path, templatePath, value);
```

#### 4. 大數據填充效率比較

> NOTE: 在 MiniExcel 使用 IEnumerable 延遲 ( 不ToList ) 可以節省記憶體使用

![image](https://user-images.githubusercontent.com/12729184/114577091-5046ec80-9cae-11eb-924b-087c7becf8da.png)



#### 5. Cell 值自動類別對應

模板

![image](https://user-images.githubusercontent.com/12729184/114802504-64830a80-9dd0-11eb-8d56-8e8c401b3ace.png)

最終效果

![image](https://user-images.githubusercontent.com/12729184/114802419-43221e80-9dd0-11eb-9ffe-a2ce34fe7076.png)

類別

```csharp
public class Poco
{
    public string @string { get; set; }
    public int? @int { get; set; }
    public decimal? @decimal { get; set; }
    public double? @double { get; set; }
    public DateTime? datetime { get; set; }
    public bool? @bool { get; set; }
    public Guid? Guid { get; set; }
}
```

代碼

```csharp
var poco = new TestIEnumerableTypePoco { @string = "string", @int = 123, @decimal = decimal.Parse("123.45"), @double = (double)123.33, @datetime = new DateTime(2021, 4, 1), @bool = true, @Guid = Guid.NewGuid() };
var value = new
{
    Ts = new[] {
        poco,
        new TestIEnumerableTypePoco{},
        null,
        poco
    }
};
MiniExcel.SaveAsByTemplate(path, templatePath, value);
```



#### 6. Example :  列出 Github 專案

模板

![image](https://user-images.githubusercontent.com/12729184/115068665-221f1200-9f25-11eb-9820-3d7d9638cb03.png)

最終效果

![image](https://user-images.githubusercontent.com/12729184/115068685-2814f300-9f25-11eb-96b5-0e7f21297f4d.png)

代碼

```csharp
var projects = new[]
{
    new {Name = "MiniExcel",Link="https://github.com/shps951023/MiniExcel",Star=146, CreateTime=new DateTime(2021,03,01)},
    new {Name = "HtmlTableHelper",Link="https://github.com/shps951023/HtmlTableHelper",Star=16, CreateTime=new DateTime(2020,02,01)},
    new {Name = "PocoClassGenerator",Link="https://github.com/shps951023/PocoClassGenerator",Star=16, CreateTime=new DateTime(2019,03,17)}
};
var value = new
{
    User = "ITWeiHan",
    Projects = projects,
    TotalStar = projects.Sum(s => s.Star)
};
MiniExcel.SaveAsByTemplate(path, templatePath, value);
```



#### 7. DataTable 當參數

```csharp
var managers = new DataTable();
{
    managers.Columns.Add("name");
    managers.Columns.Add("department");
    managers.Rows.Add("Jack", "HR");
    managers.Rows.Add("Loan", "IT");
}
var value = new Dictionary<string, object>()
{
    ["title"] = "FooCompany",
    ["managers"] = managers,
};
MiniExcel.SaveAsByTemplate(path, templatePath, value);
```



### Excel Column Name/Index/Ignore Attribute <a name="getstart4"></a>

e.g

input excel :  

![image](https://user-images.githubusercontent.com/12729184/114230869-3e163700-99ac-11eb-9a90-2039d4b4b313.png)

```csharp
public class ExcelAttributeDemo
{
    [ExcelColumnName("Column1")]
    public string Test1 { get; set; }
    [ExcelColumnName("Column2")]
    public string Test2 { get; set; }
    [ExcelIgnore]
    public string Test3 { get; set; }
    [ExcelColumnIndex("I")] // system will convert "I" to 8 index
    public string Test4 { get; set; } 
    public string Test5 { get; } //wihout set will ignore
    public string Test6 { get; private set; } //un-public set will ignore
    [ExcelColumnIndex(3)] // start with 0
    public string Test7 { get; set; }
}

var rows = MiniExcel.Query<ExcelAttributeDemo>(path).ToList();
Assert.Equal("Column1", rows[0].Test1);
Assert.Equal("Column2", rows[0].Test2);
Assert.Null(rows[0].Test3);
Assert.Equal("Test7", rows[0].Test4);
Assert.Null(rows[0].Test5);
Assert.Null(rows[0].Test6);
Assert.Equal("Test4", rows[0].Test7);
```



### Excel 類別自動判斷 <a name="getstart5"></a>

MiniExcel 預設會根據擴展名或是 Stream 類別判斷是 xlsx 還是 csv，但會有失準時候，請自行指定。

```csharp
stream.SaveAs(excelType:ExcelType.CSV);
//or
stream.SaveAs(excelType:ExcelType.XLSX);
//or
stream.Query(excelType:ExcelType.CSV);
//or
stream.Query(excelType:ExcelType.XLSX);
```



### 範例

#### 1. SQLite & Dapper 讀取大數據新增到資料庫

note : 請不要呼叫 call ToList/ToArray 等方法，這會將所有資料讀到記憶體內

```csharp
using (var connection = new SQLiteConnection(connectionString))
{
    connection.Open();
    using (var transaction = connection.BeginTransaction())
    using (var stream = File.OpenRead(path))
    {
	   var rows = stream.Query();
	   foreach (var row in rows)
			 connection.Execute("insert into T (A,B) values (@A,@B)", new { row.A, row.B }, transaction: transaction);
	   transaction.Commit();
    }
}
```

效能:
![image](https://user-images.githubusercontent.com/12729184/111072579-2dda7b80-8516-11eb-9843-c01a1edc88ec.png)


#### 2. ASP.NET Core 3.1 or MVC 5 下載/上傳 Excel Xlsx API Demo [Try it](tests/MiniExcel.Tests.AspNetCore)

```csharp
public class ApiController : Controller
{
    public IActionResult Index()
    {
        return new ContentResult
        {
            ContentType = "text/html",
            StatusCode = (int)HttpStatusCode.OK,
            Content = @"<html><body>
<a href='api/DownloadExcel'>DownloadExcel</a><br>
<a href='api/DownloadExcelFromTemplatePath'>DownloadExcelFromTemplatePath</a><br>
<a href='api/DownloadExcelFromTemplateBytes'>DownloadExcelFromTemplateBytes</a><br>
<p>Upload Excel</p>
<form method='post' enctype='multipart/form-data' action='/api/uploadexcel'>
    <input type='file' name='excel'> <br>
    <input type='submit' >
</form>
</body></html>"
        };
    }

    public IActionResult DownloadExcel()
    {
        var values = new[] {
            new { Column1 = "MiniExcel", Column2 = 1 },
            new { Column1 = "Github", Column2 = 2}
        };
        var memoryStream = new MemoryStream();
        memoryStream.SaveAs(values);
        memoryStream.Seek(0, SeekOrigin.Begin);
        return new FileStreamResult(memoryStream, "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet")
        {
            FileDownloadName = "demo.xlsx"
        };
    }

    public IActionResult DownloadExcelFromTemplatePath()
    {
        string templatePath = "TestTemplateComplex.xlsx";

        Dictionary<string, object> value = new Dictionary<string, object>()
        {
            ["title"] = "FooCompany",
            ["managers"] = new[] {
                new {name="Jack",department="HR"},
                new {name="Loan",department="IT"}
            },
            ["employees"] = new[] {
                new {name="Wade",department="HR"},
                new {name="Felix",department="HR"},
                new {name="Eric",department="IT"},
                new {name="Keaton",department="IT"}
            }
        };

        MemoryStream memoryStream = new MemoryStream();
        memoryStream.SaveAsByTemplate(templatePath, value);
        memoryStream.Seek(0, SeekOrigin.Begin);
        return new FileStreamResult(memoryStream, "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet")
        {
            FileDownloadName = "demo.xlsx"
        };
    }

    private static Dictionary<string, Byte[]> TemplateBytesCache = new Dictionary<string, byte[]>();

    static ApiController()
    {
        string templatePath = "TestTemplateComplex.xlsx";
        byte[] bytes = System.IO.File.ReadAllBytes(templatePath);
        TemplateBytesCache.Add(templatePath, bytes);
    }

    public IActionResult DownloadExcelFromTemplateBytes()
    {
        byte[] bytes = TemplateBytesCache["TestTemplateComplex.xlsx"];

        Dictionary<string, object> value = new Dictionary<string, object>()
        {
            ["title"] = "FooCompany",
            ["managers"] = new[] {
                new {name="Jack",department="HR"},
                new {name="Loan",department="IT"}
            },
            ["employees"] = new[] {
                new {name="Wade",department="HR"},
                new {name="Felix",department="HR"},
                new {name="Eric",department="IT"},
                new {name="Keaton",department="IT"}
            }
        };

        MemoryStream memoryStream = new MemoryStream();
        memoryStream.SaveAsByTemplate(bytes, value);
        memoryStream.Seek(0, SeekOrigin.Begin);
        return new FileStreamResult(memoryStream, "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet")
        {
            FileDownloadName = "demo.xlsx"
        };
    }

    public IActionResult UploadExcel(IFormFile excel)
    {
        var stream = new MemoryStream();
        excel.CopyTo(stream);

        foreach (var item in stream.Query(true))
        {
            // do your logic etc.
        }

        return Ok("File uploaded successfully");
    }
}
```

####  3. 分頁查詢

```csharp
void Main()
{
	var rows = MiniExcel.Query(path);
	
	Console.WriteLine("==== No.1 Page ====");
	Console.WriteLine(Page(rows,pageSize:3,page:1));
	Console.WriteLine("==== No.50 Page ====");
	Console.WriteLine(Page(rows,pageSize:3,page:50));
	Console.WriteLine("==== No.5000 Page ====");
	Console.WriteLine(Page(rows,pageSize:3,page:5000));
}

public static IEnumerable<T> Page<T>(IEnumerable<T> en, int pageSize, int page)
{
	return en.Skip(page * pageSize).Take(pageSize);
}
```

![20210419](https://user-images.githubusercontent.com/12729184/114679083-6ef4c400-9d3e-11eb-9f78-a86daa45fe46.gif)

#### 4. WebForm不落地導出Excel

```csharp
var fileName = "Demo.xlsx";
var sheetName = "Sheet1";
HttpResponse response = HttpContext.Current.Response;
response.Clear();
response.ContentType = "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet";
response.AddHeader("Content-Disposition", $"attachment;filename=\"{fileName}\"");
var values = new[] {
    new { Column1 = "MiniExcel", Column2 = 1 },
    new { Column1 = "Github", Column2 = 2}
};
var memoryStream = new MemoryStream();
memoryStream.SaveAs(values, sheetName: sheetName);
memoryStream.Seek(0, SeekOrigin.Begin);
memoryStream.CopyTo(Response.OutputStream);
response.End();
```

### FAQ 常見問題

#### Q: Excel 表頭標題名稱跟 class 屬性名稱不一致，如何對應?

A. 請使用 ExcelColumnName 作 mapping

![image](https://user-images.githubusercontent.com/12729184/116020475-eac50980-a678-11eb-8804-129e87200e5e.png)



#### Q. 多工作表(sheet)如何導出/查詢資料?

A. 使用 `GetSheetNames `方法搭配 Query 的 sheetName 參數



```csharp
var sheets = MiniExcel.GetSheetNames(path);
foreach (var sheet in sheets)
{
    Console.WriteLine($"sheet name : {sheet} ");
    var rows = MiniExcel.Query(path,useHeaderRow:true,sheetName:sheet);
    Console.WriteLine(rows);
}
```

![image](https://user-images.githubusercontent.com/12729184/116199841-2a1f5300-a76a-11eb-90a3-6710561cf6db.png)

#### Q. 查詢如何映射枚舉(enum)?

A. 名稱一樣，系統會自動映射(注意:大小寫不敏感)

![image](https://user-images.githubusercontent.com/12729184/116210595-9784b100-a775-11eb-936f-8e7a8b435961.png)

#### Q. 是否使用 Count 會載入全部數據到記憶體

不會，圖片測試一百萬行*十列資料，簡單測試，內存最大使用 < 60MB，花費13.65秒

![image](https://user-images.githubusercontent.com/12729184/117118518-70586000-adc3-11eb-9ce3-2ba76cf8b5e5.png)



### 侷限與警告

- 目前不支援 xls (97-2003) 或是加密檔案
- xlsm 只支持查詢



### 參考
- 讀取邏輯 :  [ExcelDataReader](https://github.com/ExcelDataReader/ExcelDataReader)  / [ClosedXML](https://github.com/ClosedXML/ClosedXML)
- API 設計方式 :　[StackExchange/Dapper](https://github.com/StackExchange/Dapper)    

### Contributors  

![](https://contrib.rocks/image?repo=shps951023/MiniExcel)