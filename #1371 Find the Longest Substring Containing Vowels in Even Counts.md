[toc]



```java
class Solution {
    public int findTheLongestSubstring(String s) {
        if (s == null || s.isEmpty()) return 0;
        char[] str = s.toCharArray();

        int[][] record = new int[str.length + 1][5];
        for (int i = 0; i < str.length; i++) {
            int idx = isVowel(str[i]);
            if (idx != -1) record[i + 1][idx] = record[i][idx] + 1;

            for (int j = 0; j < 5; j++) {
                if (j == idx) continue;
                record[i + 1][j] = record[i][j];
            }
        }

        int maxLen = 0;
        for (int i = 0; i <= str.length; i++) {
            for (int j = 0; j <= i; j++) {
                boolean flag = true;
                for (int k = 0; k < 5; k++) {
                    if ((record[i][k] - record[j][k]) % 2 == 1) {
                        flag = false;
                        break;
                    }
                }
                if (flag) {
                    maxLen = Math.max(maxLen, i - j);
                    break;
                }
            }
        }
        return maxLen;
    }

    private int isVowel(char c) {
        switch (c) {
            case 'a':
                return 0;
            case 'e':
                return 1;
            case 'i':
                return 2;
            case 'o':
                return 3;
            case 'u':
                return 4;
            default:
                return -1;
        }
    }
}
```

