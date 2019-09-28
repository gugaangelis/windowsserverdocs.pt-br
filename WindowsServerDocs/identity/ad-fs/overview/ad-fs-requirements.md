---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: Requisitos do AD FS 2016
description: Requisitos para instalar o Serviços de Federação do Active Directory (AD FS).
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c8ab160699bc6a961f4fbed6c58cf072a395a313
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407420"
---
# <a name="ad-fs-requirements"></a>Requisitos do AD FS



A seguir estão os requisitos para a implantação de AD FS:  
  
-   [Requisitos de certificado](ad-fs-requirements.md#BKMK_1)  
  
-   [Requisitos de hardware](ad-fs-requirements.md#BKMK_2)  
  
-   [Requisitos de proxy](ad-fs-requirements.md#BKMK_3)  
  
-   [Requisitos de AD DS](ad-fs-requirements.md#BKMK_4)  
  
-   [Requisitos do banco de dados de configuração](ad-fs-requirements.md#BKMK_5)  
  
-   [Requisitos do navegador](ad-fs-requirements.md#BKMK_6)  

-   [Requisitos de rede](ad-fs-requirements.md#BKMK_7)  
  
-   [Requisitos de permissões](ad-fs-requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Requisitos de certificado  
  
### <a name="ssl-certificates"></a>Certificados SSL

Cada AD FS e servidor proxy de aplicativo Web tem um certificado SSL para atender a solicitações HTTPS para o serviço de Federação.  O proxy de aplicativo Web pode ter certificados SSL adicionais para atender a solicitações de aplicativos publicados.

**Recomendação** Use o mesmo certificado SSL para todos os AD FS servidores de Federação e proxies de aplicativo Web. 

**Requirement**

Os certificados SSL em servidores de Federação devem atender aos seguintes requisitos
- O certificado é confiável publicamente (para implantações de produção)
- O certificado contém o valor de EKU (uso avançado de chave) de autenticação de servidor
- O certificado contém o nome do serviço de Federação, como "fs.contoso.com" no assunto ou no nome alternativo da entidade (SAN)
- Para a autenticação de certificado de usuário na porta 443, o certificado contém "certauth. \<nome\>do serviço de Federação ", como" certauth.FS.contoso.com "na San
- Para o registro de dispositivo ou para autenticação moderna para recursos locais usando clientes anteriores ao Windows 10, a SAN deve conter "enterpriseregistration. \<sufixo\>UPN "para cada sufixo UPN em uso na sua organização.

Os certificados SSL no proxy de aplicativo Web devem atender aos seguintes requisitos
- Se o proxy for usado para fazer proxy de AD FS solicitações que usam a autenticação integrada do Windows, o certificado SSL do proxy deverá ser o mesmo (use a mesma chave) que o certificado SSL do servidor de Federação
- Se a propriedade AD FS "ExtendedProtectionTokenCheck" estiver habilitada (a configuração padrão em AD FS), o certificado SSL do proxy deverá ser o mesmo (use a mesma chave) que o certificado SSL do servidor de Federação
- Caso contrário, os requisitos para o certificado SSL de proxy são os mesmos para o certificado SSL do servidor de Federação

### <a name="service-communication-certificate"></a>Certificado de comunicação do serviço
Esse certificado não é necessário para a maioria dos cenários de AD FS, incluindo o Azure AD e o Office 365. Por padrão, AD FS configura o certificado SSL fornecido na configuração inicial como o certificado de comunicação do serviço.

**Recomendação**
- Use o mesmo certificado que você usa para SSL.  

### <a name="token-signing-certificate"></a>Certificado de autenticação de tokens
Esse certificado é usado para assinar tokens emitidos para terceiras partes confiáveis, portanto, os aplicativos de terceira parte confiável devem reconhecer o certificado e sua chave associada, como conhecido e confiável. Quando o certificado de autenticação de tokens é alterado, como quando ele expira e você configura um novo certificado, todas as terceiras partes confiáveis devem ser atualizadas.

**Recomendação** Use o AD FS padrão, gerado internamente, certificados de assinatura de token autoassinados.  

**Requirement**
- Se sua organização exigir que os certificados da PKI corporativa sejam usados para autenticação de token, isso pode ser feito usando o parâmetro SigningCertificateThumbprint do cmdlet Install-AdfsFarm.
- Se você usar os certificados padrão gerados internamente ou certificados registrados externamente, quando o certificado de autenticação de tokens for alterado, você deverá garantir que todas as terceiras partes confiáveis sejam atualizadas com as novas informações de certificado.  Caso contrário, os logons para qualquer terceira parte confiável não atualizada falharão.

### <a name="token-encryptingdecrypting-certificate"></a>Certificado de criptografia/descriptografia de tokens
Esse certificado é usado por provedores de declarações que criptografam tokens emitidos para AD FS.

**Recomendação** Use o AD FS padrão, gerado internamente, certificados de descriptografia de token autoassinados.  

**Requirement**
- Se sua organização exigir que os certificados da PKI corporativa sejam usados para autenticação de token, isso pode ser feito usando o parâmetro DecryptingCertificateThumbprint do cmdlet Install-AdfsFarm.
- Se você usar os certificados padrão gerados internamente ou certificados registrados externamente, quando o certificado de descriptografia de token for alterado, você deverá garantir que todos os provedores de declarações sejam atualizados com as novas informações de certificado.  Caso contrário, os logons que usam qualquer provedor de declarações não atualizado falharão.
  
> [!CAUTION]  
> Os certificados que são usados para\-assinatura de token\-e criptografia\/de tokens são críticos para a estabilidade do serviço de Federação. Os clientes que gerenciam\-sua própria assinatura\-de token\/& a descriptografia de tokens criptografam certificados devem garantir que esses certificados sejam submetidos a backup e estejam disponíveis independentemente durante um evento de recuperação.  

### <a name="user-certificates"></a>Certificados de usuário
- Ao usar a autenticação de certificado de usuário X509 com AD FS, todos os certificados de usuário devem se encadear a uma autoridade de certificação raiz que seja confiável para os AD FS e os servidores de proxy de aplicativo Web.

## <a name="BKMK_2"></a>Requisitos de hardware  
AD FS e os requisitos de hardware do proxy de aplicativo Web (físico ou virtual) são controlados na CPU, portanto, você deve dimensionar seu farm para capacidade de processamento.  
- Use a [planilha de planejamento de capacidade do AD FS 2016](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx) para determinar o número de AD FS e servidores de proxy de aplicativo Web que serão necessários.

Os requisitos de memória e disco para AD FS são razoavelmente estáticos, consulte a tabela abaixo:


|**Requisito de hardware**|**Requisito mínimo**|**Requisito recomendado**|
|----- | ----- |-----|
|RAM|2 GB|4 GB |
|Espaço em disco|32 GB|100 GB |

**Requisitos de hardware SQL Server**

Se você estiver usando SQL Server para seu banco de dados de configuração do AD FS, dimensione o SQL Server de acordo com as recomendações de SQL Server mais básicas.  O tamanho do banco de dados AD FS é muito pequeno e AD FS não coloca uma carga de processamento significativa na instância do banco de dados.  O AD FS, no entanto, se conectar ao banco de dados várias vezes durante uma autenticação, portanto, a conexão de rede deve ser robusta.  Infelizmente, não há suporte para SQL Azure para o banco de dados de configuração do AD FS.
  
## <a name="BKMK_3"></a>Requisitos de proxy  
  
-   Para acesso à extranet, você deve implantar a parte do serviço \- de função do proxy de aplicativo Web da função de servidor de acesso remoto. 

-   Os proxies de terceiros devem dar suporte ao [protocolo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) para ter suporte como um proxy AD FS.  Para obter uma lista de fornecedores de terceiros, consulte as [perguntas frequentes](AD-FS-FAQ.md).

-   AD FS 2016 requer servidores de proxy de aplicativo Web no Windows Server 2016.  Um proxy de nível inferior não pode ser configurado para um farm AD FS 2016 em execução no nível de comportamento de farm 2016.
  
-   Um servidor de Federação e o serviço de função proxy de aplicativo Web não podem ser instalados no mesmo computador.  
  
## <a name="BKMK_4"></a>Requisitos de AD DS  
**Requisitos do controlador de domínio**  
  
- AD FS requer controladores de domínio que executam o Windows Server 2008 ou posterior.

- Pelo menos um controlador de domínio do Windows Server 2016 é necessário para Microsoft Passport for Work.
  
> [!NOTE]  
> Todo o suporte para ambientes com controladores de domínio do Windows Server 2003 foi encerrado. Visite [esta página](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) para obter informações adicionais sobre o ciclo de vida suporte da Microsoft.  
  
**Requisitos de\-nível funcional de domínio**  
  
 - Todos os domínios de conta de usuário e o domínio ao qual os servidores de AD FS são associados devem estar operando no nível funcional de domínio do Windows Server 2003 ou superior.  
  
 - Um nível funcional de domínio do Windows Server 2008 ou superior é necessário para autenticação de certificado de cliente se o certificado for explicitamente mapeado para uma conta de usuário no AD DS.  
  
**Requisitos de esquema**  
  
-   Novas instalações do AD FS 2016 exigem o esquema Active Directory 2016 (versão mínima 85).

-   A geração do nível de comportamento do farm de AD FS (FBL) para o nível 2016 requer o esquema Active Directory 2016 (versão mínima 85).  

  
**Requisitos de conta de serviço**  
  
-   Qualquer conta de domínio padrão pode ser usada como uma conta de serviço para AD FS. Também há suporte para contas de serviço gerenciado de grupo. As permissões necessárias no tempo de execução serão adicionadas automaticamente quando você configurar AD FS.

-   A atribuição de direitos de usuário necessária para a conta de serviço do AD é ' fazer logon como um serviço '

-   As atribuições de direitos de usuário necessárias para o ' NT Service\adfssrv ' e o ' NT Service\drs ' são ' gerar auditorias de segurança ' e ' fazer logon como um serviço '.

-   Contas de serviço gerenciado de grupo exigem pelo menos um controlador de domínio que execute o Windows Server 2012 ou superior.  O GMSA deve residir no contêiner padrão ' CN = Managed Service Accounts '.  

-   Para a autenticação Kerberos, o nome da entidade`HOST/<adfs\_service\_name>`de serviço ' ' deve ser registrado na conta de serviço AD FS. Por padrão, AD FS irá configurá-lo ao criar um novo farm de AD FS.  Se isso falhar, como no caso de uma colisão ou permissões insuficientes, você verá um aviso e deverá adicioná-lo manualmente.  
   
**Requisitos de domínio**  
  
-   Todos os servidores de AD FS devem ser ingressados em um domínio de AD DS.  
  
-   Todos os servidores de AD FS em um farm devem ser implantados no mesmo domínio.  
   
**Requisitos de várias florestas**  
  
-   O domínio ao qual os servidores de AD FS são ingressados deve confiar em todos os domínios ou florestas que contenham usuários que se autenticam no serviço AD FS.  

-   A floresta, da qual a conta de serviço do AD FS é membro, deve confiar em todas as florestas de logon do usuário. 
  
-   A conta de serviço de AD FS deve ter permissões para ler atributos de usuário em cada domínio que contenha usuários que se autenticam no serviço AD FS.  
  
## <a name="BKMK_5"></a>Requisitos do banco de dados de configuração  
Esta seção descreve os requisitos e as restrições para AD FS farms que usam, respectivamente, o banco de dados interno do Windows (WID) ou SQL Server como o banco de dados:  
  
**WID**  
  
-   Não há suporte para o perfil de resolução de artefatos do SAML 2,0 em um farm WID.    

-   Não há suporte para detecção de reprodução de token em um farm WID. (Essa funcionalidade só é usada em cenários em que AD FS está agindo como o provedor de Federação e consumindo tokens de segurança de provedores de declarações externos.)  
  
A tabela a seguir fornece um resumo de quantos servidores AD FS têm suporte em um WID em vez de um farm de SQL Server.    
  
  
|| 1-100 relações de confiança de RP (terceira parte confiável) configuradas no AD FS | Mais de 100 RP confianças configuradas  |
| --- |--- | --- |
|1-30 servidores AD FS|WID com suporte|Sem suporte usando WID-SQL Server necessário |
|Mais de 30 servidores AD FS|Sem suporte usando WID-SQL Server necessário|Sem suporte usando WID-SQL Server necessário  
  
**SQL Server**  
  
- Para AD FS no Windows Server 2016, há suporte para SQL Server 2008 e versões posteriores.

- A resolução de artefato SAML e a detecção de token de reprodução têm suporte em um farm de SQL Server.  
  
## <a name="BKMK_6"></a>Requisitos do navegador  
Quando AD FS autenticação é executada por meio de um navegador ou controle de navegador, seu navegador deve atender aos seguintes requisitos:  
  
- O JavaScript deve estar habilitado  
  
- Para o logon único, o navegador do cliente deve ser configurado para permitir cookies  
  
- Indicação de nome de servidor \(SNI\) deve ter suporte  
  
- Para certificado de usuário & autenticação de certificado de dispositivo, o navegador deve dar suporte à autenticação de certificado de cliente SSL  

- Para o logon contínuo usando a autenticação integrada do Windows, o nome do serviço de Federação (\/como https:\/FS.contoso.com) deve ser configurado em zona da intranet local ou em zona de sites confiáveis.
  ## <a name="BKMK_7"></a>Requisitos de rede  
 
**Requisitos de firewall**  
  
O firewall localizado entre o proxy de aplicativo Web e o farm de servidores de Federação e o firewall entre os clientes e o proxy de aplicativo Web deve ter a porta TCP 443 habilitada.  
  
Além disso, se a autenticação de certificado \(de usuário do cliente clientTLS de\) autenticações usando certificados de usuário X509 for necessária e o ponto de extremidade certauth na porta 443 não estiver habilitado, AD FS 2016 exigirá que a porta TCP 49443 seja habilitada entrada no firewall entre os clientes e o proxy de aplicativo Web. Isso não é necessário no firewall entre o proxy de aplicativo Web e os servidores\)de Federação. 

Para obter informações adicionais sobre os requisitos de porta híbrida [, consulte portas de identidade híbrida e protocolos](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports). 

Para obter informações adicionais, consulte [práticas recomendadas para proteger serviços de Federação do Active Directory (AD FS)](../deployment/Best-Practices-Securing-AD-FS.md)
  
**Requisitos de DNS**  
  
-   Para acesso à intranet, todos os clientes que acessam AD FS serviço \(na\) intranet da rede corporativa interna devem ser capazes de resolver o nome do serviço de AD FS para o balanceador de carga para os servidores AD FS ou o servidor AD FS.  
  
-   Para acesso à extranet, todos os clientes que acessam AD FS serviço \(de\/fora\) da extranet da rede corporativa devem ser capazes de resolver o nome do serviço de AD FS para o balanceador de carga para os servidores de proxy de aplicativo Web ou o servidor proxy de aplicativo Web.  
  
-   Cada servidor proxy de aplicativo Web na DMZ deve ser capaz de resolver AD FS nome do serviço para o balanceador de carga para os servidores AD FS ou o servidor AD FS. Isso pode ser feito usando um servidor DNS alternativo na rede DMZ ou alterando a resolução do servidor local usando o arquivo HOSTs.  
  
-   Para a autenticação integrada do Windows, você deve usar um registro \(DNS a\) não CNAME para o nome do serviço de Federação.  

-   Para autenticação de certificado de usuário na porta 443, "certauth. \<nome\>do serviço de Federação "deve ser configurado no DNS para resolver para o servidor de Federação ou proxy de aplicativo Web.

-   Para o registro de dispositivo ou para autenticação moderna para recursos locais usando clientes anteriores ao Windows 10, "enterpriseregistration. \<sufixo\>UPN ", para cada sufixo UPN em uso em sua organização, deve ser configurado para resolver para o servidor de Federação ou proxy de aplicativo Web.

**Requisitos de Load Balancer**
- O balanceador de carga não deve terminar o SSL. AD FS dá suporte a vários casos de uso com autenticação de certificado que será interrompida ao terminar o SSL. Não há suporte para a finalização do SSL no balanceador de carga para nenhum caso de uso. 
- É recomendável usar um balanceador de carga que dê suporte a SNI. Caso contrário, usar a associação de fallback 0.0.0.0 em seu AD FS/servidor proxy de aplicativo Web deve fornecer uma solução alternativa.
- É recomendável usar os pontos de extremidade de investigação de integridade HTTP (não HTTPS) para executar verificações de integridade do balanceador de carga para o tráfego de roteamento. Isso evita quaisquer problemas relacionados ao SNI. A resposta a esses pontos de extremidade de investigação é um HTTP 200 OK e é servida localmente sem dependência de serviços de back-end. A investigação HTTP pode ser acessada via HTTP usando o caminho '/ADFS/Probe '
    - &lt;nome&gt;do proxy de aplicativo Web http:///ADFS/Probe
    - http://&lt;nome&gt;do servidor ADFS/ADFS/Probe
    - &lt;endereço&gt;IP do proxy de aplicativo Web do http:///ADFS/Probe
    - http://&lt;endereço&gt;IP do ADFS/ADFS/Probe
- Não é recomendável usar o DNS round robin como uma maneira de balancear a carga. O uso desse tipo de balanceamento de carga não fornece uma maneira automatizada de remover um nó do balanceador de carga usando investigações de integridade. 
- Não é recomendável usar a afinidade de sessão baseada em IP ou as sessões adesivas para o tráfego de autenticação para AD FS no balanceador de carga. Isso pode causar uma sobrecarga de determinados nós ao usar o protocolo de autenticação herdado para que os clientes de email se conectem ao Office 365 mail Services (Exchange Online). 

## <a name="BKMK_13"></a>Requisitos de permissões  
O administrador que executa a instalação e a configuração inicial de AD FS deve ter permissões de administrador local no servidor AD FS.  Se o administrador local não tiver permissões para criar objetos no Active Directory, eles deverão primeiro ter um administrador de domínio para criar os objetos do AD necessários e, em seguida, configurar o farm de AD FS usando o parâmetro AdminConfiguration.  
  
  

