# Bit Manipulation Cheat Sheet

## Important Operations

| Operation      | Code          |
| -------------- | ------------- | -------- |
| Set kth bit    | n             | (1 << k) |
| Clear kth bit  | n & ~(1 << k) |
| Toggle kth bit | n ^ (1 << k)  |
| Check kth bit  | (n >> k) & 1  |

## Tricks

- Remove rightmost set bit  
  `n & (n-1)`

- Check if power of two  
  `n & (n-1) == 0`
