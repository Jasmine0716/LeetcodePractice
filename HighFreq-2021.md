# <a href="https://gist.github.com/Windsooon/e663358a6be45a93af2665206c4d4ae9"> High Frequency - 2021</a>
</br>

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
</br>

### <a href="https://leetcode.com/problems/flatten-nested-list-iterator/description/">341. Flatten Nested List Iterator</a>
#### Method 1: Stack + Recursion

```Java
// Method 1: Stack + Recursion
// Time Complexity: O(n)
// Space Complexity: O(h)

/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return empty list if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
public class NestedIterator implements Iterator<Integer> {
    Stack<List<NestedInteger>> listStack;
    Stack<Integer> idxStack;

    public NestedIterator(List<NestedInteger> nestedList) {
        listStack = new Stack<>();
        idxStack = new Stack<>();
        listStack.push(nestedList);
        idxStack.push(0);
    }

    @Override
    public Integer next() {
        Integer res = listStack.peek().get(idxStack.peek()).getInteger();
        idxStack.push(idxStack.pop()+1);
        return res;
    }

    @Override
    public boolean hasNext() {
        while(idxStack.peek() >= listStack.peek().size()){
            System.out.println(idxStack.peek()+" "+listStack.peek().size());
            idxStack.pop();
            listStack.pop();
            if(idxStack.isEmpty()) return false;
            idxStack.push(idxStack.pop()+1);
        }

        while (!listStack.peek().get(idxStack.peek()).isInteger()) {
            List<NestedInteger> list = listStack.peek().get(idxStack.peek()).getList();
            if(list.size() > 0){
                listStack.push(listStack.peek().get(idxStack.peek()).getList());
                idxStack.push(0);
            }else{
                idxStack.push(idxStack.pop()+1);
                return this.hasNext();
            }
        }
        return true;
    }
}

/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i = new NestedIterator(nestedList);
 * while (i.hasNext()) v[f()] = i.next();
 */
```

#### **Method 2: Stack + Iterator
```Java
// Method 2: Stack + Iteractor
// Idea:
//      1. Make every element the same type: Change Integer into List
//      2. Store iteractor into the stack, we we can know if there's a next element.
//      next: get the element directly
//      hasNext: change into the right position + change Integer into List
// Time Complexity: O()
// Space Complexity: O()

/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return empty list if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
public class NestedIterator implements Iterator<Integer> {
    Stack<Iterator<NestedInteger>> stack;

    public NestedIterator(List<NestedInteger> nestedList) {
        stack = new Stack<>();
        stack.push(nestedList.iterator());
    }

    @Override
    public Integer next() {
        // get the element directly
        return stack.peek().next().getInteger();
    }

    @Override
    public boolean hasNext() {
        // change into the right position + change Integer into List
        while(!stack.isEmpty()){
            Iterator<NestedInteger> it = stack.peek();
            if(!it.hasNext()){
                stack.pop();
                continue;
            }
            NestedInteger next = it.next();
            if(next.isInteger()) {
                List<NestedInteger> list = new ArrayList<>();
                list.add(next);
                stack.push(list.iterator());
                return true;
            }
            stack.push(next.getList().iterator());
        }
        return false;
    }
}
```
</br>

### <a href="https://leetcode.com/problems/decode-string/description/">394. Decode String</a>

#### Method 1: Stack
```Java
class Solution {
    public String decodeString(String s) {
        Stack<Character> stack = new Stack<>();

        for(int i = 0; i < s.length(); i++) {
            Character c = s.charAt(i);
            if(c == ']') {
                List<Character> list = new ArrayList<>();
                while(stack.peek() != '[') {
                    list.add(stack.pop());
                }
                stack.pop();
                int cnt = 0;
                int p = 1;
                while(!stack.isEmpty() && Character.isDigit(stack.peek())) {
                    cnt = cnt + p * (stack.pop() - '0');
                    p *= 10;
                }

                for(int j = 0; j < cnt; j++) {
                    for(int m = list.size() - 1; m >= 0; m--){
                        stack.push(list.get(m));
                    }
                }
            }else{
                stack.push(c);
            }
        }

        int l = stack.size();
        char[] res = new char[l];
        while(--l >= 0){
            res[l] = stack.pop();
        }
        return new String(res);
    }
}
```