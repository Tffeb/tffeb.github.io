---
layout: post
title: 简单记录下react中使用富文本
categories: React
description: 简单记录下react中使用富文本
keywords: API, React
---

##### 在react项目中使用富文本编辑器

首先，先安装 draftjs-to-html、react-draft-wysiwyg

```
 yarn add draftjs-to-html react-draft-wysiwyg
```

话不多说，然后直接上代码：(这里使用的是antd组件进行组合使用)

```js
import React from 'react'
import { Button, Card, Modal } from 'antd'
import { Editor } from 'react-draft-wysiwyg'
import 'react-draft-wysiwyg/dist/react-draft-wysiwyg.css'
import draftjs from 'draftjs-to-html'
export default class RichText extends React.Component {

    state = {
        showRichText: false,
        editorContent: '',  
        editorState: '',
    };
    // 清空富文本内容
    handleClearContent = () => {
        this.setState({
            editorState: ''
        })
    }
    // 展示富文本内容
    handleGetText = () => {
        this.setState({
            showRichText: true
        })
    }
    // 监听展示内容变化
    onEditorChange = (editorContent) => {
        this.setState({
            editorContent,
        });
    };
    // 监听富文本编辑器内容的变化
    onEditorStateChange = (editorState) => {
        this.setState({
            editorState
        });
    };

    render() {
        const { editorContent, editorState } = this.state;
        return (
            <div>
                <Card style={{ marginTop: 10 }}>
                    <Button type="primary" onClick={this.handleClearContent}>清空内容</Button>
                    <Button type="primary" onClick={this.handleGetText}>获取HTML文本</Button>
                </Card>
                <Card title="富文本编辑器">
                    <Editor
                        editorState={editorState}
                        onContentStateChange={this.onEditorChange}
                        onEditorStateChange={this.onEditorStateChange}
                    />
                </Card>
                <Modal
                    title="富文本"
                    visible={this.state.showRichText}
                    onCancel={() => {
                        this.setState({
                            showRichText: false
                        })
                    }}
                    footer={null}
                >
                    {draftjs(this.state.editorContent)}
                </Modal>
            </div>
        );
    }
}
```

<p>该api有趣，后续继续更新。。。</p>