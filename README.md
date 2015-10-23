#API

##基础

###HTTP Status Code
| Code | 描述 |
|--------|--------|
|200|成功|
|204|资源为找到|
|401|Token 过期|
|403|密码错误|

##Models

| Model | ... |
|--------|--------|
|Order|        |
|Caregiver|        |
|Hospital|医院|
|District|        |
|Schedule|        |

### Hospital
```
{
	"id": 1,
	"district_id": 2,
	"creator_user_id": 1,
	"name": "解放军总医院",
	"code": "",
	"address": "",
	"created_at": 0,
	"updated_at": 0,
	"district": {
		"id": 2,
		"city_id": 1,
		"name": "清华大学西门",
		"city": {
			"id": 1,
			"province_id": 1,
			"name": "北京",
			"province": {
				"id": 1,
				"name": "海淀区"
			}
		}
	}
}
```

###cities

```
[{
		"id": 1,
		"province_id": 1,
		"name": "北京",
		"province": {
			"id": 1,
			"name": "海淀区"
		}
	}]
```

###districts

```
[{
		"id": 1,
		"city_id": 1,
		"name": "五道口",
		"city": {
			"id": 1,
			"province_id": 1,
			"name": "北京",
			"province": {
				"id": 1,
				"name": "海淀区"
			}
		}
	}, {
		"id": 2,
		"city_id": 1,
		"name": "清华大学西门",
		"city": {
			"id": 1,
			"province_id": 1,
			"name": "北京",
			"province": {
				"id": 1,
				"name": "海淀区"
			}
		}
	}, {
		"id": 3,
		"city_id": 1,
		"name": "西苑",
		"city": {
			"id": 1,
			"province_id": 1,
			"name": "北京",
			"province": {
				"id": 1,
				"name": "海淀区"
			}
		}
	}, {
		"id": 4,
		"city_id": 1,
		"name": "上地",
		"city": {
			"id": 1,
			"province_id": 1,
			"name": "北京",
			"province": {
				"id": 1,
				"name": "海淀区"
			}
		}
	}]
```

### 护工信息

| column | column |
|--------|--------|
|grade|primary, secondary,tertiary|


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

###Get All Hospitals


###Get Orders


###Get Caregiver Detail


###Reset Password


###Get Schedules

###Modify Schedule
###Upload Skill Certificate Image
####Step 1
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
#####Response:

Caregiver Model

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






#####例如：
`http://101.200.74.251/test-api/caregiver-upload-image-post`

####Step 2




###Modify Caregiver


###Modify Order

###Feedback
