---
title: 兼容浏览器的事件处理程序 EventUtil
date: 2016-06-24 18:06:56
categories: [技术类-前端]
tags: [DOM, JavaScript]
---
```javascript
var EventUtil = {
    // 获取事件对象，兼容IE
    getEvent: function (event) {
        return event ? event : window.event;
    },
    // 获取事件目标，兼容IE
    getTarget: function (event) {
        return event.target || event.srcElement;
    },
    // 阻止默认事件，兼容IE
    preventDefault: function (event) {
        if (event.preventDefault) {
            event.preventDefault();
        }
        else {
            event.returnValue = false;
        }
    },
    // 阻止事件冒泡，兼容IE
    stopPropagation: function (event) {
        if (event.stopPropagation) {
            event.stopPropagation();
        }
        else {
            event.cancelBubble = true;
        }
    },
    // 注册事件
    addHandler: function (element, type, handler) {
        if (element.addEventListener) {
            // DOM2级的事件处理程序，最后一个参数为false，设置该事件处理程序在事件流的冒泡阶段触发
            element.addEventListener(type, handler, false);
        }
        else if (element.attachEvent){
            // IE的事件处理程序
            element.attachEvent('on' + type, handler);
        }
        else {
            // DOM0级的事件处理程序
            element['on' + type] = handler;
        }
    },
    // 删除事件
    removeHandler: function (element, type, handler) {
        if (element.removeEventListener) {
            element.removeEventListener(type, handler, false);
        }
        else if (element.detachEvent) {
            element.detachEvent('on' + type, handler);
        }
        else {
            element['on' + type] = null;
        }
    },
    // 获取事件(mouseover, mouseout)相关元素的信息
    getRelatedTarget: function (event) {
        if (event.relatedTarget) {
            return event.relatedTarget;
        }
        else if (event.toElement) {
            return event.toElement;
        }
        else if (event.fromElement) {
            return event.fromElement;
        }
        else {
            return null;
        }
    },
    // 获取鼠标点击事件的鼠标按钮
    getButton: function (event) {
        if (document.implementation.hasFeature('MouseEvents', '2.0')) {
            return event.button;
        }
        else {
            switch (event.button) {
                case 0:
                case 1:
                case 3:
                case 5:
                case 7:
                    // 返回0表示主鼠标按钮
                    return 0;
                case 2:
                case 6:
                    // 返回2表示次鼠标按钮
                    return 2;
                case 4:
                    // 返回1表示中间的鼠标按钮
                    return 1;
            };
        }
    },
    // 获取mousewheel事件的wheelDelta或者detail属性
    getWheelDelta: function (event) {
        if (event.wheelDelta) {
            return (client.engine.opera && client.engine.opera < 9.5
                ? -event.wheelDelta : event.wheelDelta);
        }
        else {
            return -event.detail * 40;
        }
    },
    // 获取键盘事件的字符编码
    getCharCode: function (event) {
        if (typeof event.charCode === 'number') {
            return event.charCode;
        }
        else {
            return event.keyCode;
        }
    }
};
```