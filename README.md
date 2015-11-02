#项目文档

##基础
###环境
Android Studio, Android SDK6.0, Mac OS.

###注意事项

- Target SDK Version:22, 不能大于22，因为新的Android6.0 权限机制改变，会导致无法读取存储卡等问题。


###项目结构

- daivp.zergling 下为工具类，包括网络，图片缓存等。
- onenurse 下标准MVC分包
- onenurse.manager Controllers
- onenurse.model Models
- onenurse.view Views

###Class 说明

- ChangePasswordActivity	修改密码
- EditAreaActivity	修改用户信息-修改地区
- EditExpActivity	修改用户信息-修改经验
- FindPasswordActivity	找回密码
- HomeActivity	首页（包括了订单列表，用户信息）
- MessageActivity 	信息详情
- MessagesActivity 	信息列表
- MoreInfoActivity 	更多页面
- MyCalendarActivity 	排班表页面
- MyDetailActivity 		我的详情
- OrderDetailActivity 	形单详情
- RateActivity 	订单评价
- ResultActivity 	结果页面（订单评价，接单，建议）
- SuggestActivity 	对App建议
- WebActivity 	WebView页面（用户协议，用户指南，关于我们）
- LoginActivity 	登录
- AllParser 用于解析JSON
- OneManager 核心Controller, 大部分网络请求都是异步，回调通过BaseListener<X>
例如：

```
OneManager.login(name, password, new BaseListener <Object> () {

	public void onFinish(Object data) {
		//TODO 请求成功
	}

	public void onError(String message) {
		//TODO 请求失败
	}

	public void onFail() {
		//TODO 网络失败
	}
});
```




##API相关

API文档：https://docs.google.com/document/d/17nETEK5Vdx7FwijiNE29xM35N0cMpXDClXE7Zxh5siI/edit#

###HTTP Status Code
| Code | 描述 |
|--------|--------|
|200|成功|
|204|资源为找到|
|401|Token 过期|
|403|密码错误|

###Models

| Model | ... |
|--------|--------|
|OrderInfo|订单|
|BaseCheck|表示医院/地区（因为选择时候，只考虑ID,Name,Checked，所以合并为一个Model）|
|Message|消息用，API未出，所以Model细节待定|
|Specialty|护理特长|

####Caregiver Model

|参数名|类型|必填|描述|
|--------|--------|--------|--------|
|user_id|Integer|Yes|id of user|
|name|String|Yes|姓名|
|grade|String|Yes|等级 `primary_nurse`=高级护理师,`junior_supervisor_nurse`=高级护理师,`deputy_chief_nurse`=高级护理师|
|phone|String|Yes|电话|
|native_province|String|Yes|籍贯|
|gender|String|Yes|性别 `male`=男,`female`=女|
|nation|String|Yes|民族|
|age|Integer|Yes|年龄|
|educational_background|String|Yes|学历 `bachelor`=本科，`junior_college`=专科，`high_school`=高中，`junior_school`=初中，`primary_school`=小学|
|nursing_experience_age|Integer|Yes|护龄|
|id_number|String|Yes|身份证|
|language|String|Yes|语言（直接返回中文）|
|character_description|String|Yes|口碑|
|detailed_address|String|Yes|居住地址|
|serving_districts|JSON Array of District|Yes|服务区域|
|serving_hospitals|JSON Array of Hospital|Yes|服务医院|
|skill_certificate_text|String|Yes|技能证书|
|specialty|String|Yes|护理特长 `"AAA,BBB,CCC"`|
|care_case|JSON Array of Care_case |Yes|护理案例|


##Use cases
###Login
登录
http://domain/api/v1/caregiver/auth
### Request

|参数名|类型|必填|描述|
|--------|--------|--------|--------|
|phone_number|String|Yes|电话|
|password|String|Yes|新密码|

###Response
|参数名|类型|必填|描述|
|--------|--------|--------|--------|
|access_token|String|Yes|Access Token|
|user_id|String|Yes|ID of User|
####E.G
```JSON
{
	"access_token": "6STMi0WA5OZqRcctEcympgJGMQxO_OCv",
	"user_id": 8
}
```

###Get All Districts
获取所有可服务地区
http://domain/api/v1/common/districts
### Request
无

###Response
|参数名|类型|必填|描述|
|--------|--------|--------|--------|
|access_token|String|Yes|Access Token|
|user_id|String|Yes|ID of User|
####E.G
```JSON
{
	"access_token": "6STMi0WA5OZqRcctEcympgJGMQxO_OCv",
	"user_id": 8
}
```

###Upload Skill Certificate Image

上传图片文件，获取图片URL
#####Request：
`POST /api/v1/caregiver/upload?access-token=888`
Content-Type: multipart/form-data
Name: file[skill_certificate_image_filename]
Request Headear Example:

```
POST /api/v1/caregiver/upload?access-token=888 HTTP/1.1
...
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary35LGUlFqej51MAab
...
```

Request Payload Example:

```
------WebKitFormBoundary35LGUlFqej51MAab
Content-Disposition: form-data; name="file[skill_certificate_image_filename]"; filename="aa.png"
Content-Type: image/png

...file data...

------WebKitFormBoundary35LGUlFqej51MAab

```



Order Status

|状态|可评价|显示评价过|
|--------|--------|--------|
|未付款|||
|待接单|||
|已接单|||
|服务中|||
|待确认|YES||
|待评价|YES||
|退款|| |
|取消|| |
|完成|YES||
|退款中|| ||
