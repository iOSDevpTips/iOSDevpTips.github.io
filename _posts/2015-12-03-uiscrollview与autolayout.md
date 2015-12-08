---
layout: post
title: "UIScrollView与Autolayout"
date: 2015-12-03
categories: AutoLayout
tags: [AutoLayout]


---

最近遇到AutoLayout下UIScrollView滑不动的情况，why?

非AutoLayout情况下，可以直接对contentSize属性赋值，但autolayout下contentSize只能通过添加的约束来确定。

根据Apple的文档：UIScrollView And Autolayout，有两种做法:

* 混合方式
* 纯Autolayout

###混合方式
用xib创建scrollview(启用AutoLayout),用代码创建scrollview的子视图（contentView的 translatesAutoresizingMaskIntoConstraints属性默认是YES;）,并设置scrollview的contentSize.示例代码如下：

	- (void)viewDidLoad {
	UIView *contentView;
	contentView = [[UIView alloc] initWithFrame:CGRectMake(0,0,contentWidth,contentHeight)];
	[scrollView addSubview:contentView];
	// DON'T change contentView's translatesAutoresizingMaskIntoConstraints,
	// which defaults to YES;
	 
	// Set the content size of the scroll view to match the size of the content view:
	[scrollView setContentSize:CGSizeMake(contentWidth,contentHeight)];
	 
	 /* the rest of your code here... */
	}
	
###纯Autolayout方式
* 所有涉及到的view的translatesAutoresizingMaskIntoConstraints属性设为No;
* 根据scrollView添加的约束（外部的）来放置scrollview;
* 对scrollview的子视图添加约束。

#####注意事项：子视图的size不能依赖scrollView(scrollView的size由内容决定，子视图的size依赖scrollView会导致矛盾？); 内容视图相对于scrollview的上下左右边均应该添加限制（用来确定contentsize）。示例代码如下：

	- (void)viewDidLoad {
	    UIScrollView *scrollView;
	    UIImageView *imageView;
	    NSDictionary *viewsDictionary;
	    // Create the scroll view and the image view.
	    scrollView  = [[UIScrollView alloc] init];
	    imageView = [[UIImageView alloc] init];
	    // Add an image to the image view.
	    [imageView setImage:[UIImage imageNamed:"MyReallyBigImage"]];
	    // Add the scroll view to our view.
	    [self.view addSubview:scrollView];
	    // Add the image view to the scroll view.
	    [scrollView addSubview:imageView];
	    // Set the translatesAutoresizingMaskIntoConstraints to NO so that the views autoresizing mask is not translated into auto layout constraints.
	    scrollView.translatesAutoresizingMaskIntoConstraints  = NO;
	    imageView.translatesAutoresizingMaskIntoConstraints = NO;
	    // Set the constraints for the scroll view and the image view.
	    viewsDictionary = NSDictionaryOfVariableBindings(scrollView, imageView);
	    [self.view addConstraints:[NSLayoutConstraint constraintsWithVisualFormat:@"H:|[scrollView]|" options:0 metrics: 0 views:viewsDictionary]];
	    [self.view addConstraints:[NSLayoutConstraint constraintsWithVisualFormat:@"V:|[scrollView]|" options:0 metrics: 0 views:viewsDictionary]];
	    [scrollView addConstraints:[NSLayoutConstraint constraintsWithVisualFormat:@"H:|[imageView]|" options:0 metrics: 0 views:viewsDictionary]];
	    [scrollView addConstraints:[NSLayoutConstraint constraintsWithVisualFormat:@"V:|[imageView]|" options:0 metrics: 0 views:viewsDictionary]];
	    /* the rest of your code here... */
	}
	__weak typeof(self) weakself = self;
	[[NetworkSingleton sharedManager]loginWithParams:params successBlock:^(id successInfo) {
	NSString *resultCode = successInfo[@"ro"][@"code"];
	if ([resultCode isEqualToString:KREQSUCCEED]) {
	[weakself saveUserDataWithDic:successInfo[@"resp"]];
	} else {
		[GlobalData removeAllUserDatas];
	}
	} failureBlock:^(NSString *errorInfo) {
	}];

###例子
往scrollview上添加一个高度为720、宽度为屏幕宽的contentView,实现简单的上下滑动效果。xib如下图所示。

![img](http://7xozuy.com1.z0.glb.clouddn.com/autolayout&scrollviewsnapshot.png)

	//原始方式
	- (void)setupContent
	{
	    [_scrollView addSubview:_contentView];
	    [_contentView setTranslatesAutoresizingMaskIntoConstraints:NO];
	    NSLayoutConstraint *width = [NSLayoutConstraint constraintWithItem:_contentView attribute:NSLayoutAttributeWidth relatedBy:NSLayoutRelationGreaterThanOrEqual toItem:nil attribute:NSLayoutAttributeNotAnAttribute multiplier:1.0 constant:kScreen_Width];
	
	    NSLayoutConstraint *height = [NSLayoutConstraint constraintWithItem:_contentView attribute:NSLayoutAttributeHeight relatedBy:NSLayoutRelationGreaterThanOrEqual toItem:nil attribute:NSLayoutAttributeNotAnAttribute multiplier:1.0 constant:720];
	
	   /* NSLayoutConstraint *leading = [NSLayoutConstraint constraintWithItem:_contentView attribute:NSLayoutAttributeLeading relatedBy:NSLayoutRelationEqual toItem:_scrollView attribute:NSLayoutAttributeLeading multiplier:1.0 constant:0];
	    NSLayoutConstraint *trailing = [NSLayoutConstraint constraintWithItem:_contentView attribute:NSLayoutAttributeTrailing relatedBy:NSLayoutRelationEqual toItem:_scrollView attribute:NSLayoutAttributeTrailing multiplier:1.0 constant:0];*/
	        NSLayoutConstraint *left = [NSLayoutConstraint constraintWithItem:_contentView attribute:NSLayoutAttributeLeft relatedBy:NSLayoutRelationEqual toItem:_scrollView attribute:NSLayoutAttributeLeft multiplier:1.0 constant:0];
	        NSLayoutConstraint *right = [NSLayoutConstraint constraintWithItem:_contentView attribute:NSLayoutAttributeRight relatedBy:NSLayoutRelationEqual toItem:_scrollView attribute:NSLayoutAttributeRight multiplier:1.0 constant:0];
	    NSLayoutConstraint *top = [NSLayoutConstraint constraintWithItem:_contentView attribute:NSLayoutAttributeTop relatedBy:NSLayoutRelationEqual toItem:_scrollView attribute:NSLayoutAttributeTop multiplier:1.0 constant:0];
	    NSLayoutConstraint *bottom = [NSLayoutConstraint constraintWithItem:_contentView attribute:NSLayoutAttributeBottom relatedBy:NSLayoutRelationEqual toItem:_scrollView attribute:NSLayoutAttributeBottom multiplier:1.0 constant:-100];
	    [_scrollView addConstraints:@[width, height, left, right, top, bottom]];
	}
	//Masonry实现
	- (void)setupContent
	{
	    _isSelectPic = NO;
	    [_scrollView addSubview:_contentView];
	    UIEdgeInsets padding = UIEdgeInsetsMake(0, 0, 100, 0);
	    CGFloat width = kScreen_Width;
	    NSNumber *number = [NSNumber numberWithFloat:width];
	    [_contentView mas_makeConstraints:^(MASConstraintMaker *make) {
	        make.height.mas_equalTo(@720);
	        make.width.mas_equalTo(number);
	        make.edges.equalTo(_scrollView).insets(padding);
	    }];
	}
	
		  
参考：

[*http://natashatherobot.com/ios-autolayout-scrollview/*](http://natashatherobot.com/ios-autolayout-scrollview/)
[*https://developer.apple.com/library/ios/technotes/tn2154/_index.html*](https://developer.apple.com/library/ios/technotes/tn2154/_index.html)
[*http://stackoverflow.com/questions/21788514/uiscrollview-paging-autolayout-storyboard*](http://stackoverflow.com/questions/21788514/uiscrollview-paging-autolayout-storyboard)
[*http://www.cocoachina.com/ios/20141118/10242.html*](http://www.cocoachina.com/ios/20141118/10242.html)
[*http://www.jianshu.com/p/258f4efd7a1d/comments/421134*](http://www.jianshu.com/p/258f4efd7a1d/comments/421134)