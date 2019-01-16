---
layout: post
title: TabbarControler
categories:
---

# TabbarViewControler

## using DefaultTabController

```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';
import 'package:github_app_flutter/pages/me/me.dart';
import 'package:github_app_flutter/pages/repo/repo.dart';
import 'package:github_app_flutter/pages/trending/trendingPage.dart';
import 'package:github_app_flutter/common/style/style.dart';

// import 'package:flutter_redux/flutter_redux.dart';
// import 'package:redux/redux.dart';

class HomePage extends StatefulWidget {
  static final String routeName = "home";

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage>
    with SingleTickerProviderStateMixin {
  TabController _tabController;

  @override
  void initState() {
    super.initState();
    _tabController = new TabController(vsync: this, length: tabs.length);
  }

  @override
  Widget build(BuildContext context) {
    return DefaultTabController(
      length: tabs.length,
      child: Scaffold(
        bottomNavigationBar: TabBar(
          isScrollable: true,
          labelColor: Colors.blue,
          unselectedLabelColor: Colors.black,
          indicatorSize: TabBarIndicatorSize.tab,
          indicatorPadding: EdgeInsets.all(5.0),
          indicatorColor: Colors.blue,
          tabs: tabs.map((Tabs tab) {
            return new SafeArea(
                child: new Tab(
              child: new Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: <Widget>[
                  new Icon(tab.icon, color: Colors.blue),
                  new Text(tab.title)
                ],
              ),
            ));
          }).toList(),
        ),
        body: TabBarView(
          children: tabs.map((Tabs tab) {
            return Container(
              child: tab.widget,
            );
          }).toList(),
        ),
      ),
    );
  }
}

class Tabs {
  const Tabs({this.icon, this.title, this.widget});

  final String title;
  final IconData icon;
  final Widget widget;
}

List<Tabs> tabs = <Tabs>[
  Tabs(icon: GHIcons.MAIN_DT, title: "Dynamic", widget: RepoPage()),
  Tabs(icon: GHIcons.MAIN_QS, title: "Trending", widget: TrendingPage()),
  Tabs(icon: GHIcons.MAIN_MY, title: "Me", widget: MePage()),
];
```

## using TabBar with controller

```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';
import 'package:github_app_flutter/pages/me/me.dart';
import 'package:github_app_flutter/pages/repo/repo.dart';
import 'package:github_app_flutter/pages/trending/trendingPage.dart';
import 'package:github_app_flutter/common/style/style.dart';

// import 'package:flutter_redux/flutter_redux.dart';
// import 'package:redux/redux.dart';

class HomePage extends StatefulWidget {
  static final String routeName = "home";

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage>
    with SingleTickerProviderStateMixin {
  TabController _tabController;

  @override
  void initState() {
    super.initState();
    _tabController = new TabController(vsync: this, length: tabs.length);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      bottomNavigationBar: TabBar(
        controller: _tabController,
        // isScrollable: true, // this makes each tab compressed
        labelColor: Colors.blue,
        unselectedLabelColor: Colors.black,
        // indicatorSize: TabBarIndicatorSize.tab,
        // indicatorPadding: EdgeInsets.all(5.0),
        // indicatorColor: Colors.blue,
        tabs: tabs.map((Tabs tab) {
          return new SafeArea(
              child: new Tab(
            child: new Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: <Widget>[
                new Icon(tab.icon, color: Colors.blue),
                new Padding(padding: new EdgeInsets.only(top: 5.0)),
                new Text(tab.title)
              ],
            ),
          ));
        }).toList(),
      ),
      body: new TabBarView(
        controller: _tabController,
        children: tabs.map((tab) => tab.widget).toList(),
      ),
    );
  }
}

class Tabs {
  const Tabs({this.icon, this.title, this.widget});

  final String title;
  final IconData icon;
  final Widget widget;
}

List<Tabs> tabs = <Tabs>[
  Tabs(icon: GHIcons.MAIN_DT, title: "Dynamic", widget: RepoPage()),
  Tabs(icon: GHIcons.MAIN_QS, title: "Trending", widget: TrendingPage()),
  Tabs(icon: GHIcons.MAIN_MY, title: "Me", widget: MePage()),
];
```