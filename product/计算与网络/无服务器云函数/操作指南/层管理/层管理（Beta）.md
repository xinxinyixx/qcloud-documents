## 操作场景
如果您的云函数（SCF）拥有较多的依赖库或公共代码文件，您可以使用 SCF 中的层进行管理。使用层管理，您可以将依赖放在层中而不是部署包中，可确保部署包保持较小的体积。对于 Node.js、Python 和 PHP 函数，只要将部署程序包保持在10MB以下，就可以在 SCF 控制台中在线编辑函数代码。
>?SCF 的层管理目前为测试发布功能，随时可能更新。如需使用此功能，请前往 [申请](https://cloud.tencent.com/apply/p/l2yl67xa0tc) 页面填写信息并提交申请。



## 说明事项
- 层中的文件将会添加到 `/opt` 目录中，此目录在函数执行期间可访问。
- 如果您的函数已绑定了多个层，这些层将按顺序合并到 `/opt` 目录中。如果同一个文件出现在多个层中，SCF 平台将会保留最大序号层里的文件。
- 如果您正在使用层版本被删除，与该层版本绑定的函数将会继续运行。


## 操作步骤

### 创建层<span id="create"></span>
1. 登录 SCF 控制台，选择左侧导航栏中的【[层](https://console.cloud.tencent.com/scf/layer)】，进入“层”列表页面。
2. 在页面上方选择需使用层的地域，并单击【新建】。
3. 在“新建层版本”页面，根据实际需求设置层信息。如下图所示：
![](https://main.qcloudimg.com/raw/1e77f5a6c8911f159ba1220c2b2bf501.png)
 - **层名称**：输入自定义层名称。
 - **描述**：层的描述信息，根据实际情况填写。
 - **提交方法**：支持**本地上传zip包**及**本地上传文件夹**，结合实际情况选择依赖包提交方式。
    确定提交方法后单击【上传】，在弹出的依赖包选择界面，选择需上传的依赖包并单击【确定】。
 - **兼容运行环境**：该层的兼容运行环境，最多可设置5个。
4. 单击【确定】即可成功创建。

### 云函数绑定层<span id="bind"></span>
1. 登录 SCF 控制台，选择左侧导航栏中的【[函数服务](https://console.cloud.tencent.com/scf/list)】，进入“函数服务”列表页面。
2. 选择需进行层管理的函数 ID，进入函数配置页面。
3. 选择【层管理】页签，并单击【绑定层】。如下图所示：
![](https://main.qcloudimg.com/raw/485d1172edd541f602758ed94f76e5eb.png)
4. 在弹出的“绑定层”窗口中，选择对应【层名称】及【层版本】。如下图所示：
![](https://main.qcloudimg.com/raw/b0bad15c29efd9ecd8441a0a44c9377f.png)
5. 单击【提交】即可完成绑定。



### 使用层
本步骤以 Node.js 为例，创建层并绑定本地上传的函数后使用层。

1. 参考 [创建层](#create) 步骤将 `node_modules` 上传生成层。本地函数目录结构如下图所示：
![](https://main.qcloudimg.com/raw/88a8477d8668610dd150887b326628a4.png)
2. 参考 [部署函数](https://cloud.tencent.com/document/product/583/9702) 将本地函数代码打包上传，打包时执行以下命令排除 `node_modules` 文件夹。
```
zip -r 包名.zip . -x "node_modules/*"
```
如下图所示：
![](https://main.qcloudimg.com/raw/31c531fbc98d0a5cc5c542b7e3721c9d.png)
3. 参考 [绑定云函数](#bind) 步骤，将已创建的层绑定至部署好的函数。 
4. 完成上述步骤后，即可开始在函数中使用层。
由于 NODE_PATH 环境变量包含 `/opt/node_modules` 路径，函数运行时可以查找到层中的依赖，您使用依赖的方式和原来一样，无需修改代码，本文以使用 `cos-nodejs-sdk-v5` 依赖为例。如下图所示：
![](https://main.qcloudimg.com/raw/6167eb686aeeadacd646beb998e19136.png)
Python、Java、Node.js 环境变量见下表：
<table>
	<tr>
	<th>相关环境变量</th>
	<th>路径</th>
	</tr>
	<tr>
	<td>PYTHONPATH</td>
	<td><code>/var/user:/opt </code></td>
	</tr>
	<tr>
	<td>CLASSPATH</td>
	<td><code> /var/runtime/java8:/var/runtime/java8/lib/*:/opt   </code></td>
	</tr>
	<tr>
	<td>NODE_PATH</td>
	<td><code>/var/user:/var/user/node_modules:/var/lang/node6/lib/node_modules:/opt:/opt/node_modules</code></td>
	</tr>
</table>


