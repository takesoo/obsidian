## what
- Arrange(準備), Act(実行), Assert(確認)３フェーズに分けて記述すること
```c#
// Arrange
var guid = new Guid("01234567-89ab-cdef-0123-456789abcdef");

// Act
var actual = guid.ToString("D");

// Assert
Assert.AreEqual("01234567-89ab-cdef-0123-456789abcdef", actual);

```
## why
- テストコードの可読性向上