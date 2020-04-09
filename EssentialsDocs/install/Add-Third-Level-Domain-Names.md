---
title: Adicionar Nomes de Domínio de Terceiro Nível
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: e5b4a362-1881-4024-ae4e-cc3b05e50103
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 659844ce4da6a7ec6311b36b4516875ed468733e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817549"
---
# <a name="add-third-level-domain-names"></a>Adicionar Nomes de Domínio de Terceiro Nível

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

É possível capacitar os usuários para que solicitem nomes de domínio de terceiro nível no Assistente de Configuração de Nome de Domínio. Isso é feito com a criação e a instalação de um assembly de código usado pelo Gerenciador de Domínio no sistema operacional.  
  
## <a name="create-a-provider-of-third-level-domain-names"></a>Criar um provedor de nomes de domínio de terceiro nível  
 Você pode tornar disponíveis os nomes de domínio de terceiro nível criando e instalando um assembly de código que fornece os nomes de domínio para o assistente. Para fazer isso, conclua as seguintes tarefas:  
  
-   [Adicionar uma implementação da interface IDomainSignupProvider ao assembly](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup)  
  
-   [Adicionar uma implementação da interface IDomainMaintenanceProvider ao assembly](Add-Third-Level-Domain-Names.md#BKMK_DomainMaintenance)  
  
-   [Assinar o assembly com uma assinatura Authenticode](Add-Third-Level-Domain-Names.md#BKMK_SignAssembly)  
  
-   [Instalar o assembly no computador de referência](Add-Third-Level-Domain-Names.md#BKMK_InstallAssembly)  
  
-   [Reiniciar o serviço de gerenciamento de nomes de domínio do Windows Server](Add-Third-Level-Domain-Names.md#BKMK_RestartService)  
  
###  <a name="add-an-implementation-of-the-idomainsignupprovider-interface-to-the-assembly"></a><a name="BKMK_DomainSignup"></a>Adicionar uma implementação da interface IDomainSignupProvider ao assembly  
 A interface IDomainSignupProvider é usada para adicionar as ofertas de domínio ao assistente.  
  
##### <a name="to-add-the-idomainsignupprovider-code-to-the-assembly"></a>Para adicionar o código de IDomainSignupProvider ao assembly  
  
1.  Abra o Visual Studio 2008 como administrador clicando com o botão direito do mouse no programa, no menu **Iniciar** e selecione **Executar como administrador**.  
  
2.  Clique em **Arquivo**, em **Novo** e em **Projeto**.  
  
3.  Na caixa de diálogo **Novo Projeto**, clique em **Visual C#** ; clique em **Biblioteca de Classes**, digite um nome para a solução e clique em **OK**.  
  
4.  Renomeie o arquivo Class1.cs. Por exemplo, MyDomainNameProvider.cs  
  
5.  Adicione referências aos arquivos Wssg.Web.DomainManagerObjectModel.dll, CertManaged.dll, WssgCertMgmt.dll e WssgCommon.dll.  
  
6.  Adicionar as seguintes instruções de uso.  
  
    ```c#  
  
    using System.Collections.ObjectModel;  
    using System.Net;  
    using System.Net.Sockets;  
    using Microsoft.WindowsServerSolutions.Certificates;  
    using Microsoft.WindowsServerSolutions.CertificateManagement;  
    using Microsoft.WindowsServerSolutions.Common;  
    using Microsoft.Win32;  
    ```  
  
7.  Altere o namespace e o cabeçalho de classe para corresponder ao exemplo a seguir.  
  
    ```c#  
    namespace Microsoft.WindowsServerSolutions.RemoteAccess.Domains  
    {  
        public class MyDomainNameProvider : IDomainSignupProvider  
        {  
        }  
    }  
    ```  
  
8.  Adicione o método Initialize e as variáveis exigidas para a classe, que define as ofertas apresentadas no assistente.  
  
    > [!NOTE]
    >  O método Initialize define um identificador para o provedor de domínio que deve ser exclusivo. Uma forma comum de fazer isso é definir um GUID como identificador. Para obter mais informações sobre como criar uma GUID, consulte [Criar Guid (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098).  
  
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
  
9. Adicione o método GetOfferings, que retorna a lista de ofertas inicializada na etapa anterior. O exemplo de código a seguir mostra o método GetOfferings.  
  
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
  
11. Adicione o método SetCredentials, que define as credenciais exigidas para o acesso às ofertas. O exemplo de código a seguir mostra o método SetCredentials.  
  
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
  
12. Adicione o método ValidateCredentials, que valida as credenciais definidas pelo SetCredentials. O exemplo de código a seguir mostra o método ValidateCredentials.  
  
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
  
13. Adicione o método GetAvailableDomainRoots, que retorna a lista de nomes de domínio raiz suportados pela oferta especificada na solicitação. Esta lista de nomes de domínio raiz não deve estar vazia. O exemplo de código a seguir mostra o método GetAvailableDomainRoots.  
  
    ```c#  
  
    public ReadOnlyCollection<string> GetAvailableDomainRoots(DomainNameRequest request)  
    {  
       List<string> list = new List<string>();  
       list.Add("domain1.com");  
       list.Add("domain1.org");  
  
       return list.AsReadOnly();  
    }  
    ```  
  
14. Adicione o método GetUserDomainNames, que retorna uma lista de nomes de domínio já possuída pelo usuário atual, relativa à oferta corrente. Essa lista pode estar vazia. O exemplo de código a seguir mostra o método GetUserDomainNames.  
  
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
  
15. Adicione o método GetUserDomainQuota, que retorna o número máximo de domínios permitido pela oferta especificada. Se um máximo não se aplicar, esse método deve retornar 0. O exemplo de código a seguir mostra o método GetUserDomainQuota.  
  
    ```c#  
  
    public int GetUserDomainQuota(DomainNameRequest request)  
    {  
       return 0;  
    }  
    ```  
  
16. Adicione o método CheckDomainAvailability, que verifica a disponibilidade do nome de domínio solicitado e pode retornar uma lista de sugestões. O exemplo de código a seguir mostra o método CheckDomainAvailability .  
  
    ```c#  
  
    public bool CheckDomainAvailability(DomainNameRequest request,   
       out ReadOnlyCollection<string> suggestions)  
    {  
       suggestions = null;  
  
       return true;  
    }  
    ```  
  
17. Adicione o método CommitDomain, que confirma o nome de domínio solicitado. A conclusão bem-sucedida desse método indica que o nome de domínio foi associado à conta de usuário. O provedor de manutenção será chamado imediatamente para recuperar o certificado se o estado for FullyOperational, e o provedor e a oferta serão ativados. O exemplo de código a seguir mostra o método CommitDomain.  
  
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
  
19. Adicione o método GetProviderLandingUrl, que retorna a URL para a página de aterrissagem no fluxo de trabalho de assinatura do domínio. O exemplo de código a seguir mostra o método GetProviderLandingUrl.  
  
    ```c#  
  
    public Url GetProviderLandingUrl(DomainNameRequest request)  
    {  
       return new Url("www.contoso.com");  
    }  
    ```  
  
20. Adicione o método GetDomainMaintenanceProvider, que retorna uma instância do IDomainMaintenanceProvider usada para tarefas de manutenção de domínio. Esse método é requisitado após o êxito do método CommitDomain e quando o Gerenciador de Domínio é iniciado. O exemplo de código a seguir mostra o método GetDomainMaintenanceProvider.  
  
    ```c#  
  
    public IDomainMaintenanceProvider GetDomainMaintenanceProvider()  
    {  
       return new MyDomainMaintenanceProvider();  
    }  
    ```  
  
21. Salve o projeto, mas não o feche, porque você irá adicionar a ele o próximo procedimento. Você não poderá criar o projeto até que tenha concluído o próximo procedimento.  
  
###  <a name="add-an-implementation-of-the-idomainmaintenanceprovider-interface-to-the-assembly"></a><a name="BKMK_DomainMaintenance"></a>Adicionar uma implementação da interface IDomainMaintenanceProvider ao assembly  
 O IDomainMaintenanceProvider é usado para manter o domínio depois que ele é criado.  
  
##### <a name="to-add-the-idomainmaintenanceprovider-code-to-the-assembly"></a>Para adicionar o código de IDomainMaintenanceProvider ao assembly  
  
1.  Adicione o cabeçalho de classe ao provedor de manutenção do domínio. Certifique-se de que o nome definido para o provedor corresponde ao nome no método GetDomainMaintenanceProvider definido anteriormente.  
  
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
  
4.  Adicione o método SetCredentials, que atualiza as credenciais do usuário. Por exemplo, esse método pode ser requisitado para atualizar credenciais que não são mais válidas. O exemplo de código a seguir mostra o método SetCredentials.  
  
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
  
7.  Adicione o método SubmitCertificateRequest, que envia a solicitação de certificado para o nome de domínio configurado atualmente.  
  
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
  
8.  Adicione o método GetCertificateResponse, que retorna a resposta do certificado se o status do domínio for FullyOperational. Esse método é requisitado para as solicitações de novo certificado e de renovação de certificado. O exemplo de código a seguir mostra o método GetCertificateResponse.  
  
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
  
11. Adicione o método TestConnection, que tenta estabelecer uma conexão com serviço de back-end. Se esse método exigir autenticação, uma exceção apropriada deverá ser lançada. O exemplo de código a seguir mostra o método TestConnection.  
  
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
  
14. Salve e crie a solução.  
  
###  <a name="sign-the-assembly-with-an-authenticode-signature"></a><a name="BKMK_SignAssembly"></a>Assinar o assembly com uma assinatura Authenticode  
 Você deve assinar com a Authenticode o assembly para que seja usado no sistema operacional. Para obter mais informações sobre a assinatura do assembly, consulte [Assinando e verificando códigos com Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
###  <a name="install-the-assembly-on-the-reference-computer"></a><a name="BKMK_InstallAssembly"></a>Instalar o assembly no computador de referência  
 Coloque o assembly em uma pasta no computador de referência. Anote o caminho da pasta, porque ele será informado no Registro na próxima etapa.  
  
### <a name="add-a-key-to-the-registry"></a>Adicione uma chave ao Registro  
 Uma entrada de registro é adicionada para definir as características e a localização do assembly.  
  
##### <a name="to-add-a-key-to-the-registry"></a>Para adicionar uma chave ao Registro  
  
1.  No computador de referência, clique em **Iniciar**, insira **regedit** e pressione **Enter**.  
  
2.  No painel esquerdo, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**, expanda **Gerenciadores de Domínio** e, finalmente, expanda **Provedores**.  
  
3.  Clique com o botão direito do mouse em **Provedores**, aponte para **Novo** e clique em **Chave**.  
  
4.  Digite o identificador do provedor como o nome da chave. O identificador é o GUID definido para o provedor na etapa 8 de [Adicionar uma implementação da interface IDomainSignupProvider ao assembly](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup).  
  
5.  Clique com o botão direito do mouse na chave que você acabou de criar e, em seguida, clique em **Valor da Cadeia de Caracteres**.  
  
6.  Digite **Assembly** para o nome da cadeia de caracteres e pressione **Enter**.  
  
7.  Clique com o botão direito do mouse na nova cadeia de caracteres do **Assembly** no painel à direita e clique em **Modificar**.  
  
8.  Digite o caminho completo para o arquivo do assembly criado anteriormente e clique em **OK**.  
  
9. Clique com o botão direito novamente na chave e, em seguida, clique em **Valor da Cadeia de Caracteres**.  
  
10. Digite **Habilitado** para o nome da cadeia de caracteres e pressione **Enter**.  
  
11. Clique com o botão direito na nova cadeia de caracteres **Enabled** no painel à direita e, em seguida, clique em **Modificar**.  
  
12. Digite **Verdadeiro** e, em seguida, clique em **OK**.  
  
13. Clique com o botão direito novamente na chave e, em seguida, clique em **Valor da Cadeia de Caracteres**.  
  
14. Digite **Tipo** para o nome da cadeia de caracteres e pressione **Enter**.  
  
15. Clique com o botão direito na nova cadeia de caracteres **Type** no painel à direita e clique em **Modificar**.  
  
16. Digite o nome completo da classe do provedor definida no assembly e clique em **OK**.  
  
###  <a name="restart-the-windows-server-domain-name-management-service"></a><a name="BKMK_RestartService"></a>Reiniciar o serviço de gerenciamento de nomes de domínio do Windows Server  
 É preciso reiniciar o serviço Gerenciamento de Domínio do Windows Server para que o provedor se torne disponível para o sistema operacional.  
  
##### <a name="restart-the-service"></a>Reiniciar o serviço  
  
1.  Clique em **Iniciar**, digite **mmc** e pressione **Enter**.  
  
2.  Se o snap-in Serviços não estiver listado no console, adicione-o executando as seguintes etapas:  
  
    1.  Clique em **Arquivo** e em **Adicionar/Remover Snap-in**.  
  
    2.  Na lista **Snap-ins disponíveis**, clique em **Serviços** e em **Adicionar**.  
  
    3.  Na caixa de diálogo **Serviços**, certifique-se de que **computador local** esteja selecionado e clique em **Concluir**.  
  
    4.  Clique em **OK** para fechar a caixa de diálogo **Adicionar/Remover snap-ins**.  
  
3.  Clique com o botão direito do mouse em **Serviços**, role para baixo, selecione **Gerenciamento de Domínio do Windows Server** e, a seguir, clique em **Reiniciar o serviço**.  
  
## <a name="see-also"></a>Consulte também  
 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testar a experiência do usuário](Testing-the-Customer-Experience.md)