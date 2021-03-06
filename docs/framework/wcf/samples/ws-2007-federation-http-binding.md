---
title: WS 2007 联合 HTTP 绑定
ms.date: 03/30/2017
ms.assetid: 91c1b477-a96e-4bf5-9330-5e9312113371
ms.openlocfilehash: 550a88552658864e3eedb70463e676ad6f9a2511
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "62007508"
---
# <a name="ws-2007-federation-http-binding"></a>WS 2007 联合 HTTP 绑定
此示例演示 <xref:System.ServiceModel.WS2007FederationHttpBinding> 的用法，它是一个标准绑定，可用来生成支持 1.3 版 WS-Trust 规范的联合方案。  
  
> [!NOTE]
>  本主题的最后介绍了此示例的设置过程和生成说明。  
  
 此示例由基于控制台的客户端程序 (Client.exe)、基于控制台的安全令牌服务程序 (Securitytokenservice.exe) 和基于控制台的服务程序 (Service.exe) 组成。 服务实现用来定义“请求/答复”通信模式的协定。 该协定由 `ICalculator` 接口定义，此接口公开数学运算（`Add`、`Subtract`、`Multiply` 和 `Divide`）。 客户端从安全令牌服务 (STS) 获取安全令牌，向服务发出同步请求，以进行给定数学运算。 服务使用结果进行回复。 客户端活动显示在控制台窗口中。  
  
 此示例使用 `ICalculator` 元素提供 `ws2007FederationHttpBinding` 协定。 下面的代码演示了此绑定在客户端上的配置。  
  
```xml  
<bindings>  
  <ws2007FederationHttpBinding>  
    <binding name="ServiceFed" >  
      <security mode ="Message">  
        <message issuedKeyType ="SymmetricKey"  
                 issuedTokenType ="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1" >  
          <!-- Endpoint address and binding for Security Token Service -->  
          <issuer address ="http://localhost:8000/sts/windows"   
                  binding ="ws2007HttpBinding" />                
        </message>  
      </security>  
    </binding>  
  </ws2007FederationHttpBinding>  
</bindings>  
```  
  
 上[\<安全 >](../../../../docs/framework/configure-apps/file-schema/wcf/security-element-of-ws2007federationhttpbinding.md)，则`security`值指定应使用的安全模式。 在此示例中，`message`使用安全，这就是为什么[\<消息 >](../../../../docs/framework/configure-apps/file-schema/wcf/message-element-of-ws2007federationhttpbinding.md)内指定[\<安全 >](../../../../docs/framework/configure-apps/file-schema/wcf/security-element-of-ws2007federationhttpbinding.md)。 [\<颁发者 >](../../../../docs/framework/configure-apps/file-schema/wcf/issuer.md)元素内的[\<消息 >](../../../../docs/framework/configure-apps/file-schema/wcf/message-element-of-ws2007federationhttpbinding.md)指定地址和 sts，以便客户端可以向客户端颁发安全令牌的绑定对进行身份验证`ICalculator`服务。  
  
 下面的代码演示了此绑定在服务上的配置。  
  
```xml  
<bindings>  
  <ws2007FederationHttpBinding>  
    <binding name="ServiceFed" >  
      <security mode ="Message">  
        <message issuedKeyType ="SymmetricKey"  
                 issuedTokenType ="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1" >  
          <!-- Metadata address for Security Token Service -->  
          <issuerMetadata address ="http://localhost:8000/sts/mex" >  
            <identity>  
              <certificateReference storeLocation ="CurrentUser"   
                                    storeName="TrustedPeople"   
                                    x509FindType ="FindBySubjectDistinguishedName"   
                                    findValue ="CN=STS" />  
            </identity>  
          </issuerMetadata>  
        </message>  
      </security>  
    </binding>  
  </ws2007FederationHttpBinding>  
</bindings>  
```  
  
 上[\<安全 >](../../../../docs/framework/configure-apps/file-schema/wcf/security-element-of-ws2007federationhttpbinding.md)，则`security`值指定应使用的安全模式。 在此示例中，`message`使用安全，这就是为什么[\<消息 >](../../../../docs/framework/configure-apps/file-schema/wcf/message-element-of-ws2007federationhttpbinding.md)内指定[\<安全 >](../../../../docs/framework/configure-apps/file-schema/wcf/security-element-of-ws2007federationhttpbinding.md)。 [ \<IssuerMetadata >](../../../../docs/framework/configure-apps/file-schema/wcf/issuermetadata.md)元素的`ws2007FederationHttpBinding`内部[\<消息 >](../../../../docs/framework/configure-apps/file-schema/wcf/message-element-of-ws2007federationhttpbinding.md)指定地址和标识，可用于检索终结点STS 的元数据。  
  
 下面的代码演示了该服务的行为。  
  
```xml  
<behaviors>  
  <serviceBehaviors>  
    <behavior name ="ServiceBehaviour" >  
      <serviceDebug includeExceptionDetailInFaults ="true"/>  
      <serviceMetadata httpGetEnabled ="true"/>  
      <serviceCredentials>  
        <issuedTokenAuthentication>  
          <knownCertificates>  
            <add storeLocation ="LocalMachine"  
                 storeName="TrustedPeople"  
                 x509FindType="FindBySubjectDistinguishedName"  
                 findValue="CN=STS" />  
          </knownCertificates>  
        </issuedTokenAuthentication>  
        <serviceCertificate storeLocation ="LocalMachine"  
                            storeName ="My"  
                            x509FindType ="FindBySubjectDistinguishedName"  
                            findValue ="CN=localhost"/>  
      </serviceCredentials>  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
```  
  
 [ \<IssuedTokenAuthentication >](../../../../docs/framework/configure-apps/file-schema/wcf/issuedtokenauthentication-of-servicecredentials.md)> 使服务可以指定它允许客户端身份验证期间提供的令牌上的约束。 此配置指定该服务接受由主题名称为 CN=STS 的证书签名的令牌。  
  
 STS 使用标准的 <xref:System.ServiceModel.WS2007HttpBinding> 提供单个终结点。 服务响应客户端对令牌的请求。 如果客户端使用 Windows 帐户进行身份验证，则服务将颁发一个令牌，该令牌以声明的形式包含客户端的用户名。 在创建令牌的过程中，STS 使用与 CN=STS 证书关联的私钥对令牌进行签名。 另外，它还创建对称密钥并使用与 CN=localhost 证书关联的公钥对该密钥进行加密。 在向客户端返回令牌的过程中，STS 还返回对称密钥。 客户端向 `ICalculator` 服务出示所颁发的令牌，并通过使用密钥对消息进行签名来证明客户端知道该对称密钥。  
  
 运行示例时，对安全令牌的请求显示在 STS 控制台窗口中。 操作请求和响应显示在客户端和服务控制台窗口中。 在任何一个控制台窗口中按 Enter 可以关闭应用程序。  

```
Add(100,15.99) = 115.99
Subtract(145,76.54) = 68.46
Multiply(9,81.25) = 731.25
Divide(22,7) = 3.14285714285714
Press <ENTER> to terminate client.
```

 通过运行此示例随附的 Setup.bat 文件，可以用相关的证书将服务器和 STS 配置为运行自承载应用程序。 该批处理文件在 LocalMachine/TrustedPeople 证书存储区中创建两个证书。 第一个证书的主题名称为 CN=STS，STS 使用该证书对颁发给客户端的安全令牌进行签名。 第二个证书的主题名称为 CN=localhost，STS 使用该证书，按照服务能够解密的方式对密钥进行加密。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 请确保您具有执行[的 Windows Communication Foundation 示例的一次性安装过程](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 使用管理员特权打开 Visual Studio 开发人员命令提示符并运行 Setup.bat 文件以创建所需的证书。  
  
 该批处理文件使用随 Windows SDK 分发的 Certmgr.exe 和 Makecert.exe。 但是，您必须在 Visual Studio 命令提示中运行 Setup.bat，脚本才能找到这些工具。  
  
1. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)中的说明进行操作。  
  
2. 若要在单或跨计算机配置中运行示例，请按照中的说明[运行 Windows Communication Foundation 示例](../../../../docs/framework/wcf/samples/running-the-samples.md)。 如果使用的[!INCLUDE[windowsver](../../../../includes/windowsver-md.md)]，则必须运行 Service.exe、 Client.exe 和 SecurityTokenService.exe 使用提升的权限 (右键单击文件，然后单击**以管理员身份运行**)。  
  
> [!IMPORTANT]
>  您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  如果此目录不存在，请转到[Windows Communication Foundation (WCF) 和.NET Framework 4 的 Windows Workflow Foundation (WF) 示例](https://go.microsoft.com/fwlink/?LinkId=150780)若要下载所有 Windows Communication Foundation (WCF) 和[!INCLUDE[wf1](../../../../includes/wf1-md.md)]示例。 此示例位于以下目录：  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\WS\WS2007FederationHttp`  
