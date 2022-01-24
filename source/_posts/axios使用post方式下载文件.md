---
title: axios使用post方式下载文件
date: 2022-01-24 16:41:52
tags: ajax
---
首先我们在请求的方法中添加参数"responseType: 'arraybuffer'或'blob'"：(或'blob'，与后端商议)

```javascript
// api.js

import axios from 'axios';

export function getFile (data) {
  return axios({
    url: '/XXX/getFile',
    method: 'post',
    data,
    responseType: 'arraybuffer' // 或'blob'，与后端商议
  });
}
```

组件中的方法:：
```javascript
// index.js

import { getFile } from '@/api';
import { downloadFile } from '@/tool';

async exportData () {
    try {
        const file = await getFile(params);
        downloadFile(file, '代理人列表');
    } catch (err) {
        // catch
    }
}
```
公用的downloadFile方法：
```javascript
// tool.js

export function downloadFile (file, filename = '') {
    let blob = new Blob(
        [file], 
        {
            type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet;charset=utf-8'
        });

    let downloadElement = document.createElement('a');
    let href = window.URL.createObjectURL(blob); //创建下载的链接
    downloadElement.href = href;
    downloadElement.download = filename; //下载后文件名
    document.body.appendChild(downloadElement);
    downloadElement.click(); //点击下载
    document.body.removeChild(downloadElement); //下载完成移除元素
    window.URL.revokeObjectURL(href); //释放掉blob对象
}
```