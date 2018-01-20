---
layout: post
#标题
title:  UISearchBar创建
#时间配置
date:   2015-06-13 21:38:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

![141425162079416.png]({{ site.img_url }}1696310973181CB09B7FC584728A3240.png)

![141425379735699.png]({{ site.img_url }}10BAD53725E6D234157D089FA0DD1F38.png)


```objc
#pragma mark 创建UISearchBar
    UISearchBar *search = [[UISearchBar alloc] initWithFrame:CGRectMake(30, 480 - 260, 320 - 60, 40)];
//    可以在搜索栏上方添加一行字
    search.prompt = @" "; //不写scopeBar不显示
    search.frame = CGRectMake(30, 480 - 260, 320 - 60, 100);
    self.search = search;
    search.placeholder = @"请输入搜索内容";
//    设置searchBar外边框的颜色
    search.barTintColor = [UIColor whiteColor];
//    设置其中闪的线的颜色
    search.tintColor = [UIColor redColor];
//    设置searchBar能否透明
    search.translucent = YES;
    search.backgroundColor = [UIColor whiteColor];
    search.alpha = 1;
//    设置searchBar的样式，有两种
//    search.searchBarStyle = UISearchBarStyleProminent;
    search.searchBarStyle = UISearchBarStyleDefault;
//    search.searchBarStyle = UISearchBarStyleMinimal;
//    search.barStyle = UIBarStyleBlackOpaque;
//    search.barStyle = UIBarStyleBlackTranslucent;
//    search.barStyle = UIBarStyleBlack;
    
//    设置大小写，没有限制
    search.autocapitalizationType = UITextAutocapitalizationTypeNone;
//    句子的第一个字母大写
//    search.autocapitalizationType = UITextAutocapitalizationTypeSentences;
//    每一个单词都自动设置大写
//    search.autocapitalizationType = UITextAutocapitalizationTypeAllCharacters;
//    设置每个单词的第一个字母大写
//    search.autocapitalizationType = UITextAutocapitalizationTypeWords;
    
//    设置自动修正方式
//    search.autocorrectionType = UITextAutocorrectionTypeDefault;
//    search.autocorrectionType = UITextAutocorrectionTypeNo;
    search.autocorrectionType = UITextAutocorrectionTypeYes;
    
//    设置键盘样式
    search.keyboardType = UIKeyboardTypeNumberPad;
    search.keyboardType = UIKeyboardTypeDefault;
    
//    设置拼写检查类型
//    search.spellCheckingType = UITextSpellCheckingTypeDefault;
//    search.spellCheckingType = UITextSpellCheckingTypeNo;
    search.spellCheckingType = UITextSpellCheckingTypeYes;
    
//    设置是否显示BookmarkButton
//    search.showsBookmarkButton = NO;
//    search.showsBookmarkButton = YES;
    
//    search.showsCancelButton = NO;
//    search.showsCancelButton = YES;
//    [search setShowsCancelButton:YES animated:YES];
    
//    search.showsScopeBar = NO;
//    search.showsScopeBar = YES;
    
    search.showsSearchResultsButton = NO;
    search.showsSearchResultsButton = YES;
    
    search.searchResultsButtonSelected = YES;
    search.showsScopeBar = YES;
    search.scopeButtonTitles = [NSArray arrayWithObjects:@"张三",@"李四", nil];
    
    [self.view addSubview:search];
    [search release];
```