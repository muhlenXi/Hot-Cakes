## UITextField 粘贴

指定粘贴前前 n 个字符，比如总共允许输入20个字符，现在输入框中有15个字符，现在待粘贴的文本有8个字符，则取前5个字符，剩下的丢弃。

```swift
public extension UITextField {
    /// 粘贴指定前n个字符
    ///
    /// - Parameters:
    ///   - range: 替换区间
    ///   - pastedString: 待粘贴的字符串
    ///   - prefixNumber: 最多粘贴的长度
    func pastedPrefixNumberText(range: NSRange, pastedString: String, prefixNumber: Int) {
        guard prefixNumber >= 0, prefixNumber <= pastedString.count else {
            return
        }
        let oldText = self.text ?? ""
        var cursorPositionIndex: Int = prefixNumber
        let newText = (oldText as NSString).replacingCharacters(in: range, with: String(pastedString.prefix(prefixNumber)))
        if let selectedRange = self.selectedTextRange {
            let oldCursorPositionIndex = self.offset(from: self.beginningOfDocument, to: selectedRange.start)
            cursorPositionIndex += oldCursorPositionIndex
        }
        self.text = newText
        if let newPosition = self.position(from: self.beginningOfDocument, in: UITextLayoutDirection.right, offset: cursorPositionIndex) {
            DispatchQueue.main.async {
                self.selectedTextRange = self.textRange(from: newPosition, to: newPosition)
            }
        }
    }
}
```