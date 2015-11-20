---
title: 使用truelicense实现用于JAVA工程license机制
date: 2017-10-11 10:59:35
tags: [java]
---
[文章来源:使用truelicense实现用于JAVA工程license机制](http://blog.csdn.net/u011229848/article/details/78201292)


**Keytool是一个Java数据证书的管理工具。**

keystore

Keytool将密钥（key）和证书（certificates）存在一个称为keystore的文件中

在keystore里，包含两种数据： 密钥实体（Key entity）——密钥（secret key）又或者是私钥和配对公钥（采用非对称加密）

可信任的证书实体（trusted certificate entries）——只包含公钥

Alias（别名）

每个keystore都关联这一个独一无二的alias，这个alias通常不区分大小写

keystore的存储位置

在没有制定生成位置的情况下，keystore会存在与用户的系统默认目录，

如：对于window 或xp系统，会生成在系统的***C:\Documents and Settings\UserName\***

文件名为“XXXkey.store”

keystore的生成

使用keytool生成密钥对

以下命令在dos命令行执行，注意当前执行目录，最后生成的密钥对即在该目录下：

1、首先要用KeyTool工具来生成私匙库：（-alias别名 –validity 3650表示10年有效）

keytool -genkey -alias privatekey -keystoreprivateKeys.store -validity 3650

2、然后把私匙库内的公匙导出到一个文件当中：
```
keytool -export -alias privatekey -file certfile.cer -keystore privateKeys.store
```
3、然后再把这个证书文件导入到公匙库：
```
keytool -import -alias publiccert -file certfile.cer -keystore publicCerts.store
```
最后生成文件privateKeys.store、publicCerts.store拷贝出来备用。

**生成证书**
```java
package cn.melina.license;
import de.schlichtherle.license.LicenseManager;
import de.schlichtherle.license.LicenseParam;

/**
 * LicenseManager容器类
 * @author melina
 */
public class LicenseManagerHolder {
	
	private static LicenseManager licenseManager;
 
	public static LicenseManager getLicenseManager(LicenseParam licenseParams) {
    	if (licenseManager == null) {
    		synchronized(LicenseManager.class){
    			if (licenseManager == null) {
    				licenseManager = new LicenseManager(licenseParams);
    			}
    		}
    		
    	}
    	return licenseManager;
    }
}
```
```java
package cn.melina.license;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Properties;
import java.util.prefs.Preferences;
import javax.security.auth.x500.X500Principal;
import de.schlichtherle.license.CipherParam;
import de.schlichtherle.license.DefaultCipherParam;
import de.schlichtherle.license.DefaultKeyStoreParam;
import de.schlichtherle.license.DefaultLicenseParam;
import de.schlichtherle.license.KeyStoreParam;
import de.schlichtherle.license.LicenseContent;
import de.schlichtherle.license.LicenseParam;
import de.schlichtherle.license.LicenseManager;

/**
 * CreateLicense
 * @author melina
 */
public class CreateLicense {
	//common param
	private static String PRIVATEALIAS = "";
	private static String KEYPWD = "";
	private static String STOREPWD = "";
	private static String SUBJECT = "";
	private static String licPath = "";
	private static String priPath = "";
	//license content
	private static String issuedTime = "";
	private static String notBefore = "";
	private static String notAfter = "";
	private static String consumerType = "";
	private static int consumerAmount = 0;
	private static String info = "";
	// 为了方便直接用的API里的例子
	// X500Princal是一个证书文件的固有格式，详见API
	private final static X500Principal DEFAULTHOLDERANDISSUER = new X500Principal(
			"CN=Duke、OU=JavaSoft、O=Sun Microsystems、C=US");
	
	public void setParam(String propertiesPath) {
		// 获取参数
		Properties prop = new Properties();
		InputStream in = getClass().getResourceAsStream(propertiesPath);
		try {
			prop.load(in);
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		PRIVATEALIAS = prop.getProperty("PRIVATEALIAS");
		KEYPWD = prop.getProperty("KEYPWD");
		STOREPWD = prop.getProperty("STOREPWD");
		SUBJECT = prop.getProperty("SUBJECT");
		KEYPWD = prop.getProperty("KEYPWD");
		licPath = prop.getProperty("licPath");
		priPath = prop.getProperty("priPath");
		//license content
		issuedTime = prop.getProperty("issuedTime");
		notBefore = prop.getProperty("notBefore");
		notAfter = prop.getProperty("notAfter");
		consumerType = prop.getProperty("consumerType");
		consumerAmount = Integer.valueOf(prop.getProperty("consumerAmount"));
		info = prop.getProperty("info");
		
	}

	public boolean create() {		
		try {
			/************** 证书发布者端执行 ******************/
			LicenseManager licenseManager = LicenseManagerHolder
					.getLicenseManager(initLicenseParams0());
			licenseManager.store((createLicenseContent()), new File(licPath));	
		} catch (Exception e) {
			e.printStackTrace();
			System.out.println("客户端证书生成失败!");
			return false;
		}
		System.out.println("服务器端生成证书成功!");
		return true;
	}

	// 返回生成证书时需要的参数
	private static LicenseParam initLicenseParams0() {
		Preferences preference = Preferences
				.userNodeForPackage(CreateLicense.class);
		// 设置对证书内容加密的对称密码
		CipherParam cipherParam = new DefaultCipherParam(STOREPWD);
		// 参数1,2从哪个Class.getResource()获得密钥库;参数3密钥库的别名;参数4密钥库存储密码;参数5密钥库密码
		KeyStoreParam privateStoreParam = new DefaultKeyStoreParam(
				CreateLicense.class, priPath, PRIVATEALIAS, STOREPWD, KEYPWD);
		LicenseParam licenseParams = new DefaultLicenseParam(SUBJECT,
				preference, privateStoreParam, cipherParam);
		return licenseParams;
	}

	// 从外部表单拿到证书的内容
		public final static LicenseContent createLicenseContent() {
			DateFormat format = new SimpleDateFormat("yyyy-MM-dd");
			LicenseContent content = null;
			content = new LicenseContent();
			content.setSubject(SUBJECT);
			content.setHolder(DEFAULTHOLDERANDISSUER);
			content.setIssuer(DEFAULTHOLDERANDISSUER);
			try {
				content.setIssued(format.parse(issuedTime));
				content.setNotBefore(format.parse(notBefore));
				content.setNotAfter(format.parse(notAfter));
			} catch (ParseException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			content.setConsumerType(consumerType);
			content.setConsumerAmount(consumerAmount);
			content.setInfo(info);
			// 扩展
			content.setExtra(new Object());
			return content;
		}
}
```
```java
package cn.melina.license;

public class licenseVerifyTest {
	public static void main(String[] args){
		VerifyLicense vLicense = new VerifyLicense();
		//获取参数
		vLicense.setParam("./param.properties");
		//生成证书
		boolean verify = vLicense.verify();
		System.out.println(verify);
	}
}

```
生成时使用到的param.properties文件如下:
```
##########common parameters###########
#alias
PRIVATEALIAS=privatekey
#key(important!)
KEYPWD=PASSWORD
#STOREPWD
STOREPWD=cmh199410
#SUBJECT
SUBJECT=bigdata
#licPath
licPath=bigdata.lic
#priPath
priPath=privateKeys.store
##########license content###########
#issuedTime
issuedTime=2017-10-01
#notBeforeTime
notBefore=2017-10-01
#notAfterTime
notAfter=2017-10-15
#consumerType
consumerType=user
#ConsumerAmount
consumerAmount=1
#info
info=this is a license
```
根据properties文件可以看出，这里只简单设置了使用时间的限制，当然可以自定义添加更多限制。该文件中表示授权者拥有私钥，并且知道生成密钥对的密码。并且设置license的内容。

四、第三步：验证证书（使用证书）（该部分代码结合需要授权的程序使用）

1、 首先LicenseManagerHolder.java类，同上。

2、 然后是主要验证license的代码VerifyLicense.java：
```java
package cn.melina.license;
   
   import java.io.File;
   import java.io.IOException;
   import java.io.InputStream;
   import java.util.Properties;
   import java.util.prefs.Preferences;
   
   import de.schlichtherle.license.CipherParam;
   import de.schlichtherle.license.DefaultCipherParam;
   import de.schlichtherle.license.DefaultKeyStoreParam;
   import de.schlichtherle.license.DefaultLicenseParam;
   import de.schlichtherle.license.KeyStoreParam;
   import de.schlichtherle.license.LicenseParam;
   import de.schlichtherle.license.LicenseManager;
   
   /**
    * VerifyLicense
    * @author melina
    */
   public class VerifyLicense {
   	//common param
   	private static String PUBLICALIAS = "";
   	private static String STOREPWD = "";
   	private static String SUBJECT = "";
   	private static String licPath = "";
   	private static String pubPath = "";
   	
   	public void setParam(String propertiesPath) {
   		// 获取参数
   		Properties prop = new Properties();
   		InputStream in = getClass().getResourceAsStream(propertiesPath);
   		try {
   			prop.load(in);
   		} catch (IOException e) {
   			// TODO Auto-generated catch block
   			e.printStackTrace();
   		}
   		PUBLICALIAS = prop.getProperty("PUBLICALIAS");
   		STOREPWD = prop.getProperty("STOREPWD");
   		SUBJECT = prop.getProperty("SUBJECT");
   		licPath = prop.getProperty("licPath");
   		pubPath = prop.getProperty("pubPath");
   	}
   
   	public boolean verify() {		
   		/************** 证书使用者端执行 ******************/
   
   		LicenseManager licenseManager = LicenseManagerHolder
   				.getLicenseManager(initLicenseParams());
   		// 安装证书
   		try {
   			licenseManager.install(new File(licPath));
   			System.out.println("客户端安装证书成功!");
   		} catch (Exception e) {
   			e.printStackTrace();
   			System.out.println("客户端证书安装失败!");
   			return false;
   		}
   		// 验证证书
   		try {
   			licenseManager.verify();
   			System.out.println("客户端验证证书成功!");
   		} catch (Exception e) {
   			e.printStackTrace();
   			System.out.println("客户端证书验证失效!");
   			return false;
   		}
   		return true;
   	}
   
   	// 返回验证证书需要的参数
   	private static LicenseParam initLicenseParams() {
   		Preferences preference = Preferences
   				.userNodeForPackage(VerifyLicense.class);
   		CipherParam cipherParam = new DefaultCipherParam(STOREPWD);
   
   		KeyStoreParam privateStoreParam = new DefaultKeyStoreParam(
   				VerifyLicense.class, pubPath, PUBLICALIAS, STOREPWD, null);
   		LicenseParam licenseParams = new DefaultLicenseParam(SUBJECT,
   				preference, privateStoreParam, cipherParam);
   		return licenseParams;
   	}
   }
```

```java
package cn.melina.license;

public class licenseVerifyTest {
	public static void main(String[] args){
		VerifyLicense vLicense = new VerifyLicense();
		//获取参数
		vLicense.setParam("./param.properties");
		//生成证书
		boolean verify = vLicense.verify();
		System.out.println(verify);
	}
}

```
验证时使用到的Properties文件如下：

```
##########common parameters###########
#alias
PUBLICALIAS=publiccert
#STOREPWD
STOREPWD=PASSWORD
#SUBJECT
SUBJECT=bigdata
#licPath
licPath=bigdata.lic
#pubPath
pubPath=publicCerts.store
```
bigdata.lic创建license时生成的，复制放到验证的项目中去

注意实际操作中，公钥、私钥、证书等文件的存放路径。

文章参考 http://blog.csdn.net/luckymelina/article/details/22870665

本人代码地址（最代码地址）：[http://www.zuidaima.com/share/3585793691339776.htm](http://www.zuidaima.com/share/3585793691339776.htm)