---
title: "Adicionar nomes de domínio de terceiro nível"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5b4a362-1881-4024-ae4e-cc3b05e50103
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 64bf24e45155fdd981e2061b3de7ebce1c53b36c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="add-third-level-domain-names"></a>Adicionar nomes de domínio de terceiro nível

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode adicionar a capacidade dos usuários de solicitar nomes de domínio de terceiro nível no Assistente de configuração de nome de domínio. Você pode fazer isso criando e instalando um assembly de código que é usado pelo Gerenciador de domínio no sistema operacional.  
  
## <a name="create-a-provider-of-third-level-domain-names"></a>Crie um provedor de nomes de domínio de terceiro nível  
 Você pode disponibilizar os nomes de domínio de terceiro nível criando e instalando um assembly de código que fornece os nomes de domínio ao assistente. Para fazer isso, você deve concluir as seguintes tarefas:  
  
-   [Adicione uma implementação da interface IDomainSignupProvider ao assembly](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup)  
  
-   [Adicione uma implementação da interface IDomainMaintenanceProvider ao assembly](Add-Third-Level-Domain-Names.md#BKMK_DomainMaintenance)  
  
-   [Assine o assembly com uma assinatura de Authenticode](Add-Third-Level-Domain-Names.md#BKMK_SignAssembly)  
  
-   [Instale o assembly no computador de referência](Add-Third-Level-Domain-Names.md#BKMK_InstallAssembly)  
  
-   [Reiniciar o serviço de gerenciamento de nome de domínio do Windows Server](Add-Third-Level-Domain-Names.md#BKMK_RestartService)  
  
###  <a name="BKMK_DomainSignup"></a>Adicione uma implementação da interface IDomainSignupProvider ao assembly  
 A interface IDomainSignupProvider é usada para adicionar ofertas de domínio ao assistente.  
  
##### <a name="to-add-the-idomainsignupprovider-code-to-the-assembly"></a>Para adicionar o código IDomainSignupProvider ao assembly  
  
1.  Abra o Visual Studio 2008 como administrador clicando com o programa no **iniciar** menu e selecionando **executar como administrador**.  
  
2.  Clique em **arquivo**, clique em **nova**e clique em **projeto**.  
  
3.  No **novo projeto** caixa de diálogo, clique em **Visual c#**, clique em **biblioteca de classes**, insira um nome para a solução e, em seguida, clique em **Okey**.  
  
4.  Renomeie o arquivo Class1.cs. Por exemplo, MyDomainNameProvider.cs  
  
5.  Adicione referências para os arquivos Wssg.Web.DomainManagerObjectModel.dll, CertManaged.dll, WssgCertMgmt.dll e WssgCommon.dll.  
  
6.  Adicione as seguintes instruções using.  
  
    ```c#  
  
    using System.Collections.ObjectModel;  
    using System.Net;  
    using System.Net.Sockets;  
    using Microsoft.WindowsServerSolutions.Certificates;  
    using Microsoft.WindowsServerSolutions.CertificateManagement;  
    using Microsoft.WindowsServerSolutions.Common;  
    using Microsoft.Win32;  
    ```  
  
7.  Altere o namespace e o cabeçalho de classe para coincidir com o exemplo a seguir.  
  
    ```c#  
    namespace Microsoft.WindowsServerSolutions.RemoteAccess.Domains  
    {  
        public class MyDomainNameProvider : IDomainSignupProvider  
        {  
        }  
    }  
    ```  
  
8.  Adicione o método Initialize e as variáveis necessárias à classe, que define as ofertas apresentadas no assistente.  
  
    > [!NOTE]
    >  O método Initialize define um identificador para o provedor de domínio que deve ser exclusivo. Uma maneira comum de fazer isso é definir um GUID como o identificador. Para obter mais informações sobre como criar um GUID, consulte [criar Guid (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098).  
  
     O exemplo de código a seguir mostra o método Initialize.  
  
    ```c#  
  
    static readonly Guid MyID = new Guid("8C999DF5-696A-47af-822D-94F1673D3397");  
    public Guid ID { get { return MyID; } }  
    public string Name { get { return "My Provider"; } }  
    List<Offering> offerings = new List<Offering>();  
  
    public void Initialize(DomainProviderSettings config)  
    {  
       var offer1 = new Offering()  
       {  
          Description = "My Domain Provider",  
          Name = "Offering 1",  
          ProviderID = ID,  
          MoreInfoUrl = new Uri("http://www.contoso.com"),  
          MembershipServiceName = "My Membership",  
          EulaUrl = new Uri("http://www.contoso.com"),  
       };  
  
       this.offerings.Add(offer1);  
       RegistryKey key =   
          Registry.LocalMachine.CreateSubKey(@"Software\Microsoft\Windows Server\Domain Manager\Settings");  
       key.Close();  
    }  
    ```  
  
9. Adicione o método GetOfferings, que retorna a lista de ofertas que foi inicializada na etapa anterior. O exemplo de código a seguir mostra o método GetOfferings.  
  
    ```c#  
  
    public ReadOnlyCollection<Offering> GetOfferings()  
    {  
       return this.offerings.AsReadOnly();  
    }  
    ```  
  
10. Adicione o método FindOfferingForDomain, que retorna a oferta da lista. O exemplo de código a seguir mostra o método FindOfferingForDomain.  
  
    ```  
  
    public Offering FindOfferingForDomain(string domain)  
    {  
       // Return the offering that has the domain name.  
       return offerings[0];  
    }  
  
    ```  
  
11. Adicione o método SetCredentials, que define as credenciais que são necessárias para acessar as ofertas. O exemplo de código a seguir mostra o método SetCredentials.  
  
    ```c#  
  
    private string currentUser { get; set; }  
    private string currentPassword { get; set; }  
  
    public bool SetCredentials(DomainNameRequest request,   
       DomainProviderCredentials credentials, bool validate)  
    {  
       currentUser = credentials.UserName;  
       currentPassword = credentials.Password;  
       if (validate)  
       {  
          return ValidateCredentials();  
       }  
  
       return true;  
    }  
    ```  
  
12. Adicione o método ValidateCredentials, que valida as credenciais definidas por SetCredentials. O exemplo de código a seguir mostra o método ValidateCredentials.  
  
    ```c#  
  
    public static readonly string offerUser = "user1@contoso.com";  
    public static readonly string offerPassword = "password1";  
  
    public bool ValidateCredentials()  
    {  
       if (IsUser())  
          return string.Equals(currentPassword, offerPassword);  
       else  
          return false;  
    }   
  
    private bool IsUser()  
    {  
       return string.Equals(currentUser, offerUser, StringComparison.OrdinalIgnoreCase);  
    }  
    ```  
  
13. Adicione o método GetAvailableDomainRoots, que retorna a lista de nomes de domínio raiz com suporte na oferta especificada na solicitação. Essa lista de nomes de domínio raiz não deve estar vazia. O exemplo de código a seguir mostra o método GetAvailableDomainRoots.  
  
    ```c#  
  
    public ReadOnlyCollection<string> GetAvailableDomainRoots(DomainNameRequest request)  
    {  
       List<string> list = new List<string>();  
       list.Add("domain1.com");  
       list.Add("domain1.org");  
  
       return list.AsReadOnly();  
    }  
    ```  
  
14. Adicione o método GetUserDomainNames, que retorna uma lista de nomes de domínio que o usuário atual já possui, em relação à oferta atual. Essa lista pode estar vazia. O exemplo de código a seguir mostra o método GetUserDomainNames.  
  
    ```c#  
  
    public static readonly string AvailableDomain1 = "available.domain1.com",  
       AvailableDomain2 = "available.domain2.com";  
    public static readonly string OccupiedDomain1 = "occupied.domain1.com",  
       OccupiedDomain2 = "occupied.domain2.com";  
  
    public ReadOnlyCollection<string> GetUserDomainNames(DomainNameRequest request)  
    {  
       var userDomains = new List<string>();  
       userDomains.Add(OccupiedDomain1);  
       userDomains.Add(AvailableDomain1);  
  
       return userDomains.AsReadOnly();  
    }  
    ```  
  
15. Adicione o método GetUserDomainQuota, que retorna o número máximo de domínios que a oferta especificada permite. Se um máximo não for aplicável, esse método deverá retornar 0. O exemplo a seguir mostra o método GetUserDomainQuota.  
  
    ```c#  
  
    public int GetUserDomainQuota(DomainNameRequest request)  
    {  
       return 0;  
    }  
    ```  
  
16. Adicione o método CheckDomainAvailability, que verifica a disponibilidade do nome do domínio solicitado e pode retornar uma lista de sugestões. O exemplo de código a seguir mostra o método CheckDomainAvailability.  
  
    ```c#  
  
    public bool CheckDomainAvailability(DomainNameRequest request,   
       out ReadOnlyCollection<string> suggestions)  
    {  
       suggestions = null;  
  
       return true;  
    }  
    ```  
  
17. Adicione o método CommitDomain, que confirma o nome de domínio solicitado. Conclusão bem-sucedida desse método implica que o nome de domínio está associado à conta de usuário, o provedor de manutenção será chamado imediatamente para recuperar o certificado se o estado for FullyOperational e o provedor e a oferta se tornarão ativos. O exemplo de código a seguir mostra o método CommitDomain.  
  
    ```c#  
  
    public DomainStatus CommitDomain(DomainNameRequest request)  
    {              
       ReadOnlyCollection<string> suggestions;  
       if (!CheckDomainAvailability(request, out suggestions))  
       {  
          throw new DomainException(FailureReason.InvalidDomainName, null, null);  
       }  
  
       return DomainStatus.Ready;  
    }  
    ```  
  
18. Adicione o método ReleaseDomain, que informa ao provedor que o usuário deseja liberar o nome de domínio. O exemplo de código a seguir mostra o método ReleaseDomain.  
  
    ```c#  
  
    public bool ReleaseDomain(DomainNameRequest request)  
    {  
       return true;  
    }  
    ```  
  
19. Adicione o método GetProviderLandingUrl, que retorna a URL para a página de aterrissagem no fluxo de trabalho de inscrição de domínio. O exemplo de código a seguir mostra o método GetProviderLandingUrl.  
  
    ```c#  
  
    public Url GetProviderLandingUrl(DomainNameRequest request)  
    {  
       return new Url("www.contoso.com");  
    }  
    ```  
  
20. Adicione o método GetDomainMaintenanceProvider, que retorna uma instância de IDomainMaintenanceProvider, usado para tarefas de manutenção de domínio. Esse método é chamado depois que o método CommitDomain é bem-sucedido e quando o Gerenciador de domínio é iniciado. O exemplo de código a seguir mostra o método GetDomainMaintenanceProvider.  
  
    ```c#  
  
    public IDomainMaintenanceProvider GetDomainMaintenanceProvider()  
    {  
       return new MyDomainMaintenanceProvider();  
    }  
    ```  
  
21. Salve o projeto e não o feche porque você adicionará a ele com o próximo procedimento. Você não conseguirá compilar o projeto até você concluir o próximo procedimento.  
  
###  <a name="BKMK_DomainMaintenance"></a>Adicione uma implementação da interface IDomainMaintenanceProvider ao assembly  
 IDomainMaintenanceProvider é usada para manter o domínio depois que ela é criada.  
  
##### <a name="to-add-the-idomainmaintenanceprovider-code-to-the-assembly"></a>Para adicionar o código IDomainMaintenanceProvider ao assembly  
  
1.  Adicione o cabeçalho de classe para o provedor de manutenção de domínio. Certifique-se de que o nome que você define para o provedor corresponde ao nome no método GetDomainMaintenanceProvider previamente definido.  
  
    ```c#  
  
    public class MyDomainMaintenanceProvider : IDomainMaintenanceProvider  
    {  
    }  
    ```  
  
2.  Adicione o método Activate, que define o provedor ativo. O exemplo de código a seguir mostra o método Activate.  
  
    ```c#  
  
    string DomainName { get; set; }  
    protected DomainProviderSettings Settings { get; set; }  
  
    public void Activate(DomainProviderSettings settings,   
       DomainNameConfiguration config, DomainProviderCredentials credentials)  
    {  
       Settings = settings;  
       SetCredentials(credentials);  
       DomainName = config.AutoConfiguredAnywhereAccessFullName.Punycode;  
    }  
    ```  
  
3.  Adicione o método Deactivate, que é usado para desativar todas as ações. O exemplo de código a seguir mostra o método Deactivate.  
  
    ```  
  
    public void Deactivate()  
    {  
       //Deactivate all actions  
    }  
  
    ```  
  
4.  Adicione o método SetCredentials, que atualiza as credenciais do usuário. Por exemplo, esse método pode ser chamado para atualizar credenciais que não são mais válidas. O exemplo de código a seguir mostra o método SetCredentials.  
  
    ```c#  
  
    protected DomainProviderCredentials Credentials { get; set; }  
  
    public bool SetCredentials(DomainProviderCredentials credentials)  
    {  
       this.Credentials = credentials;  
  
       return true;  
    }  
    ```  
  
5.  Adicione o método ValidateCredentials, que valida as credenciais especificadas. O exemplo de código a seguir mostra o método ValidateCredentials.  
  
    ```c#  
  
    public static readonly string offerUser = "user1@contoso.com";  
    public static readonly string offerPassword = "password1";  
  
    public bool ValidateCredentials()  
    {  
       if (string.Equals(this.Credentials.UserName,   
          offerUser,   
          StringComparison.OrdinalIgnoreCase) &&   
             string.Equals(this.Credentials.Password, offerPassword))  
          return true;  
  
       return false;  
    }  
    ```  
  
6.  Adicione o método GetPublicAddress, que retorna o endereço IP externo do servidor. O exemplo de código a seguir mostra o método GetPublicAddress.  
  
    ```c#  
  
    public IPAddress GetPublicIPAddress()  
    {  
       string PublicIP = "0.0.0.0";  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          PublicIP = (key == null) ? "0.0.0.0" : key.GetValue("PublicIP", "0.0.0.0").ToString();  
       }  
       IPAddress ip = IPAddress.Parse(PublicIP);  
  
       if (PublicIP == "0.0.0.0")  
       {  
          string strHostName = Dns.GetHostName();  
          IPHostEntry ipEntry = Dns.GetHostEntry(strHostName);  
  
          IPAddress[] addr = ipEntry.AddressList;  
          foreach (IPAddress add in addr)  
          {  
             if (add.AddressFamily == AddressFamily.InterNetwork)  
             {  
                return add;  
             }  
          }  
       }  
       else  
       {  
          return IPAddress.Parse(PublicIP);  
       }  
  
       return null;    
    }  
    ```  
  
7.  Adicione o método SubmitCertificateRequest, que envia a solicitação de certificado para o nome de domínio configurado no momento.  
  
    ```c#  
  
    string cert=null;  
  
    public void SubmitCertificateRequest(string certificateRequest)  
    {  
       cert = CertManaged.SubmitRequest(certificateRequest, CertCommon.CAServerFQDN + "\\" +      
          CertCommon.CAName,   
          Microsoft.WindowsServerSolutions.CertificateManagement.CRFlags.Base64Header,   
          CertCommon.CATemplate,   
          EncodingFlags.Base64);  
    }  
    ```  
  
8.  Adicione o método GetCertificateResponse, que retorna a resposta do certificado se o status de domínio for FullyOperational. Esse método é chamado para as duas novas solicitações de certificado e para as solicitações de renovação de certificado. O exemplo de código a seguir mostra o método GetCertificateResponse.  
  
    ```c#  
  
    public string GetCertificateResponse(bool renew)  
    {  
       return cert;  
    }  
    ```  
  
9. Adicione o método SubmitRenewCertificateRequest, que processa a renovação do certificado. O exemplo de código a seguir mostra o método SubmitRenewCertificateRequest.  
  
    ```c#  
  
    public void SubmitRenewCertificateRequest()  
    {  
       // Add certificate renewal code   
    }  
    ```  
  
10. Adicione o método UpdateDNSRecords, que atualiza os registros DNS armazenados pelo provedor. O exemplo de código a seguir mostra o método UpdateDNS.  
  
    ```c#  
  
    public bool UpdateDnsRecords(IList<DnsRecord> records)  
    {  
       string UpdateDNS = "true";  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          UpdateDNS = (key == null) ? "true" : key.GetValue("UpdateDNS", "true").ToString();  
       }  
  
       return UpdateDNS == "true";  
    }  
  
    ```  
  
11. Adicione o método Testconnectionn, que tenta estabelecer uma conexão ao serviço de back-end. Se esse método exigir autenticação, uma exceção apropriada deverá ser acionada. O exemplo de código a seguir mostra o método TestConnection.  
  
    ```c#  
  
    public bool TestConnection()  
    {  
       // Add code to test connection  
  
       return true;  
    }  
    ```  
  
12. Adicione o método GetDomainState, que retorna o estado atual do domínio. O exemplo de código a seguir mostra o método GetDomainState.  
  
    ```c#  
  
    public DomainState GetDomainState()  
    {  
       string domainstatus = "FullyOperational";  
       long expirationDate = 365;  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          domainstatus = (key == null) ? "Ready" : key.GetValue("DomainStatus", "Ready").ToString();  
          expirationDate = Int64.Parse(key.GetValue("ExpirationDate", "365").ToString());  
       }  
  
       switch (domainstatus)  
       {  
          case "Failed":  
             return new DomainState(DomainStatus.Failed,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "Ready":  
             return new DomainState(DomainStatus.Ready,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "InRenewal":  
             return new DomainState(DomainStatus.InRenewal,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "InRenewalCustomerInterventionRequired":  
             return new DomainState(DomainStatus.InRenewalCustomerInterventionRequired,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "Pending":  
             return new DomainState(DomainStatus.Pending,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "PendingCustomerInterventionRequired":  
             return new DomainState(DomainStatus.PendingCustomerInterventionRequired,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "RenewalFailed":  
             return new DomainState(DomainStatus.RenewalFailed,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          default:  
             return new DomainState(DomainStatus.Unknown,   
                null,   
                DateTime.Now.AddDays(expirationDate));                   
          }  
    }  
    ```  
  
13. Adicione o método GetCertificateState, que retorna o estado atual do certificado. O exemplo de código a seguir mostra o método GetCertificateState.  
  
    ```c#  
  
    public CertificateState GetCertificateState(bool renew)  
    {  
       return new CertificateState(CertificateStatus.Ready, string.Empty);  
    }  
    ```  
  
14. Salve e compile a solução.  
  
###  <a name="BKMK_SignAssembly"></a>Assine o assembly com uma assinatura de Authenticode  
 Você deve assinar o assembly com Authenticode para ser usado no sistema operacional. Para obter mais informações sobre como assinar o assembly, consulte [assinando e verificando códigos com Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
###  <a name="BKMK_InstallAssembly"></a>Instale o assembly no computador de referência  
 Coloque o assembly em uma pasta no computador de referência. Anote o caminho da pasta porque você vai inseri-lo no registro na próxima etapa.  
  
### <a name="add-a-key-to-the-registry"></a>Adicionar uma chave ao registro  
 Adicione uma entrada do registro para definir as características e a localização do assembly.  
  
##### <a name="to-add-a-key-to-the-registry"></a>Para adicionar uma chave do registro  
  
1.  No computador de referência, clique em **iniciar**, insira **regedit**e pressione **Enter**.  
  
2.  No painel esquerdo, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**, expanda **gerenciadores de domínio**e, em seguida, expanda **provedores**.  
  
3.  Clique com botão direito **provedores**, aponte para **nova**e clique em **chave**.  
  
4.  Digite o identificador para o provedor como o nome para a chave. O identificador é o GUID que você definiu para o provedor na etapa 8 da [adicione uma implementação da interface IDomainSignupProvider ao assembly](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup).  
  
5.  Clique com botão direito a chave que você acabou criado e clique em **valor de cadeia de caracteres**.  
  
6.  Tipo **Assembly** para o nome da cadeia de caracteres e pressione **Enter**.  
  
7.  Clique com botão direito do novo **Assembly** cadeia de caracteres no painel direito e clique em **modificar**.  
  
8.  Digite o caminho completo para o arquivo de assembly que você criou anteriormente e, em seguida, clique em **Okey**.  
  
9. Clique com botão direito a chave novamente e clique em **valor de cadeia de caracteres**.  
  
10. Tipo **Enabled** para o nome da cadeia de caracteres e pressione **Enter**.  
  
11. Clique com botão direito do novo **Enabled** cadeia de caracteres no painel direito e clique em **modificar**.  
  
12. Tipo **True**e clique em **Okey**.  
  
13. Clique com botão direito a chave novamente e clique em **valor de cadeia de caracteres**.  
  
14. Tipo **tipo** para o nome da cadeia de caracteres e pressione **Enter**.  
  
15. Clique com botão direito do novo **tipo** cadeia de caracteres no painel direito e clique em **modificar**.  
  
16. Digite o nome completo da classe do seu provedor definida no assembly e clique em **Okey**.  
  
###  <a name="BKMK_RestartService"></a>Reiniciar o serviço de gerenciamento de nome de domínio do Windows Server  
 Você deve reiniciar o serviço de gerenciamento de domínio do Windows Server para o provedor se torne disponível para o sistema operacional.  
  
##### <a name="restart-the-service"></a>Reiniciar o serviço  
  
1.  Clique em **iniciar**, tipo **mmc**e pressione **Enter**.  
  
2.  Se o snap-in Serviços não estiver listado no console, adicioná-lo, concluindo as seguintes etapas:  
  
    1.  Clique em **arquivo**e clique em **Adicionar/Remover Snap-in**.  
  
    2.  No **snap-ins disponíveis** listar, clique em **serviços**e clique em **adicionar**.  
  
    3.  No **serviços** caixa de diálogo caixa, certifique-se de que **computador local** está selecionado e clique em **concluir**.  
  
    4.  Clique em **Okey** para fechar o **Adicionar/remover snap-ins** caixa de diálogo.  
  
3.  Clique duas vezes em **serviços**, role para baixo e selecione **gerenciamento de domínio do Windows Server**e clique em **reiniciar o serviço**.  
  
## <a name="see-also"></a>Consulte também  
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)