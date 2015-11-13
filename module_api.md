[TOC]

#SDK API说明
SDK中将包括很多工具类，公共资源等。
先介绍部分使用说明，会逐步完善，稍后提供SDK和Demo。

## 接口使用说明
与服务器的接口交互，请使用工具类，将简化交互过程，不需要再实现网络请求，加密算法，异步线程等。
以一个接口为例，说明使用。
首选看接口文档，如下：
###app.customer.apply(预约报名）

应用级参数

|参数名|类型|必填|描述|
|--------|--------|--------|--------|
|mobile|string|是|预约手机号|
|user_name|string|是|姓名|

返回数据

``` JSON
{
    "result": 1,
    "data": "您已成功报名，设计师将尽快联系您！",
    "message": ""
}
```

如果要实现以上接口交互，代码如下：
Manager中：
```Java
public class CaseManager extends AbstractManager {

	public static void apply(String mobile, String name, final SimpleListener<String>listener){
    	//设置Request, 按照文档添加参数
		Protocol protocol = Protocol.create("app.customer.apply").add("mobile", mobile).add("user_name", name);
        //创建Task, 进行异步的网络请求
		TaskUtil.newTask(protocol, new TaskUtil.Listener() {

			@Override
			public void onFinish(JSONObject result) {
            	//当网络请求成功，直接返回JSON
				if (AllParser.success(result)) {//解析JSON
                	//如果成功，将结果回调给UI,或者其他处理。
					handleSuccess(listener, result.optString("data"));
				} else {
                	//如果失败，回调通知UI
					handleError(listener, result);
				}
			}

			@Override
			public void onNetworkError(boolean timeout) {
            	//当网络请求失败，timeout为true时，失败原因是超时，false时，为其他网络错误。
				handleError(listener, timeout);
			}
		});
	}
}

```

UI调用Manager：

```JAVA
CaseManager.apply("mobile", "name", new SimpleListener<String>(){
	@Override
	public void onFinish(String s) {
		//显示结果，已在UI线程
	}

	@Override
	public void onError(String message) {
		//显示错误信息，已在UI线程
	}
});
```

具体说明：
- `AbstractManager` 是SDK中提供的基础Manager, 提供`handleSuccess()`, `handleError()`等方法，用于回调给UI，并保证回调到UI线程。

- `SimpleListener<X>` 是回调用的Listener, X类型根据具体情况，可随意约束。

- `Protocol` 是拼请求用的，将自行加入 公共参数，计算加密等。参数使用`add("key",value)`的方式添加，对应接口文档中的`应用级参数`

- `TaskUtil.newTask()` 是用来生一个网络任务，放到线程池中，通过`TaskUtil.Listener`回调结果。结果分为`onFinish()`,`onNetworkError()`,分别表示网络通信成功/失败。通信成功后，返回JSON，再根据JSON判断请求是否成功。

- `AllParser.success()` 是SDK种提供的，公用的JSON解析工具。
