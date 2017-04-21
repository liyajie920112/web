> 给excel单元格设置样式

```c#
/// <summary>
/// 单元格样式
/// </summary>
/// <param name="wb"></param>
/// <returns></returns>
public ICellStyle GetCellStyle(IWorkbook wb)
{
    ICellStyle style = wb.CreateCellStyle();
    style.WrapText = true;//换行
    style.Alignment = HorizontalAlignment.Center;//水平居中
    style.VerticalAlignment = VerticalAlignment.Center;//垂直居中
    style.BorderTop = BorderStyle.Thin;//设置上边框,实心线
    style.BorderLeft = BorderStyle.Thin;//设置下边框
    style.BorderRight = BorderStyle.Thin;
    style.BorderBottom = BorderStyle.Thin;
    return style;

}
```

> 如何使用单元格样式
```c#
cell.CellStyle = style;//style就是ICellStyle,cell就是ICell
```
其他样式自己试

> 使用NPOI设置excel单元格字体

```c#
/// <summary>
/// 设置字体
/// </summary>
/// <param name="wb">IWorkbook对象</param>
/// <param name="fontSize">字体大小</param>
/// <param name="fontName">字体名称</param>
/// <param name="isBold">是否加粗</param>
/// <returns></returns>
public IFont GetFont(IWorkbook wb, string fontName = "宋体", bool isBold = true, short fontSize = 11)
{
    IFont font = wb.CreateFont();
    font.FontHeightInPoints = fontSize;
    font.FontName = fontName;
    font.IsBold = isBold;
    return font;
}
```
> 如何使用字体
```C#
style.SetFont(font);//font就是IFont,style就是ICellStyle
```

其他字体样式自己去试

> excel合并单元格

```c#
// 第0行到第0行,第0列到第10列进行合并
//public CellRangeAddress(int firstRow, int lastRow, int firstCol, int lastCol);
sheet.AddMergedRegion(new NPOI.SS.Util.CellRangeAddress(0, 0, 0, 10));
```

> NPOI设置打印相关
```c#
sheet.PrintSetup.PaperSize = (short)PaperSize.A4_Small;//设置打印纸张大小
sheet.FitToPage = false;//设置成不自适应页面
sheet.RepeatingRows = new NPOI.SS.Util.CellRangeAddress(0, 0, 0, 10);//设置打印头
sheet.PrintSetup.Landscape = true;//横向打印
//设置页边距为1 对应excel中的2.5,0.04对应excel中的0.1
double left = 0.04 * 15;
double right = 0.04 * 15;
double top = 0.04 * 19;
double bottom = 0.04 * 19;
double header = 0.04 * 8;
double footer = 0.04 * 8;
sheet.SetMargin(MarginType.LeftMargin, left);//设置左页边距
sheet.SetMargin(MarginType.RightMargin, right);//设置右页边距
sheet.SetMargin(MarginType.TopMargin, top);//设置上页边距
sheet.SetMargin(MarginType.BottomMargin, bottom);//设置下页边距
sheet.SetMargin(MarginType.HeaderMargin, header);//设置页眉
sheet.SetMargin(MarginType.FooterMargin, footer);//设置页脚
sheet.HorizontallyCenter = true;//设置打印水平居中
sheet.PrintSetup.PageStart = 1;//设置开始页码
sheet.Footer.Center = "—&P—";//设置打印页脚,&P代表当前页码,到第二页的时候&P就是2 &D当前日期
```
> NPOI导出图片到Excel
```c#
string FileName = fileurl;//图片地址
// 把图片转换成字节数组
byte[] bytes = System.IO.File.ReadAllBytes(FileName);
if (!string.IsNullOrEmpty(FileName))
{
    int pictureIdx = wb.AddPicture(bytes, NPOI.SS.UserModel.PictureType.JPEG);
    HSSFPatriarch patriarch = (HSSFPatriarch)sheet.CreateDrawingPatriarch();

    //控制图片位置
    HSSFClientAnchor anchor = new HSSFClientAnchor(30, 20, 1000, 0, col, row, col, row + 3);
    HSSFPicture pict = (HSSFPicture)patriarch.CreatePicture(anchor, pictureIdx);

    //pict.Resize();//这是用图片原始大小来显示
}
```