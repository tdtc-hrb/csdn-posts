---
title: "Chain of Command 模式"
description: "java实现"
date: 2023-12-22T12:08:08+08:00
---

我们知道Chain of Responsibility模式，其实就是Template method + Next。

  在进行多个调用时，按顺序执行；需要调用2个方法。

  此模式，只适合抽象方法！

  在进行设计模式的程序实现中xiaobin使用已编写的SSL程序做演示。

![chain of command](https://github.com/tdtc-hrb/csdn/raw/master/images/chain-of-command.png)

## Chain of Command
```
import com.cartionsoft.xbf.ContentXbf;
import com.cartionsoft.xbf.LibXbf;
import com.cartionsoft.xbf.ReadXbf;
import com.cartionsoft.xbf.XiaobinFile;

public class Demo {

	public static void init() throws Exception {
		int stationID = 0;
	    String keyPWD;

		String libPath = "xbfLibR.dll";
		String xbfPath = "C://send//dySSL.xbf";

		ContentXbf contentXbf = new ContentXbf(xbfPath);

        // All checks are linked. Client can build various chains using the same
        // components.
        XiaobinFile xiaobinFile = XiaobinFile.link(
            new LibXbf(libPath),
            new ReadXbf()
        );

        // Server gets a chain from client code.
        contentXbf.setXbf(xiaobinFile);
        
        stationID = contentXbf.getStationID(-1);
        keyPWD = contentXbf.getKeyPWD(-1);
        
        System.out.println(keyPWD + " : " + stationID);
	}

	public static void main(String[] args) throws Exception {
		// TODO Auto-generated method stub
		init();
	}

}
```
- template-method(abstract)
```
readRecord()
```
- Next
```
readRecordNext()
```
- traverse
```
link()
```
### Next
```
	/**
     * Runs readRecord on the next object in chain or ends traversing if we're in
     * last object in chain.
     */
    protected String readRecordNext(int dimRecord, String filename) {
        if (next == null) {
            return "";
        }
        return next.readRecord(dimRecord, filename);
    }
```

### link
```
    /**
     * Builds chains of XiaobinFile objects.
     */
    public static XiaobinFile link(XiaobinFile first, XiaobinFile... chain) {
    	XiaobinFile head = first;
        for (XiaobinFile nextInChain: chain) {
            head.next = nextInChain;
            head = nextInChain;
        }
        return first;
    }
```
