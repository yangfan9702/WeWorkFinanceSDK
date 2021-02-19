# WeWorkFinanceSDK
企业微信会话存档SDK（基于企业微信C版官方SDK封装），暂时只支持在`linux`环境下使用当前SDK。

### 使用方式

`clone`项目到自己项目内并`cd`到`WeWorkFinanceSDK/lib`文件夹内执行`export LD_LIBRARY_PATH=$(pwd)`命令设置动态链接库检索地址，然后在项目内引入当前包即可直接使用。

### Example

```go
package main

import (
	"encoding/json"
	"fmt"
	"github.com/NICEXAI/WeWorkFinanceSDK"
)

func main()  {
	corpID := "xxxxxxxxxxxx"
	corpSecret := "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
	rsaPrivateKey := `
-----BEGIN RSA PRIVATE KEY-----
xxxxxxxxxxxxxxxxxxxxxxxxxxx
-----END RSA PRIVATE KEY-----
	`
	//初始化客户端
	client, err := WeWorkFinanceSDK.NewClient(corpID, corpSecret, rsaPrivateKey)
	if err != nil {
		fmt.Printf("SDK 初始化失败：%v \n", err)
		return
	}
	//获取消息
	chatDataList, err := client.GetChatData(0, 100, "", "", 3)
	if err != nil {
		fmt.Printf("消息同步失败：%v \n", err)
		return
	}
	for _, chatData := range chatDataList {
		//消息解密
		chatInfo, err := client.DecryptData(chatData.EncryptRandomKey, chatData.EncryptChatMsg)
		if err != nil {
			fmt.Printf("消息解密失败：%v \n", err)
			return
		}
		str, _ := json.Marshal(chatInfo)
		fmt.Println(string(str))
	}
}
```
