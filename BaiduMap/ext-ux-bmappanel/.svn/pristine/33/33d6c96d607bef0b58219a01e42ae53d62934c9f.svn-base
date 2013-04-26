/*!
* Ext JS Library 3.4.0
* Copyright(c) 2006-2011 Sencha Inc.
* licensing@sencha.com
* http://www.sencha.com/license
*/
/**
* @class Ext.ux.BMapPanel
* @extends Ext.Panel
* @author xia bingyin
*/
Ext.ux.BMapPanel = Ext.extend(Ext.Panel, {
    initComponent: function () {
        //测试配置
        var defConfig = {
            width: 300,
            height: 400,
            plain: true,
            border: false,
            zoomLevel: 12,
            gmapType: 'map',
            ContextMenus: ['放大', '缩小', '放置到最大级', '查看全国', '在此添加标注', { setZoom: '缩小'}],
            markers: [{ lng: 28.867918, lat: 111.041754 }, { lng: 30.867918, lat: 111.041754 }, { lng: 31.59364, lat: 120.368824, listeners: { click: function () { alert(1) } }}],
            mapControls: ['NavigationControl', 'ScaleControl', 'OverviewMapControl', 'MapTypeControl'],
            x: 120.305456,
            y: 31.570037
        };
        Ext.applyIf(this, defConfig);
        Ext.ux.BMapPanel.superclass.initComponent.call(this);
    }
    , afterRender: function () {
        Ext.ux.BMapPanel.superclass.afterRender.call(this);
        //实例地图
        this.bmap = new BMap.Map(this.body.dom);
        //设置中心位置
        this.bmap.centerAndZoom(new BMap.Point(this.x, this.y), this.zoomLevel);
        //地图加载完成后加载控件和菜单
        this.bmap.addEventListener("tilesloaded", this.onMapReady());

    }
    , getMap: function () {
        return this.bmap;
    }
    , onMapReady: function () {
        //添加地图控件
        this.addMapControls();
        //添加标注
        this.addMarkers(this.markers);
        //添加右击菜单
        this.addContextMenus();
        this.getMap().enableScrollWheelZoom();

    }
    , addMapControls: function () {
        //根据配置设置地图默认控件
        if (this.gmapType === 'map') {
            if (Ext.isArray(this.mapControls)) {
                for (var i = 0; i < this.mapControls.length; i++) {
                    this.addMapControl(this.mapControls[i]);
                }
            } else if (typeof this.mapControls === 'string') {
                this.addMapControl(this.mapControls);
            } else if (typeof this.mapControls === 'object') {
                this.getMap().addControl(this.mapControls);
            }
        }
    }
    , addMapControl: function (mc) {
        //Check是否是百度地图的事件
        var mcf = BMap[mc];
        if (typeof mcf === 'function') {
            this.getMap().addControl(new mcf());
        }

    }
    , addContextMenus: function () {
        var contextMenu = new BMap.ContextMenu();
        //根据配置设置地图右击菜单
        if (Ext.isArray(this.ContextMenus)) {
            for (i = 0; i < this.ContextMenus.length; i++) {
                contextMenu.addItem(this.addContextMenu(this.ContextMenus[i]));
            }
        }
        this.bmap.addContextMenu(contextMenu);
    }
     , addContextMenu: function (Menu) {
         //var mcf = BMap[mc];
         if (typeof Menu === 'string') {
             switch (Menu) {
                 case '放大': return new BMap.MenuItem(Menu, function () { this._map.zoomIn() }, 100)
                 case '缩小': return new BMap.MenuItem(Menu, function () { this._map.zoomOut() }, 100)
                 case '放置到最大级': return new BMap.MenuItem(Menu, function () { this._map.setZoom(18) }, 100)
                 case '查看全国': return new BMap.MenuItem(Menu, function () { this._map.setZoom(4) }, 100)
                 case '在此添加标注':
                     return new BMap.MenuItem(Menu, function (p) {
                         var marker = new BMap.Marker(p), px = this._map.pointToPixel(p);
                         this._map.addOverlay(marker);
                         //启动标注可移动
                         marker.enableDragging(true);
                         //取得标记坐标
                         var nPosition = marker.getPosition();
                         //创建信息窗口）
                         var infoWindow = new BMap.InfoWindow(nPosition.lat + ',' + nPosition.lng);
                         //绑定click事件
                         marker.addEventListener("click", function () { this.openInfoWindow(infoWindow); });
                         //移动标注后取坐标
                         marker.addEventListener("dragend", function () {
                             nPosition = marker.getPosition();
                             infoWindow = new BMap.InfoWindow(nPosition.lat + ',' + nPosition.lng);
                         })
                     }, 100)
             }
         }
     }
    ,
    addMarkers: function (markers) {
        if (Ext.isArray(markers)) {
            for (var i = 0; i < markers.length; i++) {
                var mkr_point = new BMap.Point(markers[i].lat, markers[i].lng);
                var markerS = new BMap.Marker(mkr_point);
                this.addMarker(mkr_point, markerS, false, true, markers[i].listeners);
            }
        }

    },
    addMarker: function (point, markerS, clear, center, listeners) {
        //        Ext.applyIf(marker, G_DEFAULT_ICON);
        if (clear === true) {
            this.getMap().clearOverlays();
        }
        if (center === true) {
            this.getMap().setCenter(point, this.zoomLevel);
        }
        if (typeof listeners === 'object') {
            for (evt in listeners) {
                markerS.addEventListener(evt, listeners[evt]);
            }
        }
        this.getMap().addOverlay(markerS);
    }
});
//注册Ext.ux.BMapPanel的type为bmappanel
Ext.reg('bmappanel', Ext.ux.BMapPanel); 