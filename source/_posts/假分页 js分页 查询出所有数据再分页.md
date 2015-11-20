---
title: 假分页 js分页 查询出所有数据再分页
date: 2016-04-14 10:04:59
tags: [java,假分页]
---
[文章来源:假分页 js分页 查询出所有数据再分页](http://blog.csdn.net/u011229848/article/details/51149199)


版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/u011229848/article/details/51149199

假分页 js分页 查询出所有数据再分页

//jsp页面代码
```html
<%@page import="com.xqx.fcch.model.TDevCompanyInfo"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@	taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<c:set var="ctx" value="${pageContext.request.contextPath}"/>
<%@ taglib prefix="tags" tagdir="/WEB-INF/tags"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<!--引入BootStrap-->
    <link rel="stylesheet" href="${ctx}/resources/bootstrap-3.3.0/dist/css/bootstrap.min.css">
    <script src="${ctx}/resources/js/jquery-1.10.2.min.js"></script>
    <script src="${ctx}/resources/bootstrap-3.3.0/dist/js/bootstrap.min.js"></script>
</head>
<body>
	<div>
		<table  class="table ">
		<tr align="center" >
				<td >
				<form id="search" action="${ctx}/admin/devCompany/searchFromFC" method="post">
				公司名称: <input name=companyName value="${companyName}" id="companyName"/>  
				公司地址: <input name="companyAddress" value="${companyAddress}" id="companyAddress" />  
				营业执照号: <input name="businessLicenseCode" value="${businessLicenseCode}" id="businessLicenseCode"/>  
				<input type="hidden" name="size"> 
				<input type="hidden" name="page">
				<input type="button" value="查找" class="btn btn-primary btn-sm btn-info" onclick="check()">
				</form>
				</td>
			</tr>
		</table>
		<table  class="table table-bordered table-hover" id="devCompanyInfos">
			<thead>
			<tr class="info" >
				<th>公司名称</th>
				<th>联系电话(座机)</th> 
				<th>公司地址</th>
				<th>公司法人</th>
				<th>营业执照号</th>
				<th>注册资本</th>
				<th>资质证书编号</th>
				<th>发证机关</th>
				<th>公司邮箱</th>
				<th>公司传真</th>
				<th>操作员</th>
			 	<th>分配</th>
			</tr>
			</thead>
			<c:forEach items="${devCompanyInfos }" var="s" >
			<tr style="font-size: 13px;">
				<td>${s.companyName }</td>
				<td>${s.telePhone}</td>
				<td>${s.companyAddress}</td>
				<td>${s.legalPerson}</td>
				<td>${s.businessLicenseCode}</td>
				<td>${s.registeredCapital}</td>
				<td>${s.incNum}</td>
				<td>${s.authority}</td>
				<td>${s.mail}</td>
				<td>${s.fax}</td>
				<td>${s.operator}</td>
				<c:if test="${s.isAssign}">
				<td>已分配</td>
				</c:if>
				<c:if test="${s.isAssign==false}">
				<td><input  class="btn btn-primary btn-sm btn-info"  type="button" value="分配" id="assign" 
				onclick="assign('${s.id}');"/></td>
				</c:if>
			 </tr>
			</c:forEach>
		</table>
		</div>
	
		<div class="gridItem" style="padding: 5px; width: 300px; float: left; text-align: left; height: 20px; font-size: 15px;" > 
		    	共有 <span id="spanTotalInfor"></span> 条记录	 
		    	当前第<span id="spanPageNum"></span>页	 
		    	共<span id="spanTotalPage"></span>页
		</div>
  		<div class="gridItem" style="margin-left:50px;  padding: 5px; width: 400px; float: right; text-align: center; height: 20px; vertical-align: middle; font-size: 15px;">   
      		<span id="spanFirst" >首页</span> 
      		<span id="spanPre">上一页</span>
      		<span id="spanInput" style="margin: 0px; padding: 0px 0px 4px 0px; height:100%; "> 
        		第<input id="Text1" type="text" class="TextBox" onkeyup="changepage()"   style="height:24px; text-align: center;width:30px ;" />页 
      		</span>
      		<span id="spanNext">下一页</span> 
      		<span  id="spanLast">尾页</span> 
		</div>
		    	
		<script type="text/javascript">
		      var theTable = document.getElementById("devCompanyInfos");//Id选择对了就OK！   ${devCompanyInfos }是所有后台返回数据
		      var txtValue = document.getElementById("Text1").value;
		      function changepage() {
		        var txtValue2 = document.getElementById("Text1").value;
		        if (txtValue != txtValue2) {
		          if (txtValue2 > pageCount()) {
		          }
		          else if (txtValue2 <= 0) {
		          }
		          else if (txtValue2 == 1) {
		            first();
		          }
		          else if (txtValue2 == pageCount()) {
		            last();
		          }
		          else {
		            hideTable();
		            page = txtValue2;
		            pageNum2.value = page;
		            currentRow = pageSize * page;
		            maxRow = currentRow - pageSize;
		            if (currentRow > numberRowsInTable) currentRow = numberRowsInTable;
		            for (var i = maxRow; i < currentRow; i++) {
		              theTable.rows[i].style.display = '';
		            }
		            if (maxRow == 0) { preText(); firstText(); }
		            showPage();
		            nextLink();
		            lastLink();
		            preLink();
		            firstLink();
		           }
		          txtValue = txtValue2;
		        }
			}
  		</script>
  		<!-- 引入分页js -->
  		<script type="text/javascript" src="${ctx}/resources/js/page/pagging.js"></script>
		
	</body>
</html>
```

pagging.js 源码
```javascript
//获取对应控件
var totalPage = document.getElementById("spanTotalPage");//总页数
var pageNum = document.getElementById("spanPageNum");//当前页
var totalInfor = document.getElementById("spanTotalInfor");//记录总数
var pageNum2 = document.getElementById("Text1");//当前页文本框
var spanPre = document.getElementById("spanPre");//上一页
var spanNext = document.getElementById("spanNext");//下一页
var spanFirst = document.getElementById("spanFirst");//首页
var spanLast = document.getElementById("spanLast");//尾页
var pageSize = 8;//每页信息条数
var numberRowsInTable = theTable.rows.length-1;//表格最大行数
var page = 1;
//下一页
function next() {
  if (pageCount() <= 1) {
  }
  else {
      hideTable();
      currentRow = pageSize * page + 1; //下一页的第一行
      maxRow = currentRow + pageSize;	//下一页的一行
      if (maxRow > numberRowsInTable) maxRow = numberRowsInTable+1;//如果计算中下一页最后一行大于实际最后一行行号
      for (var i = currentRow; i < maxRow; i++) {
        theTable.rows[i].style.display = '';
      }
      page++;
      pageNum2.value = page;
      if (maxRow == numberRowsInTable) { //如果下一页的最后一行是表格的最后一行
        nextNoLink(); //下一页 和尾页 不点击
        lastNoLink(); 
      }
      showPage();//更新page显示
      if (page == pageCount()) {//如果当前页是尾页
        nextNoLink();
        lastNoLink();
      }
      preLink();
      firstLink();
    }
  }
//上一页
function pre() {
  if (pageCount() <= 1) {
  }
  else {	   
      hideTable();
      page--;
      pageNum2.value = page;
      currentRow = pageSize * page + 1;//下一页的第一行
      maxRow = currentRow - pageSize;//本页的第一行
      if (currentRow > numberRowsInTable) currentRow = numberRowsInTable;//如果计算中本页的第一行小于实际首页的第一行行号，则更正
      for (var i = maxRow; i < currentRow; i++) {
        theTable.rows[i].style.display = '';
      }
      if (maxRow == 0) { preNoLink(); firstNoLink(); }
      showPage();//更新page显示
      if (page == 1) {
        firstNoLink();
        preNoLink();
      }
      nextLink();
      lastLink();
    }
  }
//第一页
function first() {
  if (pageCount() <= 1) {
  }
  else {
    hideTable();//隐藏所有行
    page = 1;
    pageNum2.value = page;
    for (var i = 1; i < pageSize+1; i++) {//显示第一页的信息
      theTable.rows[i].style.display = '';
    }
    showPage();//设置当前页

    firstNoLink();
    preNoLink();
    nextLink();
    lastLink();
  }
}
//最后一页
function last() {
  if (pageCount() <= 1) {
  }
  else {
    hideTable();//隐藏所有行
    page = pageCount();//将当前页设置为最大页数
    pageNum2.value = page;
    currentRow = pageSize * (page - 1)+1;//获取最后一页的第一行行号
    for (var i = currentRow; i < numberRowsInTable+1; i++) {//显示表格中最后一页信息
      theTable.rows[i].style.display = '';
    }
    showPage();
    lastNoLink();
    nextNoLink();
    preLink();
    firstLink();
  }
}
function hideTable() {//隐藏表格内容
  for (var i = 0; i < numberRowsInTable+1; i++) {
    theTable.rows[i].style.display = 'none';
  }
  theTable.rows[0].style.display = '';//标题栏显示
}
function showPage() {//设置当前页数
  pageNum.innerHTML = page;
}
function inforCount() {//设置总记录数
  spanTotalInfor.innerHTML = numberRowsInTable;
}
//总共页数
function pageCount() {
  var count = 0;
  if (numberRowsInTable % pageSize != 0) count = 1;
  return parseInt(numberRowsInTable / pageSize) + count;
}
//显示链接 link方法将相应的文字变成可点击翻页的  nolink方法将对应的文字变成不可点击的
function preLink() { spanPre.innerHTML = "<a href='javascript:pre();'>上一页</a>"; }
function preNoLink(){ spanPre.innerHTML = "上一页"; }
function nextLink() { spanNext.innerHTML = "<a href='javascript:next();'>下一页</a>"; }
function nextNoLink(){ spanNext.innerHTML = "下一页";}
function firstLink() { spanFirst.innerHTML = "<a href='javascript:first();'>首页</a>"; }
function firstNoLink(){ spanFirst.innerHTML = "首页";}
function lastLink() { spanLast.innerHTML = "<a href='javascript:last();'>尾页</a>"; }
function lastNoLink(){ spanLast.innerHTML = "尾页";}
//初始化表格
function hide() {
  for (var i = pageSize + 1; i < numberRowsInTable+1 ; i++) {
    theTable.rows[i].style.display = 'none';
  }
  theTable.rows[0].style.display = '';
  inforCount();
  totalPage.innerHTML = pageCount();
  showPage();
  pageNum2.value = page;
  if (pageCount() > 1) {
    nextLink();
    lastLink();
  }
}
hide();
```

注意jsp页面分页下面JavaScript代码中注释 

```javascript
var theTable = document.getElementById("devCompanyInfos");//Id选择对了就OK！   ${devCompanyInfos }是所有后台返回数据
```