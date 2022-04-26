---
title: JavaSe-Day-01
categories: 学习
tags: JavaSe
---

### Scanner

next() 不能有空白符，不然空白符后不显示

nextLine() 可以有空白符

### 方法重载

- 方法的名称必须相同
- 参数列表必须不同
- 方法的返回类型可以相同，也可以不同

### 递归

```java
public class Demo4 {
    public static void main(String[] args) {
        System.out.println(a(5));
    }
    public static int a(int j){
        if (j==1){
            return 1;
        }else {
            return j*a(j-1);
        }
    }
}
```

### 增强for循环

```java
public class Demo5 {
    public static void main(String[] args) {
        int[] arr = {1,2343,432,524,23,};
        for (int arra:arr){
            System.out.println(arra);
        }
    }
}
```

### 冒泡排序

```java
public class Demo6 {
    static int temp =0;
    public static void main(String[] args) {
        int[] rw={23,123,435,234};

        int [] wd=px(rw);
        System.out.println(Arrays.toString(wd));
    }
    public static int[] px(int[] arr){
        for (int i =0;i<arr.length-1;i++){
            for (int j=0;j<arr.length-1-i;j++){
                if (arr[j+1]>arr[j]){
                    temp=arr[j];
                    arr[j]=arr[j+1];
                    arr[j+1]=temp;
                }
            }
        }


        return arr;
    }
}
```

