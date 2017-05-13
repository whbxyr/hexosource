---
title: DOM详解
date: 2016-04-20 20:32:42
categories: [技术类-前端]
tags: [DOM, html, JavaScript]
---
&emsp;&emsp;**文档节点**是每个文档的**根**节点，文档节点只有一个子节点，即`<html>`元素，我们称之为**文档元素**。文档元素是文档的最外层元素，文档中的其他所有元素都包含在文档元素中。每个文档只能有一个文档元素。在HTML页面中，文档元素始终都是`<html>`元素。在XML中，没有预定义的元素，因此任何元素都可能成为文档元素。

&emsp;&emsp;接下来讲讲**Node**类型。DOM1级定义了一个Node接口，该接口将由DOM中的所有节点类型实现。这个Node接口在JavaScript中是作为Node类型实现的；除IE之外，在其他所有浏览器中都可以访问到这个类型。JavaScript中的所有节点类型都继承自Node类型，因此所有节点类型都共享着相同的基本属性和方法。每个节点都有一个nodeType属性，共有12种节点类型，每种类型都由Node类型中定义的一个数值常量来表示，如下：
```
Node.ELEMENT_NODE(1)
Node.ATTRIBUTE_NODE(2)
Node.TEXT_NODE(3)
Node.CDATA_SECTION_NODE(4)
Node.ENTITY_REFERENCE_NODE(5)
Node.ENTITY_NODE(6)
Node.PROCESSING_INSTRUCTION_NODE(7)
Node.COMMENT_NODE(8)
Node.DOCUMENT_NODE(9)
Node.DOCUMENT_TYPE_NODE(10)
Node.DOCUMENT_FRAGMENT_NODE(11)
Node.NOTATION_NODE(12)
```