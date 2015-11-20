---
title: SpringMvc 使用poi导入导出Excel
date: 2016-03-30 10:38:58
tags: [java,spring]
---
[文章来源:SpringMvc 使用poi导入导出Excel](http://blog.csdn.net/u011229848/article/details/51012282)


```java
/**Util类*/ 
package com.common.util;  
  
public class ExportUtil  
{  
    private XSSFWorkbook wb = null;  
  
    private XSSFSheet sheet = null;  
  
    /** 
     * @param wb 
     * @param sheet 
     */  
    public ExportUtil(XSSFWorkbook wb, XSSFSheet sheet)  
    {  
        this.wb = wb;  
        this.sheet = sheet;  
    }  
  
    /** 
     * 合并单元格后给合并后的单元格加边框 
     *  
     * @param region 
     * @param cs 
     */  
    public void setRegionStyle(CellRangeAddress region, XSSFCellStyle cs)  
    {  
  
        int toprowNum = region.getFirstRow();  
        for (int i = toprowNum; i <= region.getLastRow(); i++)  
        {  
            XSSFRow row = sheet.getRow(i);  
            for (int j = region.getFirstColumn(); j <= region.getLastColumn(); j++)  
            {  
                XSSFCell cell = row.getCell(j);// XSSFCellUtil.getCell(row,  
                                                // (short) j);  
                cell.setCellStyle(cs);  
            }  
        }  
    }  
  
    /** 
     * 设置表头的单元格样式 
     *  
     * @return 
     */  
    public XSSFCellStyle getHeadStyle()  
    {  
        // 创建单元格样式  
        XSSFCellStyle cellStyle = wb.createCellStyle();  
        // 设置单元格的背景颜色为淡蓝色  
        cellStyle.setFillForegroundColor(HSSFColor.PALE_BLUE.index);  
        cellStyle.setFillPattern(XSSFCellStyle.SOLID_FOREGROUND);  
        // 设置单元格居中对齐  
        cellStyle.setAlignment(XSSFCellStyle.ALIGN_CENTER);  
        // 设置单元格垂直居中对齐  
        cellStyle.setVerticalAlignment(XSSFCellStyle.VERTICAL_CENTER);  
        // 创建单元格内容显示不下时自动换行  
        cellStyle.setWrapText(true);  
        // 设置单元格字体样式  
        XSSFFont font = wb.createFont();  
        // 设置字体加粗  
        font.setBoldweight(XSSFFont.BOLDWEIGHT_BOLD);  
        font.setFontName("宋体");  
        font.setFontHeight((short) 200);  
        cellStyle.setFont(font);  
        // 设置单元格边框为细线条  
        cellStyle.setBorderLeft(XSSFCellStyle.BORDER_THIN);  
        cellStyle.setBorderBottom(XSSFCellStyle.BORDER_THIN);  
        cellStyle.setBorderRight(XSSFCellStyle.BORDER_THIN);  
        cellStyle.setBorderTop(XSSFCellStyle.BORDER_THIN);  
        return cellStyle;  
    }  
  
    /** 
     * 设置表体的单元格样式 
     *  
     * @return 
     */  
    public XSSFCellStyle getBodyStyle()  
    {  
        // 创建单元格样式  
        XSSFCellStyle cellStyle = wb.createCellStyle();  
        // 设置单元格居中对齐  
        cellStyle.setAlignment(XSSFCellStyle.ALIGN_CENTER);  
        // 设置单元格垂直居中对齐  
        cellStyle.setVerticalAlignment(XSSFCellStyle.VERTICAL_CENTER);  
        // 创建单元格内容显示不下时自动换行  
        cellStyle.setWrapText(true);  
        // 设置单元格字体样式  
        XSSFFont font = wb.createFont();  
        // 设置字体加粗  
        font.setBoldweight(XSSFFont.BOLDWEIGHT_BOLD);  
        font.setFontName("宋体");  
        font.setFontHeight((short) 200);  
        cellStyle.setFont(font);  
        // 设置单元格边框为细线条  
        cellStyle.setBorderLeft(XSSFCellStyle.BORDER_THIN);  
        cellStyle.setBorderBottom(XSSFCellStyle.BORDER_THIN);  
        cellStyle.setBorderRight(XSSFCellStyle.BORDER_THIN);  
        cellStyle.setBorderTop(XSSFCellStyle.BORDER_THIN);  
        return cellStyle;  
    }  
}  
service类  
public interface ITestExportExcelService  
{  
    public void exportExcel(String hql,String [] titles,ServletOutputStream outputStream);  
}  
@Service  
public class TestExportExcelServiceImpl implements ITestExportExcelService  
{  
    @Resource  
    private ITestExportExcelDao dao;  
      
    public void exportExcel(String hql, String[] titles, ServletOutputStream outputStream)  
    {  
        List<Goods> list = dao.exportExcel(hql);  
        // 创建一个workbook 对应一个excel应用文件  
        XSSFWorkbook workBook = new XSSFWorkbook();  
        // 在workbook中添加一个sheet,对应Excel文件中的sheet  
        XSSFSheet sheet = workBook.createSheet("导出excel例子");  
        ExportUtil exportUtil = new ExportUtil(workBook, sheet);  
        XSSFCellStyle headStyle = exportUtil.getHeadStyle();  
        XSSFCellStyle bodyStyle = exportUtil.getBodyStyle();  
        // 构建表头  
        XSSFRow headRow = sheet.createRow(0);  
        XSSFCell cell = null;  
        for (int i = 0; i < titles.length; i++)  
        {  
            cell = headRow.createCell(i);  
            cell.setCellStyle(headStyle);  
            cell.setCellValue(titles[i]);  
        }  
        // 构建表体数据  
        if (list != null && list.size() > 0)  
        {  
            for (int j = 0; j < list.size(); j++)  
            {  
                XSSFRow bodyRow = sheet.createRow(j + 1);  
                Goods goods = list.get(j);  
  
                cell = bodyRow.createCell(0);  
                cell.setCellStyle(bodyStyle);  
                cell.setCellValue(goods.getGoodsName());  
  
                cell = bodyRow.createCell(1);  
                cell.setCellStyle(bodyStyle);  
                cell.setCellValue(goods.getGoodsCost());  
  
                cell = bodyRow.createCell(2);  
                cell.setCellStyle(bodyStyle);  
                cell.setCellValue(goods.getGoodsUnit());  
            }  
        }  
        try  
        {  
            workBook.write(outputStream);  
            outputStream.flush();  
            outputStream.close();  
        }  
        catch (IOException e)  
        {  
            e.printStackTrace();  
        }  
        finally  
        {  
            try  
            {  
                outputStream.close();  
            }  
            catch (IOException e)  
            {  
                e.printStackTrace();  
            }  
        }  
  
    }  
  
}  
dao类  
public interface ITestExportExcelDao  
{  
    public List<Goods> exportExcel(String hql);  
}  
@Repository  
public class TestExportExcelDaoImpl implements ITestExportExcelDao  
{  
    @Resource  
    private SessionFactory factory;  
      
    /** 
     * 以goods表为例导出测试 
     */  
    @SuppressWarnings("unchecked")  
    public List<Goods> exportExcel(String hql)  
    {  
        Session session = factory.getCurrentSession();  
        List<Goods> list = session.createQuery(hql).list();  
        return list;  
    }  
  
}  
控制层Controller  
@Controller  
@RequestMapping("/testexportexcel/*")  
public class TestExportExcelController  
{  
    @Resource  
    private ITestExportExcelService service;  
  
    @RequestMapping  
    public String exportExcel(HttpServletResponse response)  
    {  
        response.setContentType("application/binary;charset=ISO8859_1");  
        try  
        {  
            ServletOutputStream outputStream = response.getOutputStream();  
            String fileName = new String(("导出excel例子").getBytes(), "ISO8859_1");  
            response.setHeader("Content-disposition", "attachment; filename=" + fileName + ".xlsx");// 组装附件名称和格式  
            String hql = "from Goods";  
            String[] titles = { "商品名", "商品单价", "商品单位" };  
            service.exportExcel(hql, titles, outputStream);  
        }  
        catch (IOException e)  
        {  
            e.printStackTrace();  
        }  
        return null;  
    }  
  
    @RequestMapping  
    public String upload(HttpServletRequest request, HttpServletResponse response)  
    {  
        MultipartHttpServletRequest mulRequest = (MultipartHttpServletRequest) request;  
        MultipartFile file = mulRequest.getFile("excel");  
        String filename = file.getOriginalFilename();  
        if (filename == null || "".equals(filename))  
        {  
            return null;  
        }  
        try  
        {  
            InputStream input = file.getInputStream();  
            XSSFWorkbook workBook = new XSSFWorkbook(input);  
            XSSFSheet sheet = workBook.getSheetAt(0);  
            if (sheet != null)  
            {  
                for (int i = 1; i < sheet.getPhysicalNumberOfRows(); i++)  
                {  
                    XSSFRow row = sheet.getRow(i);  
                    for (int j = 0; j < row.getPhysicalNumberOfCells(); j++)  
                    {  
                        XSSFCell cell = row.getCell(j);  
                        String cellStr = cell.toString();  
                        System.out.print("【"+cellStr+"】 ");  
                    }  
                    System.out.println();  
                }  
  
            }  
        }  
        catch (Exception e)  
        {  
            e.printStackTrace();  
        }  
        return "/test/uploadExcel.jsp";  
    }  
  
}
```