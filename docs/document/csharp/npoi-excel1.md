> 首先要知道Excel中的几个概念

* 工作簿 (IWorkbook)
* 工作表 (ISheet)
* 行 (IRow)
* 列 (ICell)

> 整体流程, 5步

1. 实例化工作簿
2. 通过工作薄来获取工作表
3. 通过工作表来获取行
4. 通过行来获取列
5. 通过列来获取单元格的内容

> 区分excel的格式,是.xls还是.xlsx

如果是.xls我们实例化`IWorkbook`的时候要用 `HSSFWorkbook`
如果是.xlsx我们实例化`IWorkbook`的时候要用 `XSSFWorkbook`
```c#
IWorkbook wk;
using (FileStream fs = File.OpenRead(excelpath))
{
    if (Path.GetExtension(excelpath) == ".xls")
    {
        wk = new HSSFWorkbook(fs);
    }
    else
    {
        wk = new XSSFWorkbook(fs);
    }
    return wk;
}
```
`excelpath`是excel在服务器中的路径

> 工作表就是`excel`下面的选项卡,每一个选项卡就是一个工作表

我们通过IWorkbook来获取工作表
```c#
//方式一,通过工作表名字
IWorkbook wk = new HSSFWorkbook(fs);
ISheet sheet = wk.GetSheet(sheetname);
```
```c#
//方式二,通过工作表索引,从0开始
IWorkbook wk = new HSSFWorkbook(fs);
ISheet sheet = wk.GetSheetAt(0);
```
提供的方法不少, 其他的自己体验

> 通过工作表来获取行

```C#
// 通过索引来获取行
IRow row = sheet.GetRow(index);
// 获取总行数(最后一行编号)
int rowcount = sheet.LastRowNum;
```

> 通过行来获取列

```c#
// 通过索引来获取
ICell cell = row.GetCell(index);
// 获取总列数
int colcount = row.LastCellNum;
// 获取单元格的内容
string content = cell.ToString();
// 如果单元格内容是日期格式
string date = cell.DateCellValue.ToString("yyyy/MM/dd");
```

> 读取excel中的图片,使用`IWorkbook`对象

```c#
var pics = wk.GetAllPictures();
var len = pics.Count;
if(len == 0) { return "没有图片"; }
else
{
    // 取到第index张图片, 如果是.xlsx格式的excel需要用`XSSFWorkbook`进行强转
    IPictureData pic = (HSSFPictureData)pics[index];
}
```

因为项目中用到了这些功能,所以就研究了一下

[NPOI开源网站http://npoi.codeplex.com/](http://npoi.codeplex.com/)

[NPOI Nuget地址](http://www.nuget.org/packages/NPOI/)

[NPOI下载地址](http://npoi.codeplex.com/releases/)
