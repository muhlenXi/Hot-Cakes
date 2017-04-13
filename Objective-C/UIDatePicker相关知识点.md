### UIDatePicker相关知识点

#### String 转 Date

```objc
NSString * time = @"00:00";
NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];
[dateFormatter setDateFormat: @"HH:mm"];
NSDate *date= [dateFormatter dateFromString:time];
```

#### 1、初始化一个 datePicker

```objc
self.datePicker = [[UIDatePicker alloc] initWithFrame:CGRectMake(0, 100, 320, 200)];
self.datePicker.date = self.currentDate;
self.datePicker.datePickerMode = UIDatePickerModeDateAndTime;
// 设置时区，中国在东八区
self.datePicker.timeZone = [NSTimeZone timeZoneWithName:@"GTM+8"]; 
// 添加事件
[self.datePicker addTarget:self action:@selector(datepickeValueChanged:) forControlEvents:UIControlEventValueChanged];
```

#### 2、获取datePicker的时间

```objc
- (void)clockValueChanged:(id) sender
{
    UIDatePicker * picker = (UIDatePicker *) sender;
    NSDateFormatter * pickerFormatter = [[NSDateFormatter alloc]init];
    [pickerFormatter setDateFormat:@"yyyy-MM-dd HH:mm:ss"];
    NSString * timeStr = [pickerFormatter stringFromDate:picker.date];
    //打印显示日期时间
    NSLog(@"选择的时间是：%@",timeStr); 
}
```

#### 3、获取当前的时间（日期+时间+周）

```objc
- (void) getCurrentTime
{
    NSDate * currentDate = [NSDate date];
    NSCalendar * calendar = [NSCalendar calendarWithIdentifier:NSCalendarIdentifierGregorian];
    NSDateComponents * components = [[NSDateComponents alloc] init];
    NSUInteger unitFlags = NSCalendarUnitYear|NSCalendarUnitMonth|NSCalendarUnitDay|NSCalendarUnitHour|NSCalendarUnitMinute|NSCalendarUnitSecond|NSCalendarUnitWeekday;
    components = [calendar components:unitFlags fromDate:currentDate];
    
    NSInteger year = [components year];
    NSInteger month = [components month];
    NSInteger day = [components day];
    NSInteger hour = [components hour];
    NSInteger minute = [components minute];
    NSInteger second = [components second];
    NSInteger weekday = [components weekday];
    
    //对星期进行处理
    weekday =  weekday - 1 == 0 ? 7 : weekday - 1;
    
    NSLog(@"今天是：%ld年%ld月%ld日%ld时%ld分%ld秒 星期%ld",year,month,day,hour,minute,second,weekday);
}
```

#### 4、获取前几天或后几天的时间

```objc
- (void) getNumerDaysBeforeOrAfter:(NSInteger) number
{
    
    NSDate * agoDate = [[NSDate alloc] initWithTimeIntervalSinceNow:24 * 3600 * number];
    NSCalendar * calendar = [NSCalendar calendarWithIdentifier:NSCalendarIdentifierGregorian];
    NSDateComponents * components = [[NSDateComponents alloc] init];
    NSUInteger unitFlags = NSCalendarUnitYear|NSCalendarUnitMonth|NSCalendarUnitDay|NSCalendarUnitHour|NSCalendarUnitMinute|NSCalendarUnitSecond|NSCalendarUnitWeekday;
    components = [calendar components:unitFlags fromDate:agoDate];
    
    NSInteger year = [components year];
    NSInteger month = [components month];
    NSInteger day = [components day];
    
    NSInteger hour = [components hour];
    NSInteger minute = [components minute];
    NSInteger second = [components second];
    NSInteger weekday = [components weekday];
    
    //对星期进行处理
    weekday =  weekday - 1 == 0 ? 7 : weekday - 1;
    
    NSLog(@"第 %ld 天是：%ld年%ld月%ld日%ld时%ld分%ld秒 星期%ld",number,year,month,day,hour,minute,second,weekday);
}
```

#### 5、计算两个时间点之间的差值

```objc
NSCalendar *calendar = [NSCalendar currentCalendar];
// 需要对比的时间数据
NSCalendarUnit unit = NSCalendarUnitYear | NSCalendarUnitMonth| NSCalendarUnitDay | NSCalendarUnitHour | NSCalendarUnitMinute | NSCalendarUnitSecond;
        
// 对比时间差
NSDateComponents *dateCom = [calendar components:unit fromDate:self.sleepDate toDate:self.sackDate options:0];
NSString * timeStr = [NSString stringWithFormat:@"%ld-%ld-%ld %ld:%ld:%ld",dateCom.year,dateCom.month,dateCom.day,dateCom.hour,dateCom.minute,dateCom.second];
NSLog(@"时间差 timeStr == %@",timeStr);
```