## 操作场景
通过视频处理 MPS，将 MPS 产生的回调任务通过 SCF 及时备份至 COS 是一个标准的方案，视频处理 MPS 在云函数 SCF 中配置了模板供用户使用。我们用到了视频处理（MPS）和云函数（SCF）。其中 MPS 主要用于视频处理任务，SCF 主要提供回调消息处理。

## 操作步骤
### 步骤1：创建云函数 SCF
1. 登录 [云函数控制台](https://console.cloud.tencent.com/scf/list)，选择左侧导航栏中的【[函数服务](https://console.cloud.tencent.com/scf/list)】。
![](https://main.qcloudimg.com/raw/d638a7441c3f636bdca73e10c6444b8c.png)
2. 在函数服务页面上方选择北京地域，并单击【新建】进入新建函数页面。
3. 设置以下参数信息，如下图所示：
	- **函数名称**：您可自定义名称，现以“MPSAnalysis”为例。
	- **运行环境**：目前仅支持 Python 2.7。
	- **创建方式**：选择模板函数。
	- **模糊搜索**：输入“CLS 消息转储至 COS”，并进行搜索。
	单击模板中的【查看详情】，即可在弹出的“模板详情”窗口中查看相关信息，支持下载操作。
![](https://main.qcloudimg.com/raw/c9accdd429f7f1616020caa38d4f6c5e.png)
3. 单击【下一步】，进入函数配置页。
![](https://main.qcloudimg.com/raw/1c8722be89889d7d6843ebbc7903181a.png)
4. 保持默认配置请单击【完成】，即完成函数的创建。

### 步骤2：配置 MPS 触发器
1. 在 [触发器](https://console.cloud.tencent.com/scf) 页面，选择【创建触发器】。
![](https://main.qcloudimg.com/raw/e30d23b09411b6aa7e99faa398807490.png)
2. 在弹出的创建触发器窗口中添加 MPS 触发器。如下图所示：
![](https://main.qcloudimg.com/raw/ddd374be2000e3e7f3ee9242b0d8ee8b.png)
主要参数信息如下，其余配置项请保持默认：
	- **事件类型**：MPS 触发器以账号维度的事件类型推送 Event 事件，目前支持工作流任务（WorkflowTask）和视频编辑任务（EditMediaTask）两种事件类型触发。
	- **事件处理**：MPS 触发器以服务维度产生的事件作为事件源，不区分地域、资源等属性。每个账号只允许两类事件分别绑定单个函数。如需多个函数并行处理任务，请参考 [函数间调用 SDK](https://cloud.tencent.com/document/product/583/37316)。


### 步骤3：测试函数功能
1. 在【[MPS 控制台](https://console.cloud.tencent.com/mps)】执行 MPS 的视频处理工作流。
2. 切换至【[云函数控制台](https://console.cloud.tencent.com/scf/list?rid=8&ns=default)】，查看执行结果。
在函数详情页面中选择【日志查询】页签，可以看到打印出的日志信息。如下图所示：
![](https://main.qcloudimg.com/raw/f5d10848b674f137826689ac1dc28c8a.png)
3. 切换至 [对象存储 COS 控制台](https://console.cloud.tencent.com/cos5)，查看数据转储及加工结果。

>?您可以根据自身的需求编写具体的数据加工处理方法。
