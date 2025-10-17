---
title: "模板方法模式"
description: "java实现"
date: 2023-12-08T12:08:08+08:00
---

我们知道模板方法模式，其实就是等待被实现的方法。

  在进行多个可选择调用时，可以根据不同的方式；只调用一个方法即可解决不同的实现。

  此模式，只适合抽象方法！

  在进行设计模式的程序实现中xiaobin使用已编写的SSL程序做演示。

![template method](https://github.com/tdtc-hrb/cnblogs/raw/master/images/template-method.png)

## Template Method
```
import java.io.*;
import java.util.*;
import javax.net.ssl.*;

public abstract class Transmission {

	protected String host;
	protected int port;
	protected String keyPath;
	protected String keyPwd;
	
	private SSLSocket socket;
	
	protected abstract SSLSocket createSocket() throws Exception;
	
	/**
	 * <p> Receive byte array data </p>
	 * @return data
	 */
	public byte[] receiveData() throws Exception {
		// TODO Auto-generated method stub
		socket = createSocket();
		
		InputStream in = new BufferedInputStream(
				socket.getInputStream());
		int b;
		List<Byte> listBuffer = new ArrayList<Byte>();
		while((b = in.read()) != -1) {
			listBuffer.add((byte) b);
		}
		
		int i = 0;
		byte[] data = new byte[listBuffer.size()];
		for (Byte byte1 : listBuffer) {
			data[i] = byte1;
			i++;
		}

		in.close();
		
		return data;
	}
	
	/**
	 * <p> Send byte array data </p>
	 * @param data
	 */
	public void sendData(byte[] data) throws Exception {
		socket = createSocket();
		OutputStream out = new BufferedOutputStream(
				socket.getOutputStream());
		out.write(data);
		out.flush();
		out.close();
	}
}
```
template-method(abstract):
```
createSocket()
```

### BC(BouncyCastle)
```
import java.io.FileInputStream;

import java.security.KeyStore;
import java.security.Security;

import javax.net.ssl.SSLContext;
import javax.net.ssl.SSLSocket;
import javax.net.ssl.SSLSocketFactory;
import javax.net.ssl.TrustManagerFactory;

import org.bouncycastle.jcajce.provider.BouncyCastleFipsProvider;
import org.bouncycastle.jsse.provider.BouncyCastleJsseProvider;

import com.cartionsoft.ssl.trans.Transmission;

public class TransBc extends Transmission {
	
	/**
	 * Create Socket with BC
	 * 
	 * @param keyPath
	 *        key store file path
	 * @param keyPWD
	 *        key store file password
	 */
	public 
	SSLSocket createSocket() throws Exception {
		// TODO Auto-generated method stub
		Security.addProvider(new BouncyCastleFipsProvider());
		Security.addProvider(new BouncyCastleJsseProvider());
		
		SSLContext sslContext = SSLContext.getInstance("TLS", "BCJSSE");
		
		TrustManagerFactory trustMgrFact = TrustManagerFactory.getInstance("PKIX", "BCJSSE");
		
		KeyStore ks = KeyStore.getInstance("JKS");
		
		char[] pwd = keyPwd.toCharArray();
		ks.load(new FileInputStream(keyPath), pwd);
		
		trustMgrFact.init(ks);
		
		sslContext.init(null, trustMgrFact.getTrustManagers(), null);
		SSLSocketFactory fact = sslContext.getSocketFactory();
		
		SSLSocket clientSocket = (SSLSocket) fact.createSocket(host, port);
		
		String[] enableList = clientSocket.getEnabledCipherSuites();
		clientSocket.setEnabledCipherSuites(enableList);
		
		return clientSocket;
	}

}
```

### Sun
```
import javax.net.ssl.SSLSocket;
import javax.net.ssl.SSLSocketFactory;

import com.cartionsoft.ssl.trans.Transmission;

public class TransSun extends Transmission {

	@Override
	public SSLSocket createSocket() throws Exception {
		SSLSocketFactory clientFactory = (SSLSocketFactory) SSLSocketFactory.getDefault();
		SSLSocket clientSocket = (SSLSocket) clientFactory.createSocket(host, port);
		
		String[] enableList = clientSocket.getEnabledCipherSuites();
		clientSocket.setEnabledCipherSuites(enableList);
		
		return clientSocket;
	}

}
```
