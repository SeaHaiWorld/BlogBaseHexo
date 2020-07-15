---
title: Jest - 单元测试及可视化报告
date: 2019-07-12 20:10:33
tags:
- Jest
- nodejs
categories:
- 自动化测试
---
### 准备工作
---
#### 安装 jest
> cnpm install --save-dev jest

#### 查看版本
> jest -v

确保你的项目已经安装了**版本较新**的 jest 测试框架(随版本更迭有可能产生无法预知的BUG，如：配置属性名变更)

已完成对 jest 配置文件 **jest.config.js** 的基本配置
### 函数测试
---
首先要做的事情：What needs to be tested?？

如果你正在编写 Web 应用，那么一个好的起点就是测试应用的每个页面和每个用户交互。但 Web 应用由单元代码组成，如 **UI 、函数和组件**，分别需要单独进行测试。
<!--more-->
#### 函数单元测试
两种情况：

*   你正接手一些函数功能**未知**的代码
*   你要实现之前没有的**新功能**,但不知是否实现

对于这两种情况，你可以通过将**测试**看作检查给定函数是否产生**预期结果**来帮助自己。 以下是典型测试流程的样子：

*   导入要测试的函数
*   给函数**输入**
*   定义**期望**
*   检查是否按**预期**输出

只需这样执行：输入 → 预期输出 → 断言结果


#### 书写测试用例
以下是一个完整的测试用例

创建 filename.js 文件，描述测试函数
```
   // utils.js
   export function fixedZero(val) {
     return val * 1 < 10 ? `0${val}` : val;
   }
```
创建 filename.test.js 文件,描述断言
```
    // utils.test.js
    import { fixedZero } from './utils';
    ...
    // describe('函数分组测试描述',() => {
    //   test('单元测试描述', () => {
    //       expect("函数结果").toEqual("期望结果");
    //   });    
    // })

    describe('fixedZero tests', () => {
        it('should not pad large numbers', () => {
            expect(fixedZero(10)).toEqual(10);
            expect(fixedZero(11)).toEqual(11);
            expect(fixedZero(15)).toEqual(15);
            expect(fixedZero(20)).toEqual(20);
            expect(fixedZero(100)).toEqual(100);
            expect(fixedZero(1000)).toEqual(1000);
            expect(fixedZero(1000)).toEqual(1000);
    });

        it('should pad single digit numbers and return them as string', () => {
            expect(fixedZero(0)).toEqual('00');
            expect(fixedZero(1)).toEqual('01');
            expect(fixedZero(2)).toEqual('02');
            expect(fixedZero(3)).toEqual('03');
            expect(fixedZero(4)).toEqual('04');
            expect(fixedZero(5)).toEqual('05');
            expect(fixedZero(6)).toEqual('06');
            expect(fixedZero(7)).toEqual('07');
            expect(fixedZero(8)).toEqual('08');
            expect(fixedZero(9)).toEqual('09');
        });
    });
```

在命令行输入npm test utils.test.js，我们可以看到命令台的返回
![ceshi.png](https://blogimage-1259219507.cos.ap-chengdu.myqcloud.com/fenzuceshi.png)

### 实现在浏览器上实现测试结果的可视化显示
---
当我们对一些比较庞大的项目，要进行成百上千个函数进行单元测试时，测试结果仅仅显示在控制台明显不够看。如何优雅的让测试结果展示出来，并可以详细的观察测试结果的具体问题呢？

我们会想到将 jest 的测试结果 --> 存储 --> 发请求(简单服务器) --> 发送到浏览器 --> 展示
#### 两种方法
* 通过 nodejs 实现但是需要配置一个简单的 node 服务器，来实现在浏览器显示。但是方法过于繁琐，不在赘述。
* 借助于报告工具 jest-html-report (本质与第一个方法没有区别，只是这个工具是打包好的 node 库，可以直接投入使用)

#### 参数配置
根据 jest 官方文档在 jest.config.js 中有 testResultsProcessor 属性：

| Property |  Description  | Type | Default|
|-------|:---:|:---:|------|
| testResultsProcessor | This option allows the use of a custom results processor. This processor must be a node module that  exports a function expecting an object with the following structure as the first argument and return it: | string | undefined |

这个属性可以允许结果处理程序使用。这个处理器必须是一个输出函数的 node 模块，这个函数的第一个参数会接收测试结果，且必须在最终返回测试结果。可以用与于接收测试结果，且在最终返回测试结果

首先我们安装它：cnpm install jest-html-report  --save-dev 

在 jest.config.js 中，具体配置 jest-html-reporter 的属性

用到的属性：

| Property |  Description  | Type | Default|
| :-----| :----: | :----: | :----: |
| pageTitle | The title of the document | string | "Test Suite"|
| outputPath | The path to where the plugin will output the HTML report | string |"./test-report.html" |
| includeFailureMsg | If this setting is set to true, this will output the detailed failure message for each failed test. | boolean | false |

其他属性参考**官方文档**：https://github.com/Hargne/jest-html-reporter/wiki/configuration

完成 jest.config.js 中 jest-html-reporter 的配置：
```
       //jest.config.js
       module.exports={
           ...
           testResultsProcessor:'./testReport',
           reporters: [
           'default',
           [
             './node_modules/jest-html-reporter',
             {
               //输出页面标题
               pageTitle: 'Test Report',
               //插件将会输出的HTML报告的路径。
               outputPath:'testReport/JesttestReport.html',
               //为每个失败的测试输出详细的失败消息。
               includeFailureMsg: true,
             },
           ],
         ],
       }
```
再次在命令行输入 npm test utils.test.js，我们可以看到测试结果被成功返回在 testReport/JesttestReport.html 中

我们打开生成的 html 文件，测试结果的可视化就完成啦。
![testreport.png](https://blogimage-1259219507.cos.ap-chengdu.myqcloud.com/Jesttestreport.png)


### 总结
---
+ 掌握单元测试
+ 实现自动生成测试报告

### 参考阅读
---
+ 如何将jest的测试结果转移到页面上：[https://juejin.im/post/5c2c57ba5188257ed57eb471](https://juejin.im/post/5c2c57ba5188257ed57eb471)
+ 关于 jest 测试结果如何在浏览器上显示的问题：[https://blog.csdn.net/weixin_34314962/article/details/88811759](https://blog.csdn.net/weixin_34314962/article/details/88811759)
+ Jest官方文档：[https://jestjs.io/docs/en/getting-started.html](https://jestjs.io/docs/en/getting-started.html)
+ jest-html-reporter官方文档：[https://github.com/Hargne/jest-html-reporter/wiki/configuration](https://github.com/Hargne/jest-html-reporter/wiki/configuration)
