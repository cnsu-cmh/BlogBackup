---
title: JAVA调用支付宝直充接口
date: 2015-11-19 23:43:21
tags: [java]
---
[文章来源:JAVA调用支付宝直充接口](http://blog.csdn.net//u011229848//article/details/49936381)

(转载)
## JAVA调用支付宝直充接口

JAVA调用支付宝的直充接口原理和银联接口一样，当商家和支付宝签好协议后就可以问支付宝的人要相关的代码了。我本地用的是支付宝UTF-8编码的代码，先把util里的东西达成一个alipay.jar文件。然后编写我们自己的调用和响应代码：
1.封装支付宝需要的接口数据
@Description("会员中心资金帐户支付宝充值")
@CreatedInfo(creator = "hamano", date = "2008-09-22")
public ModelAndView fundsDepositByAliPay(HttpServletRequest request,
HttpServletResponse response) {
RemindPageUtil remindPageUtil = new RemindPageUtil();
Map<String, Object> paraMap = new HashMap<String, Object>();
String paygateway = "https://www.alipay.com/cooperate/gateway.do?"; // '支付接口
String service = "create_direct_pay_by_user";// 快速付款交易服务
String sign_type = "MD5";
String out_trade_no = SnGenerator.generateUnionPaySn(); // 商户网站订单
String input_charset = "utf-8";
String key = ""; // 支付宝安全校验码(账户内提取)
String partner= ""; // 支付宝合作伙伴id (账户内提取)
String body = "商品描述"; // 商品描述，推荐格式：商品名称（订单编号：订单编号）
String total_fee ="0.01";// 订单总价
String payment_type = "1";// 支付宝类型.1代表商品购买
String seller_email = "pay@365.com"; // 卖家支付宝帐户
String subject = "商品名称"; // 商品名称
String show_url = "http://www/foolpid.net/product/show_product.jsp";//商品显示页
String notify_url = "http://www/foolpid.net/accountController.do?method=alipayNotify"; // 通知接收URL
String return_url = "http://www/foolpid.net/accountController.do?method=alipayReturn"; // 支付完成后跳转返回的网址URL
String ItemUrl = Payment.CreateUrl(paygateway, service, sign_type,
out_trade_no, input_charset, partner, key, show_url, body,
total_fee, payment_type, seller_email, subject, notify_url,
return_url);
//支付宝接口用POST传输，所以我自己写了个将GET转为POST的方法，代码在下面的程序中给出
HttpClient http=new HttpClient(response);
http.setParameter("body",body);
http.setParameter("notify_url",notify_url);
http.setParameter("out_trade_no",out_trade_no);
http.setParameter("partner",partner);
http.setParameter("payment_type",payment_type);
http.setParameter("return_url",return_url);
http.setParameter("seller_email",seller_email);
http.setParameter("service",service);
http.setParameter("show_url",show_url);
http.setParameter("sign",ItemUrl);
http.setParameter("sign_type",sign_type);
http.setParameter("subject",subject);
http.setParameter("total_fee",total_fee);
try {
http.sendByPost("https://www.alipay.com/cooperate/gateway.do?_input_charset=utf-8");
} catch (IOException e) {
}
return null;
}
2.根据notify_url的设置响应支付宝接口
public ModelAndView alipayNotify(HttpServletRequest request,
HttpServletResponse response) throws Exception {
RemindPageUtil remindPageUtil = new RemindPageUtil();
Map<String, Object> paraMap = new HashMap<String, Object>();
String privateKey = ""; // 支付宝安全校验码(账户内提取)
String partner= ""; // 支付宝合作伙伴id (账户内提取)
String alipayNotifyURL = "http://notify.alipay.com/trade/notify_query.do?"
+ "&partner="
+ partner
+ "¬ify_id="
+ request.getParameter("notify_id");
// 获取支付宝ATN返回结果，true是正确的订单信息，false 是无效的
String responseTxt = CheckURL.check(alipayNotifyURL);
Map params = new HashMap();
// 获得POST 过来参数设置到新的params中
Map requestParams = request.getParameterMap();
for (Iterator iter = requestParams.keySet().iterator(); iter.hasNext();) {
String name = (String) iter.next();
String[] values = (String[]) requestParams.get(name);
String valueStr = "";
for (int i = 0; i < values.length; i++) {
valueStr = (i == values.length - 1) ? valueStr + values[i]
: valueStr + values[i] + ",";
}
params.put(name, valueStr);
}
String mysign = com.alipay.util.SignatureHelper.sign(params, privateKey);
String orderId=request.getParameter("out_trade_no");
String amount=request.getParameter("total_fee");
if (mysign.equals(request.getParameter("sign"))
&&responseTxt.equals("true")) {
if("TRADE_FINISHED".equals(request.getParameter("trade_status"))){
return null;
}else{
System.out.println("支付失败");
}
} else {
System.out.println("支付失败");
}
return null;
}
3.return_url的调用和前面一致，用于支付完成后的返回页面
附注：将GET转为POST的类
public class HttpClient {
Map<String, String> parameter=new HashMap<String, String>();
HttpServletResponse response;
public HttpClient(HttpServletResponse response)
{
this.response=response;
}
public void setParameter(String key,String value)
{
this.parameter.put(key, value);
}
public void sendByPost(String url) throws IOException
{
this.response.setContentType("text/html");
response.setCharacterEncoding("utf-8");
PrintWriter out = this.response.getWriter();
out.println("<!DOCTYPE HTML PUBLIC /"-//W3C//DTD HTML 4.01 Transitional//EN/">");
out.println("<HTML>");
out.println(" <HEAD><TITLE>sender</TITLE></HEAD>");
out.println(" <BODY>");
out.println("<form name=/"submitForm/" action=/""+url+"/" method=/"post/">");
Iterator<String> it=this.parameter.keySet().iterator();
while(it.hasNext())
{
String key=it.next();
out.println("<input type=/"hidden/" name=/""+key+"/" value=/""+this.parameter.get(key)+"/"/>");
}
out.println("</from>");
out.println("<script>window.document.submitForm.submit();</script> ");
out.println(" </BODY>");
out.println("</HTML>");
out.flush();
out.close();
}
}
我 在做程序的时候碰到了一个很怪的现象，在if (mysign.equals(request.getParameter("sign"))&&responseTxt.equals("true")) {}这一句，用jsp页面的时候没有任何问题，但写在类的方法中就会出现mysign和sign不一致的情况，在网上也查过相关资料有的说是编码问题，但 我打印出回调的参数时中文数据都是对的。后来问了支付宝的工作人员，说是key和partner没写，可我写的都对的。最后他们也没给我一个合理的答案， 他们说&&用||代替好了，因为最关键的是下面 if("TRADE_FINISHED".equals(request.getParameter("trade_status"))){}这里的判 断。

所以最后我就改成了if (mysign.equals(request.getParameter("sign"))||responseTxt.equals("true")) {}，如果有谁也碰到我一样的情况，请在此留言，告知我一下答案。谢谢！！！

呵呵，我也遇到了这样的问题，不过我使用的编码都是gbk的，解决方法如下：Map requestParams = request.getParameterMap();
for (Iterator iter = requestParams.keySet().iterator(); iter.hasNext();)
{
String name = (String) iter.next();
String[] values = (String[]) requestParams.get(name);
String valueStr = "";
for (int i = 0; i &lt; values.length; i++)
{
valueStr = (i == values.length - 1) ? valueStr + values[i]: valueStr + values[i] + ",";
valueStr=new String (valueStr.getBytes("ISO-8859-1"),"gbk"); //转utf-8
}
params.put(name, valueStr);
}
你可以参考一下，不知道可不可以