title: runtime
date: 2014-04-10 14:32:55
tags: IOS
---
<p>
关联对象
</p>
{% codeblock lang:objc %}
static void *alertKey = "alertKey";
typedef void (^Block)(int);

UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"" message:@"test" delegate:self cancelButtonTitle:@"取消" otherButtonTitles:@"确定", nil];
  
    
      Block block  = ^(NSInteger index ){
        if (index == 0)
        {
          NSLog(@"1");
        }
        else
        {
            NSLog(@"2");
        }
    };

    objc_setAssociatedObject(alert, alertKey, block, OBJC_ASSOCIATION_COPY);
    [alert show];
    
    
{% endcodeblock %}

{% codeblock lang:objc %}
- (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex
{
     Block block = objc_getAssociatedObject(alertView, alertKey);
    block(buttonIndex);
}
{% endcodeblock %}