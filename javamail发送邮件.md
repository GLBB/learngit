---
title: javamail发送邮件
date: 2018-01-31 20:46:54
tags:
---
1. 打开QQ邮件
选择设置
选择账户
找到 POP3/IMAP/SMTP/Exchange/CardDAV/CalDAV服务
开启POP3/SMTP服务
记住授权码
编写代码
导入mail.jar
```
import java.security.GeneralSecurityException;
import java.util.Properties;

import javax.mail.Message;
import javax.mail.MessagingException;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.AddressException;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;

import com.sun.mail.util.MailSSLSocketFactory;

public class MailTest {
	public static void main(String[] args) throws AddressException, MessagingException, GeneralSecurityException {
		Properties properties = new Properties();
		// 开启debug调试，以便在控制台查看
		properties.setProperty("mail.debug", "true");
		// 设置邮件传输协议
		properties.setProperty("mail.transport.protocol", "smtp");
		// 设置邮件服务器主机名
		properties.setProperty("mail.host", "smtp.qq.com");
		// 发送服务器需要身份验证
		properties.setProperty("mail.smtp.auth", "true");
		
		// 开启SSL加密，否则会失败
		MailSSLSocketFactory factory = new MailSSLSocketFactory();
		factory.setTrustAllHosts(true);
		properties.put("mail.smtp.ssl.enable", "true");
		properties.put("mail.smtp.ssl.socketFactory", factory);
		
		// 创建session
		Session session = Session.getInstance(properties);
		// 通过session得到transport对象
		Transport transport = session.getTransport();
		// 连接邮件服务器：邮箱类型，帐号，授权码代替密码（更安全）
		transport.connect("smtp.qq.com","123456789","sofjsfsksjfl");
		// 123456789是QQ账号  sofjsfsksjfl 这个是授权码
		
		// 创建邮件对象
		Message message = new MimeMessage(session);
		// 指明邮件的发件人
		message.setFrom(new InternetAddress("123456789@qq.com"));
		// 指明邮件的收件人
		message.setRecipient(Message.RecipientType.TO, new InternetAddress("1811560185@qq.com"));
		// 邮件的标题
		message.setSubject("JavaMail第4次测试");
		// 邮件的文本内容
		message.setContent("<h1>Java测试发送邮件成功</h1>","text/html;charset=UTF-8");
		// 发送邮件
		transport.sendMessage(message, message.getAllRecipients());
		transport.close();
		
	}

}
```
当报 unkown host smtp.qq.com 时
可以在cmd ping smtp.qq.com ，看网络是否联通
参考链接<a href="http://www.cnblogs.com/xyzq/p/5704358.html">[参考1]
<a href="http://www.codes51.com/itwd/2880376.html">[参考二]




