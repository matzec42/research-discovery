## Arrays and Objects Problems

### 72. Edit Distance

- Find the minimum number of operations to convert word1 into word2 (allowed operations: insert a char, delete a char, replace a char)
- Logically --- think through if word2 (target word) is an empty string. You'd have to **delete** all letters (call it 'i') in word1. If word1 is empty, you'd need to **insert** j characters (call insertions 'j').