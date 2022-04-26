---
title: JavaEE-Day-02
categories: 学习
tags: JavaEE
---

## String类

- charAt()

```java
//下标输出

public class Stringchart {
    public static void main(String[] args) {
        String s ="zty";
        System.out.println(s.charAt(1));
    }
}
```

- toLowerCase()
- toUpperCase()

```java
 //将字符串小写          toLowerCase() 
 //将字符串大写          toUpperCase()

public class Stringchart {
    public static void main(String[] args) {
        String s ="zty";
        String s1="ZTY";
        System.out.println(s.toLowerCase());
        System.out.println(s.toUpperCase());
    }
}
```

- substring()

```java
//下标截取

public class Stringchart {
    public static void main(String[] args) {
        String s2 = "seugherjgerh";
        System.out.println(s2.substring(3));
        System.out.println(s2.substring(3,6));
    }
}
```

- indexOf()

```java
//字符串首次出现的下标        lastindexOf()   从后往前

public class Stringchart {
    public static void main(String[] args) {
        String s2 = "seugherjgerh";
        System.out.println(s2.indexOf("r"));
    }
}
```

- replace()

```java
//字符串替换

public class Stringchart {
    public static void main(String[] args) {
        String s2 = "seugherjgerh";
        System.out.println(s2.replace("s","z"));             //将某个字符替换
        System.out.println(s2.replace(s2,"z"));              //将将字符替换
    }
}
```

- split()

```java
//以指定字符分割数组

public class Stringchart {
    public static void main(String[] args) {
        String s2 = "zty,zty,zty,zty";
        String [] s=s2.split(",");
        for (int i=0;i<s.length;i++){
            System.out.println(s[i]);
        }
    }
}

```

- 基本数据类型与包装类的转换

```java
public class Stringchart {
    public static void main(String[] args) {
        String s2 = "123";
        int i = Integer.parseInt(s2);         //String类转换为int类
        System.out.println(i+i);
        String s = String.valueOf(i);         //int转换为String类
        System.out.println(s+s);
    }
}

```

- String 与char[]转换---toCharArray()

```java
public class Stringchart {
    public static void main(String[] args) {
        String s2 = "123";
        char[] chars = s2.toCharArray();              //String转化为char[]
        for (int i = 0; i < chars.length; i++) {
            System.out.println(chars[i]);
        }
                String s = new String(chars);         //char[]转换为String
        System.out.println(s);
    }
}
```

- String 与byte[]转换---getBytes()

```
public class Stringchart {
    public static void main(String[] args) {
        String s2 = "123";
        byte[] bytes = s2.getBytes();
        for (int i = 0; i < bytes.length; i++) {
            System.out.println(bytes[i]);
        }
    }
}
```

### StringBuffer

```java
public class Stringchart {
    public static void main(String[] args) {
        StringBuffer stringBuffer =new StringBuffer();
        stringBuffer.append("23534645");               //添加
        System.out.println(stringBuffer);
        stringBuffer.insert(0,"张天宇");                //指定位置插入
        System.out.println(stringBuffer);
        stringBuffer.deleteCharAt(3);                  //删除
        System.out.println(stringBuffer);
        stringBuffer.replace(3,5,"张天宇");             //替换
        System.out.println(stringBuffer);
    }
}
// 还有一些和String类一样的方法

//StringBuilder也一样
```

