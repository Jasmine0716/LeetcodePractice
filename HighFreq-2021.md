# <a href="https://gist.github.com/Windsooon/e663358a6be45a93af2665206c4d4ae9"> High Frequency - 2021</a>
<br>

### <a href="https://leetcode.com/problems/daily-temperatures/description/">739. Daily Temperatures</a>
```Java
// Method 1: Monotonous Stack
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        Stack<Integer> stack = new Stack<>();
        Stack<Integer> idx = new Stack<>();
        int[] res = new int[temperatures.length];

        for (int i = 0; i < temperatures.length; i++) {
            if (!stack.isEmpty()) {
                while (!stack.isEmpty() && stack.peek() < temperatures[i]) {
                    stack.pop();
                    int index = idx.pop();
                    res[index] = i - index;
                }
            }

            stack.push(temperatures[i]);
            idx.push(i);
        }
        return res;
    }
}
```