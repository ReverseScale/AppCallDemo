# AppCommunicationDemo
Inter-application communication value example

# 简单 A-B 示例
## DemoA
### 设置 URL Types

![](https://user-gold-cdn.xitu.io/2018/3/30/16275b7956e2682c?w=665&h=182&f=png&s=26852)

### 设置 Info.plist
```Json
<key>LSApplicationQueriesSchemes</key>
<array>
	<string>Demob</string>
</array>
```
![](https://user-gold-cdn.xitu.io/2018/3/30/16275c16ab2bf8da?w=752&h=36&f=png&s=13743)

### AppDelegate 中实现
```objc
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString *,id> *)options {
    NSLog(@"%@", url);
    return YES;
}
```

### UI 效果
![](https://user-gold-cdn.xitu.io/2018/3/30/16275b995bce3c0b)

### 创建 UITextField 控件
```objc
@property (weak, nonatomic) IBOutlet UITextField *textFild;
```

### 发送响应事件
```objc
- (IBAction)action:(id)sender {
    //打开B应用,有B应用计算结果,并且返回
    NSURL *url = [NSURL URLWithString:[NSString stringWithFormat:@"Demob://%@", _textFild.text]];//"b"为B应用的url schemes
    if ([[UIApplication sharedApplication] canOpenURL:url]) {
        //        [[UIApplication sharedApplication] openURL:url];
        [[UIApplication sharedApplication] openURL:url options:@{} completionHandler:^(BOOL success) {
            
        }];
    }
}
```

## DemoB
### 设置 URL Types
![](https://user-gold-cdn.xitu.io/2018/3/30/16275c273c4973cf?w=579&h=158&f=png&s=25467)

### 设置 Info.plist
```Json
<key>LSApplicationQueriesSchemes</key>
<array>
	<string>Demoa</string>
</array>
```
![](https://user-gold-cdn.xitu.io/2018/3/30/16275c04b3971aa0?w=606&h=38&f=png&s=13596)

### AppDelegate 中实现
```objc
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString *,id> *)options {
    NSLog(@"%@", url);
    NSString *urlString = url.absoluteString;
    NSArray *reuslt = [urlString componentsSeparatedByString:@"://"];
    if (reuslt.count > 1) {
        NSString *number = reuslt.lastObject;
        NSInteger resltIng = number.integerValue * 2;
        
        NSURL *url = [NSURL URLWithString:[NSString stringWithFormat:@"Demoa://%ld", resltIng]];
        //        [[UIApplication sharedApplication] openURL:url];
        [[UIApplication sharedApplication] openURL:url options:@{} completionHandler:^(BOOL success) {
            
        }];
    }
    return YES;
}
```
