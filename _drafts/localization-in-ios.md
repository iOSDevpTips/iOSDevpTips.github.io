---
layout: post
title: "iOS本地化"
category: Tip
---

Xcode中选中'PROJECT'，不是'TARGETS'，再选择'Info'，在'Localizations'项中添加想要的语言，默认xib或storyboard和InfoPlist.strings都已本地化了

- 本地化程序名称  

在InfoPlist.strings(English)文件中加入：
    CFBundleDisplayName = "ProjectName";

在InfoPlist.strings(Chinese)文件中加入：
    CFBundleDisplayName = "项目名称";

其他语言类似。

- 本地化字符串

新建一个名叫'Localizable'的strings文件，在'File Inspector'中找到'Localization'项，点击'Localize...'，完成后重新打开'Localizable.strings'，还是刚才那，勾选需要本地化的语言。
在Localizable.strings(English)文件中加入：
    "key" = "value";

在Localizable.strings(Chinese)文件中加入：
    "key" = "值";

其他语言类似。
在代码中用NSLocalizedString(@"key", nil)来使用'key'对应的本地化的值。
