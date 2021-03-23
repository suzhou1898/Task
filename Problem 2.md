 Problem 2:

```java
class Solution {
 public static int reverse(String s){
        int num = Integer.parseInt(s);
        if (num == 0){
            return 0;
        }
        StringBuilder sb = new StringBuilder();
        for (int i=0; i<32; i++){
            int tmp = (num >> i) & 1;
            sb.append(tmp);
        }
        String newStr = new String(sb);
        int index = -1;
        for (int i=0; i<newStr.length(); i++){
            if (newStr.charAt(i) == '1'){
                index = i;
            }
        }
        int row = index / 8;
        String str = newStr.substring(0, (row + 1) * 8);
        int numZero = (3 - row) * 8;
        String zero = "";
        while (numZero > 0){
            zero += 0;
            numZero--;
        }
        String f = zero + str;
        int sum = 0;
        for (int i=0; i<f.length(); i++){
            int temp = f.charAt(i) - '0';
            sum = (sum << 1) + temp;
        }
        return sum;
    }
}
```

