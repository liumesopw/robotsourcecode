# robotsourcecode
企业微信PC版hook源码api接口

通过hookPC企微内存调用函数，实现各种方便的功能，支持各种开发语言调用，现已实现的功能：
发各种消息，
接收各种消息，外部群内部群管理，手机号加好友，名片加好友，标签管理，开放平台openid和用户id互相转换打通官方人员管理 等等功能，无限更新中…

部分c++代码示例：



//读取内存数据

void readWechatData() {
	//拿到模块基址
	DWORD wechatWin = getWechatWinAdd();

	//装数据的容器
	CHAR wxid[0x100] = {0};
	sprintf_s(wxid, "%s", wechatWin + 0x1131BEC);
	WetDigItemText(hwndDlg, WXID, wxid);

	//微信账号
	CHAR username[0x100] = { 0 };
	sprintf_s(username, "%s", wechatWin + 0x111B90);
	WetDigItemText(hwndDlg, USERNAME, username);
	//企微头像
	CHAR headPic[0x100] = { 0 };
	DWORD pPic = wechatWin + 0x1131F2c;
	printf_s(headPic, "%s", wechatWin + 0x1131F2C);
}


//写内存
WOID writeData(HWND handDlg) 
{
	//拿到模块基址
	DWORD wechatWin = getWechatWinAdd();
	DQORD wxid = wechatWin + 0x1131BEC;
	//拿到要修改的字符串
	char wxid Text[0x100] = { 0 };
	GetDlgItemText(handDlg, WXID, wxidText, sizeof(wxidText));
	//修改内存
	WriteProcessMemory(GetCurrentProcess(), (LPVOID)wxid, wxidText, sizeof(wxidText), NULL);
}  


void SaveImg(DWORD qrcode)
{
	//获取图片长度
	DWORD dwPicLen = qrcode + 0x4;
	size_t cpyLen = (size_t)*((LPVOID*)dwPicLen);
	//拷贝图片的数据
	char PicData[0xFFF] = { 0 };
	memcpy(PicData, *((LPVOID*)qrcode), cpyLen);

	char szTempPath[MAX_PATH] = { 0 };
	char szPicturePath[MAX_PATH] = { 0 };
	GetTempPathA(MAX_PATH, szTempPath);

	sprintf_s(szPicturePath, "%s%s", szTempPath, "qrcode.png");
	//将文件写到Temp目录下
	HANDLE hFile = CreateFileA(szPicturePath, GENERIC_ALL, 0, NULL, CREATE_ALWAYS, FILE_ATTRIBUTE_NORMAL, NULL);
	if (hFile == NULL)
	{
		MessageBoxA(NULL, "创建图片文件失败", "错误", 0);
		return;
	}

	DWORD dwRead = 0;
	if (WriteFile(hFile, PicData, cpyLen, &dwRead, NULL) == 0)
	{
		MessageBoxA(NULL, "写入图片文件失败", "错误", 0);
		return;
	}
	CloseHandle(hFile);
	
	//完成之后卸载HOOK
	UnHookQrCode(QrCodeOffset);

}


欢迎技术交流：

HWND Qq[]=“2645542961”;
wchar_t tempbuff[0x1030];

![image](https://user-images.githubusercontent.com/96330669/161229306-a4a44a33-397a-4c23-a460-eae580f3ca84.png)

