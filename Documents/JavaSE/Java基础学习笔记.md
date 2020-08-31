## 输出乘法表

```java
public static void main(String[] args) {
        for(int i=9;i>0;i--) {
            for(int j=1,k=10-i;j<=i;j++,k++) {
                System.out.print(j+"×"+k+"="+k*j+"\t");// \t: 水平制表位，固定占8位
            }
            System.out.println();
            }
        }
```

### 做功能的套路

- 先写行为/方法:
  - 若为某个对象所特有的行为，就将方法设计在特定的类中
  - 若为所有对象所共有的行为，就将方法设计在超类中



