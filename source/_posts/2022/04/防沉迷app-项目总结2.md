---
title: 防沉迷app-项目总结2
date: 2022-04-26 10:50:08
permalink:
tags:
   - 技术
   - 项目总结
   - 算法
categories:
- 技术生活
  - 编程技术
    - 项目总结
---

# 防沉迷项目总结（2）

## 软件开发的需求变动问题
作为开发者，最不喜欢的场景是需求朝令夕改。频繁的需求改动对开发者的耐性和信心是极大的打击。
1. 心情：这个就不用多说了，开发是很枯燥的。让程序员重复做一个事情，不停的重复更枯燥。
2. 设计问题：最初a需求的技术方案，开发者设计好了，需求变了之后，技术方案都有可能会受到影响。

对于这个问题，目前是无解。市场是对的，老板也是对的，需求没错。所以就需要开发者提高自身素质，对潜在的需求变动要有前瞻性。在技术设计方面
对于可能的变动预留足够的弹性空间。


### 一个被需求折磨的例子
需求a：桌面app只需要显示和隐藏
a基础上改动后的需求b：要初始化显示系统app，同时行列也要变化。

这块改动比较大，改动之前的就不贴代码了

```
 package com.benny.openlauncher.util;

import android.util.Log;

import com.benny.openlauncher.AppObject;
import com.benny.openlauncher.activity.HomeActivity;
import com.benny.openlauncher.model.App;
import com.benny.openlauncher.model.Item;
import com.benny.openlauncher.widget.CellContainer;
import com.benny.openlauncher.widget.Desktop;

import java.util.ArrayList;
import java.util.List;

// 桌面app排列有关工具类
public class DeskTopUtils {

    // 第一次初始化
    public static void onCreate(List<App> apps) {
       // TODO 
    }
    // 添加到hide，并从数据库移除
    public static void removeFromDesk(String pkg) {
        Log.d("DeskTopUtils","remove local app from desk pkg:"+ pkg);
        // hide 更新
        ArrayList<String> hiddenList = AppSettings.get().getHiddenAppsList();
        if(HomeActivity._db == null )return;

        // 根据pkg找到app
        List<App>  apps =  AppManager.getInstance(AppObject.get()).getNonFilteredApps();
        App findApp = null;
        for (App app : apps) {
            if (app.getComponentName().contains(pkg)) {
                findApp = app;
                // 构造Intent string
                HomeActivity._db.deleteItemByIntent(Tool.getIntentAsString(Tool.getIntentFromApp(findApp)));
                Log.d("DeskTopUtils","remove find match local app "+ findApp.getComponentName());
            }
        }

        if( findApp == null ) {
            Log.d("DeskTopUtils","removeFromDesk can't find match local app "+ pkg);
            return;
        } else {
            Log.d("DeskTopUtils","removeFromDesk find match local app "+ findApp.getComponentName());
        }


    }

    // 生成坐标
    private static String genCoordinate(List<List<Item>> itemLists ){
        String coordinateStr =  "";
        for (int pageCount = 0; pageCount < itemLists.size(); pageCount++) {
            List<Item> page = itemLists.get(pageCount);
            for (int j = 0; j < page.size(); j++) {
                coordinateStr += "("+pageCount +"," + page.get(j)._x+","+page.get(j)._y+");";
            }
        }
        return coordinateStr;
    }

    // 从hide移除，并从desktop中选择合适位置，添加进去
    public static void addToDesk(String pkg) {
        // hide 更新
        ArrayList<String> hiddenList = AppSettings.get().getHiddenAppsList();
        if(HomeActivity._db == null )return;

        // 根据pkg找到app
        List<App>  apps =  AppManager.getInstance(AppObject.get()).getNonFilteredApps();
        App findApp = null;
        for (App app : apps) {
            if (app.getComponentName().contains(pkg)) {
                findApp = app;
            }
        }

        if( findApp == null ) {
            Log.d("DeskTopUtils","addToDesk can't find match local app "+ pkg);
            return;
        } else {
            Log.d("DeskTopUtils","addToDesk find match local app "+ findApp.getComponentName());
        }

        // 数据库更新
        List<CellContainer> _pages = new ArrayList<>();
        List<List<Item>> desktopItems = HomeActivity._db.getDesktop();

        String deskGrid = genCoordinate(desktopItems);
        int row = AppSettings.get().getDesktopRowCount();
        int col = AppSettings.get().getDesktopColumnCount();

        // 查找每一个page中的合适位置
        String t_point = "";
        int find_x = 0;
        int find_y = 0;
        boolean find = false;

        // 倒序，刚容易找到位置，先这样
        for (int pageCount = 0; pageCount < desktopItems.size(); pageCount++) {
            List<Item> page = desktopItems.get(pageCount);
            // 如果倒序，刚容易找到位置，先这样;取舍：这样可以在添加的时候，优先使用最前面的空缺位置
            for( int j=0;j<page.size();j++) {
                //尝试在每一个元素周边寻找区域
                Item t_item = page.get(j);
                int t_x = t_item._x;
                int t_y = t_item._y;
                // 右边
                int tx_top = t_x-1;
                if( tx_top >=0 ) {
                    t_point = "(" +pageCount+","+tx_top+","+t_y+")";
                    if( !deskGrid.contains( t_point ) ) {
                        find_x = tx_top;
                        find_y = t_y;
                        find = true;
                        break;
                    }
                }

                // 左边
                tx_top = t_x +1;
                if( tx_top >=0 && tx_top < col ){
                    t_point = "("+pageCount+","+tx_top+","+t_y+")";
                    if( !deskGrid.contains( t_point ) ) {
                        find_x = tx_top;
                        find_y = t_y;
                        find = true;
                        break;
                    }
                }

                // 上边
                int ty_top = t_y -1;
                if( ty_top >=0 ){
                    t_point = "("+pageCount+","+t_x+","+ty_top+")";
                    if( !deskGrid.contains( t_point ) ) {
                        find_x  = t_x;
                        find_y = ty_top;
                        find = true;
                        break;
                    }
                }

                // 下边
                ty_top = t_y + 1;
                if( ty_top >=0 && ty_top < row){
                    t_point = "("+pageCount+","+t_x+","+ty_top+")";
                    if( !deskGrid.contains( t_point ) ) {
                        find_x  = t_x;
                        find_y = ty_top;
                        find = true;
                        break;
                    }
                }
            }

            Item newItem = Item.newAppItem(findApp);
            newItem._spanY = 1;
            newItem._spanX = 1;
            if( find ) {
                newItem._x = find_x;
                newItem._y = find_y;
                HomeActivity._db.saveItem(newItem, pageCount, Definitions.ItemPosition.Desktop);
                Log.d("DeskTopUtils","addToDesk find location to locate app  page,x,y="+pageCount +"," +find_x+","+ find_y +"，package="+Tool.getIntentAsString(newItem._intent));
                break;
            } else {
                // 新建一个页面，放置app
                if ( pageCount == desktopItems.size()-1 ) {
                    int newPage = pageCount + 1;
                    newItem._x = 0;
                    newItem._y = 0;
                    HomeActivity._db.saveItem(newItem, newPage, Definitions.ItemPosition.Desktop);
                    Log.d("DeskTopUtils", "addToDesk no location , new page ,  page,x,y=" + newPage + "," + find_x + "," + find_y + "，package=" + Tool.getIntentAsString(newItem._intent));
                    break;
                } else {
                    Log.d("DeskTopUtils", "addToDesk no location , query new page try");
                }
            }
        }
    }

}


```

