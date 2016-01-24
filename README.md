###SSKeychain
#### SSKeychain的使用心得
####SSKeyChains对苹果安全框架API进行了简单封装，支持对存储在钥匙串中密码、账户进行访问，包括读取、删除和设置。SSKeyChain的作者是大名鼎鼎的SSToolkit的作者samsoffes。
####1.准备篇
####项目地址：https://github.com/Mingriweiji-github/-SSKeychain

在工程中加入SSKeyChain

- 在工程中加入Security.framework框架。

- 把SSKeychain.h和SSKeychain.m 以及SSKeychainQuery.h SSKeychainQuery.m 加到我们的项目文件夹。

通过以下类方法来使用SSKeyChain（请查看SSKeyChain.h）：

+ (NSArray *)allAccounts;

+ (NSArray *)accountsForService:(NSString *)serviceName;

+ (NSString *)passwordForService:(NSString *)serviceNameaccount:(NSString *)account;

+ (BOOL)deletePasswordForService:(NSString *)serviceNameaccount:(NSString *)account;

+ (BOOL)setPassword:(NSString *)password forService:(NSString*)serviceName account:(NSString *)account;

###2.使用篇
- 在工程中加入Security.framework框架。

- 把SSKeychain.h和SSKeychain.m 以及SSKeychainQuery.h SSKeychainQuery.m 加到我们的项目文件夹。
- 报错位置  #import SSKeychain/SSKeychainQuery.h

- 解决方法
注释//#import SSKeychain/SSKeychainQuery.h
直接导入#import SSKeychainQuery.h

- 具体方法如下 在需要使用的类 例如 AppDelegate里先写两个宏定义

- define keychain_service @"uuid"

- define keychain_account @"appuuid"

- @implementation AppDelegate

-  pragma mark SSKeychain 获取不变的UUID

- -(NSString *)getUUID
{
    NSString *strUUID = [SSKeychain passwordForService:keychain_service account:keychain_account];
    NSError *error=nil;

    if (strUUID==nil||[strUUID isEqualToString:@"" ]||strUUID.length==0)
    {
        CFUUIDRef uuid = CFUUIDCreate(NULL);

        assert(uuid != NULL);

        CFStringRef uuidStr = CFUUIDCreateString(NULL, uuid);

        BOOL  succcess= [SSKeychain setPassword:[NSString stringWithFormat:@"%@",uuidStr] forService:keychain_service account:keychain_account  error:&error];
        if(succcess)
        {
            NSLog(@"keychain success 获取的UUID is %@",strUUID);
        }
    }

//    BOOL delete = [SSKeychain deletePasswordForService:keychain_service account:keychain_account];
//    if (delete) {
//
//        NSLog(@"delete is success");
//    }
    NSLog(@"SSKeychain 获取不变的UUID is %@",strUUID);

    return strUUID;
}
