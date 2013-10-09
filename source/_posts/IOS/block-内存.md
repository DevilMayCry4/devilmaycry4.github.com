title: Block 内存
date: 2013-10-09 17:31:15
tags: IOS
---
<p>
block调用自身在前面添加__block关键字
</p>
{% codeblock lang:objc %}
- (IBAction)grabURLInBackground:(id)sender
{
   NSURL *url = [NSURL URLWithString:@"http://allseeing-i.com"];
   __block ASIHTTPRequest *request = [ASIHTTPRequest requestWithURL:url];
   [request setCompletionBlock:^{
      // Use when fetching text data
      NSString *responseString = [request responseString];
 
      // Use when fetching binary data
      NSData *responseData = [request responseData];
   }];
   [request setFailedBlock:^{
      NSError *error = [request error];
   }];
   [request startAsynchronous];
}
{% endcodeblock %}