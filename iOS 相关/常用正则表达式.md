### 常用正则表达式

#### 简单举例

```objc
NSString * text = @"muhlenXi545";   //要验证的字符串
NSString * pattern = @"[0-9A-Z]{8}-[0-9A-Z]{4}-[0-9A-Z]{4}-[0-9A-Z]{4}-[0-9A-Z]{12}";
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"SELF MATCHES %@",pattern];
BOOL isMatch = [predicate evaluateWithObject:text];
if (isMatch) {
    
    [AppDelegate mainDelegate].selectedItem.uuidStr = [XYJTools deleteMinusToUUIDStr:text];
    [self.navigationController popViewControllerAnimated:YES];
    
} else {
    UIAlertView * alert = [[UIAlertView alloc] initWithTitle:nil message:@"请按规范输入 UUID" delegate:nil cancelButtonTitle:@"确定" otherButtonTitles:nil, nil];
    [alert show];
}

```

####  1-只包括字母和数字（一般用于验证用户名）
	
	^[A-Za-z0-9]+$
	   
####  2-UUID (适用于大写字母)	   
	 
    [0-9A-Z]{8}-[0-9A-Z]{4}-[0-9A-Z]{4}-[0-9A-Z]{4}-[0-9A-Z]{12}  
		
####  只包括数字
	
	 ^[0-9]*$
		
####  整数

    ^-?\\d+$

