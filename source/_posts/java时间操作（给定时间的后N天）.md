---
title: java时间操作（给定时间的后N天）
date: 2016-10-16 21:23:04
tags: [java]
---
[文章来源:java时间操作（给定时间的后N天）](http://blog.csdn.net//u011229848//article/details/52833106)

```java
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;

public class Test {
	public static void main(String[] args) {
		System.out.println(new Test().getDateAfterNDays("2012-05-10", 7));// 输出2012-5-17
		new Test().getTimeDifference("2014-06-04 14:48:47");
	}

	/**
	 * 获取给定日期N天后的日期
	 * 
	 * @author cusn-cmh
	 */
	public String getDateAfterNDays(String dateTime, int days) {
		Calendar calendar = Calendar.getInstance();
		String[] dateTimeArray = dateTime.split("-");
		int year = Integer.parseInt(dateTimeArray[0]);
		int month = Integer.parseInt(dateTimeArray[1]);
		int day = Integer.parseInt(dateTimeArray[2]);
		calendar.set(year, month - 1, day);
		long time = calendar.getTimeInMillis();// 给定时间与1970 年 1 月 1
												// 日的00:00:00.000的差，以毫秒显示
		calendar.setTimeInMillis(time + days * 1000 * 60 * 60 * 24);// 用给定的
																	// long值设置此Calendar的当前时间值
		return calendar.get(Calendar.YEAR)// 应还书籍时间--年
				+ "-" + (calendar.get(Calendar.MONTH) + 1)// 应还书籍时间--月
				+ "-" + calendar.get(Calendar.DAY_OF_MONTH)// 应还书籍时间--日
		;
	}
	
	/**
	 * 获取给定时间与当前系统时间的差值(以毫秒为单位)
	 * 
	 * @author cusn-cmh
	 */
	public long getTimeDifference(String paramTime) {
		DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		String systemTime = new SimpleDateFormat("yyyy-MM-dd kk:mm:ss")
				.format(new Date().getTime());// 获取系统时间
		long difference = 0;
		try {
			Date systemDate = dateFormat.parse(systemTime);
			Date lockDate = dateFormat.parse(paramTime);
			difference = systemDate.getTime() - lockDate.getTime();
			System.out.println("系统时间：" + systemTime + ",给定时间：" + paramTime
					+ ",给定时间与当前系统时间的差值(以毫秒为单位)：" + difference + "毫秒");
		} catch (Exception e) {
			e.printStackTrace();
		}
		return difference;
	}

}
```