# extended_nested_scroll_view

[![pub package](https://img.shields.io/pub/v/extended_nested_scroll_view.svg)](https://pub.dartlang.org/packages/extended_nested_scroll_view) [![GitHub stars](https://img.shields.io/github/stars/fluttercandies/extended_nested_scroll_view)](https://github.com/fluttercandies/extended_nested_scroll_view/stargazers) [![GitHub forks](https://img.shields.io/github/forks/fluttercandies/extended_nested_scroll_view)](https://github.com/fluttercandies/extended_nested_scroll_view/network)  [![GitHub license](https://img.shields.io/github/license/fluttercandies/extended_nested_scroll_view)](https://github.com/fluttercandies/extended_nested_scroll_view/blob/master/LICENSE)  [![GitHub issues](https://img.shields.io/github/issues/fluttercandies/extended_nested_scroll_view)](https://github.com/fluttercandies/extended_nested_scroll_view/issues) <a target="_blank" href="https://jq.qq.com/?_wv=1027&k=5bcc0gy"><img border="0" src="https://pub.idqqimg.com/wpa/images/group.png" alt="flutter-candies" title="flutter-candies"></a>

Language: [English](README.md) | [中文简体](README-ZH.md)

NestedScrollView: extended nested scroll view to fix following issues.

1.[pinned sliver header issue](https://github.com/flutter/flutter/issues/22393)

2.[inner scrollables in tabview sync issue](https://github.com/flutter/flutter/issues/21868)

3.pull to refresh is not work.

[Chinese blog](https://juejin.im/post/5bea43ade51d45544844010a)

- [extended_nested_scroll_view](#extendednestedscrollview)
- [Example for issue 1](#example-for-issue-1)
- [Example for issue 2](#example-for-issue-2)
  - [Step1](#step1)
  - [Step2](#step2)
- [Example for NestedScrollView pull to refresh](#example-for-nestedscrollview-pull-to-refresh)
- [Without ScrollController in NestedScrollView's body](#without-scrollcontroller-in-nestedscrollviews-body)
# Example for issue 1

give total height of pinned sliver headers in pinnedHeaderSliverHeightBuilder callback
``` dart
 var tabBarHeight = primaryTabBar.preferredSize.height;
      var pinnedHeaderHeight =
          //statusBar height
          statusBarHeight +
              //pinned SliverAppBar height in header
              kToolbarHeight;

 return NestedScrollView(
        pinnedHeaderSliverHeightBuilder: () {
          return pinnedHeaderHeight;
        },
       
```
# Example for issue 2

## Step1

Put your list which in tabview into NestedScrollViewInnerScrollPositionKeyWidget,and get unique a key
``` dart
 return extended.NestedScrollViewInnerScrollPositionKeyWidget(
        widget.tabKey,
        ListView.builder(
            itemBuilder: (c, i) {
              return Container(
                alignment: Alignment.center,
                height: 60.0,
                child: Text(widget.tabKey.toString() + ": List$i"),
              );
            },
            itemCount: 100)
        );
```
## Step2

get current tab key in innerScrollPositionKeyBuilder callback. this key should as same as in step 1 given.
``` dart
 extended.NestedScrollView(
        innerScrollPositionKeyBuilder: () {
          var index = "Tab";
          if (primaryTC.index == 0) {
            index +=
                (primaryTC.index.toString() + secondaryTC.index.toString());
          } else {
            index += primaryTC.index.toString();
          }
          return Key(index);
        },
```
# Example for NestedScrollView pull to refresh

NestedScrollViewRefreshIndicator is as the same as Flutter RefreshIndicator.
``` dart
 NestedScrollViewRefreshIndicator(
       onRefresh: onRefresh,
       child: extended.NestedScrollView(
         headerSliverBuilder: (c, f) {
           return _buildSliverHeader(primaryTabBar);
         },
```

[Better one to pull to refresh](https://github.com/fluttercandies/loading_more_list/blob/master/example/lib/demo/nested_scroll_view_demo.dart)

Please see the example app of this for a full example.

# Without ScrollController in NestedScrollView's body

* due to we can't set ScrollController for list in NestedScrollView's body(it will breaking behaviours of InnerScrollController in NestedScrollView),provide Demos
  
* [pull to refresh](https://github.com/fluttercandies/extended_nested_scroll_view/tree/master/example/lib/pages/pull_to_refresh.dart),
  
* [load more](https://github.com/fluttercandies/extended_nested_scroll_view/tree/master/example/lib/pages/load_more.dart) 
  
* [scroll to top](https://github.com/fluttercandies/extended_nested_scroll_view/tree/master/example/lib/pages/scroll_to_top.dart) 
  
  to show how to do it without ScrollController