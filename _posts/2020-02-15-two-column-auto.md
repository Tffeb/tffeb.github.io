---
layout: post
title: 两栏定宽中间自适应
categories: HTML5
description: 两栏定宽中间自适应
keywords: HTML5
---

**5种layout方案**

```html
    <!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style media="screen">
      .layout {
        margin-top: 20px;
        position: relative;
      }
      .layout article div {
        min-height: 100px;
      }
    </style>
  </head>
  <body>

      <!-- 使用浮动 -->
    <section class="layout float">
      <style media="screen">
        .layout.float .left {
          float: left;
          width: 300px;
          background: red;
        }
        .layout.float .right {
          float: right;
          width: 300px;
          background: blue;
        }
        .layout.float .center {
          background: yellow;
        }
      </style>
      <article class="left-right-center">
        <div class="left"></div>
        <div class="right"></div>
        <div class="center">
          <h1>浮动解决方案</h1>
        </div>
      </article>
    </section>


    <!-- 使用定位 -->
    <section class="layout absolute">
      <style media="screen">
        .layout.absolute .left-center-right > div {
          position: absolute;
        }
        .layout.absolute .left {
          left: 0;
          width: 300px;
          background: blue;
        }
        .layout.absolute .center {
          left: 300px;
          right: 300px;
          background: yellow;
        }
        .layout.absolute .right {
          right: 0;
          width: 300px;
          background: red;
        }
      </style>
      <article class="left-center-right">
        <div class="left"></div>
        <div class="center">
          <h1>定位解决方案</h1>
        </div>
        <div class="right"></div>
      </article>
    </section>


    <!-- 使用flexbox -->
    <section class="layout flexbox">
      <style media="screen">
        .layout.flexbox {
          margin-top: 140px;
        }
        .layout.flexbox .left-center-right {
          display: flex;
        }
        .layout.flexbox .left {
          width: 300px;
          background: blue;
        }
        .layout.flexbox .center {
          flex: 1;
          background: yellow;
        }
        .layout.flexbox .right {
          width: 300px;
          background: red;
        }
      </style>
      <article class="left-center-right">
        <div class="left"></div>
        <div class="center">
          <h1>flexbox解决方案</h1>
        </div>
        <div class="right"></div>
      </article>
    </section>


    <!-- 使用table -->
    <section class="layout table">
      <style media="screen">
        .layout.table .left-center-right {
          width: 100%;
          display: table;
          height: 100px;
        }
        .layout.table .left-center-right > div {
          display: table-cell;
        }
        .layout.table .left {
          width: 300px;
          background: blueviolet;
        }
        .layout.table .center {
          background: yellow;
        }
        .layout.table .right {
          width: 300px;
          background: red;
        }
      </style>
      <article class="left-center-right">
        <div class="left"></div>
        <div class="center">
          <h1>table解决方案</h1>
        </div>
        <div class="right"></div>
      </article>
    </section>

    <!-- 使用网格 -->
    <section class="layout grid">
      <style media="screen">
        .layout.grid .left-center-right {
          display: grid;
          width: 100%;
          grid-template-rows: 100px;
          grid-template-columns: 300px auto 300px;
        }
        .layout.grid .left {
          background: brown;
        }
        .layout.grid .center {
          background: yellow;
        }
        .layout.grid .right {
          background: yellowgreen;
        }
      </style>
      <article class="left-center-right">
        <div class="left"></div>
        <div class="center">
          <h1>grid解决方案</h1>
        </div>
        <div class="right"></div>
      </article>
    </section>
  </body>
</html>
```