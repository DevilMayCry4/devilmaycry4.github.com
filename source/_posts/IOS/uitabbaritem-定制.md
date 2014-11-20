title: UITabbarItem 定制
date: 2013-10-26 14:04:47
tags: IOS
---
<p>
设置item的标题颜色：
</p>

        [[UITabBarItem appearance] setTitleTextAttributes:@{ UITextAttributeTextColor : [UIColor grayColor] }
                                                 forState:UIControlStateNormal];
        [[UITabBarItem appearance] setTitleTextAttributes:@{ UITextAttributeTextColor : [UIColor redColor] }
                                                 forState:UIControlStateSelected];


<p>
设置选中和没有选中时候都图片：
</p>

//for ios7 before
- (void)setFinishedSelectedImage:(UIImage *)selectedImage withFinishedUnselectedImage:(UIImage *)unselectedImage ;

//for ios7 and later
//tips:设置图片的时候要用
//[[UIImage imageNamed:@"ImageName"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal]
- (instancetype)initWithTitle:(NSString *)title image:(UIImage *)image selectedImage:(UIImage *)selectedImage;


<p>
ios7 去掉默认都选中颜色
</p>


   self.tabBar.tintColor = [UIColor clearColor];


<p>
ios7之前去掉选中的时候都mask方块
</p>


//empty.png 是一张透明都png图
     self.tabBar.selectionIndicatorImage = [UIImage imageNamed:@"empty.png"];
     self.tabBar.selectedImageTintColor = [UIColor clearColor];
