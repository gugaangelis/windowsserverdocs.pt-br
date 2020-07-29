---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: Requisitos do AD FS 2016
description: Requisitos para instalar os Serviços de Federação do Active Directory (AD FS).
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8f1af40f54536ca380db7fe810506c937bb2d478
ms.sourcegitcommit: f305bc5f1c5a44dac62f4288450af19f351f9576
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87118609"
---
# <a name="ad-fs-requirements"></a>Requisitos do AD FS



Os requisitos para implantar o AD FS são:  
  
-   [Requisitos de certificado](ad-fs-requirements.md#BKMK_1)  
  
-   [Requisitos de hardware](ad-fs-requirements.md#BKMK_2)  
  
-   [Requisitos de proxy](ad-fs-requirements.md#BKMK_3)  
  
-   [Requisitos do AD DS](ad-fs-requirements.md#BKMK_4)  
  
-   [Requisitos de banco de dados de configuração](ad-fs-requirements.md#BKMK_5)  
  
-   [Requisitos de navegador](ad-fs-requirements.md#BKMK_6)  

-   [Requisitos de rede](ad-fs-requirements.md#BKMK_7)  
  
-   [Requisitos de permissões](ad-fs-requirements.md#BKMK_13)  
  
## <a name="certificate-requirements"></a><a name="BKMK_1"></a>Requisitos de certificado  
  
### <a name="ssl-certificates"></a>Certificados SSL

Cada servidor Proxy de Aplicativo Web e AD FS tem um certificado SSL para atender a solicitações HTTPS para o Serviço de Federação.  O Proxy de Aplicativo Web pode ter certificados SSL adicionais para atender a solicitações para aplicativos publicados.

**Recomendação:** use o mesmo certificado SSL para todos os servidores de federação do AD FS e proxies de Aplicativo Web. 

**Requisitos:**

certificados SSL em servidores de federação devem atender aos requisitos a seguir
- O certificado é publicamente confiável (para implantações de produção)
- O certificado contém o valor de EKU (Uso Avançado de Chave) de Autenticação de Servidor
- O certificado contém o nome do Serviço de Federação, como “fs.contoso.com” na Entidade ou no SAN (Nome Alternativo da Entidade)
- Para autenticação do certificado do usuário na porta 443, o certificado contém "certauth.\<federation service name\>", como "certauth.fs.contoso.com" no SAN
- Para o registro do dispositivo ou para a autenticação moderna em recursos locais usando clientes anteriores ao Windows 10, o SAN deve conter "enterpriseregistration.\<upn suffix\>" para cada sufixo UPN em uso na sua organização.

os certificados SSL no Proxy de Aplicativo Web devem atender aos requisitos a seguir
- Se o proxy for usado para usar um proxy de solicitações do AD FS que usam a Autenticação Integrada do Windows, o certificado SSL do proxy precisará ser igual (usar a mesma chave) o certificado SSL do servidor de federação
- Se a propriedade “ExtendedProtectionTokenCheck” do AD FS estiver habilitada (a configuração padrão no AD FS), o certificado SSL de proxy precisará ser o mesmo (usar a mesma chave) que o certificado SSL do servidor de federação
- Caso contrário, os requisitos do certificado SSL de proxy são os mesmos que os do certificado SSL do servidor de federação

### <a name="service-communication-certificate"></a>Certificado de Comunicação de Serviço
Esse certificado não é necessário para a maioria dos cenários do AD FS incluindo o Azure AD e o Office 365. Por padrão, o AD FS configura o certificado SSL fornecido na configuração inicial como o certificado de comunicação de serviço.

**Recomendação:**
- Use o mesmo certificado que você usa para SSL.  

### <a name="token-signing-certificate"></a>Certificado de Autenticação de Tokens
Esse certificado é usado para assinar tokens emitidos para terceiras partes confiáveis; assim, os aplicativos de terceira parte confiável devem reconhecer o certificado e a chave associada dele como conhecidos e confiáveis. Quando o certificado de autenticação de tokens é alterado, como quando ele expira e você configura um novo certificado, todas as terceiras partes confiáveis devem ser atualizadas.

**Recomendação:** use os certificados de autenticação de tokens autoassinados padrão e gerados internamente do AD FS.  

**Requisitos:**
- se a organização exigir que os certificados da PKI corporativa sejam usados para autenticação de token, isso poderá ser feito usando o parâmetro SigningCertificateThumbprint do cmdlet Install-AdfsFarm.
- Se você usar os certificados padrão gerados internamente ou os certificados registrados externamente, quando o certificado de autenticação de tokens for alterado, você deverá verificar se todas as terceiras partes confiáveis estão atualizadas com as novas informações de certificado.  Caso contrário, os logons em terceiras partes confiáveis não atualizadas falharão.

### <a name="token-encryptingdecrypting-certificate"></a>Certificado de criptografia/descriptografia de tokens
Esse certificado é usado por provedores de declarações que criptografam tokens emitidos para o AD FS.

**Recomendação:** use os certificados de descriptografia de tokens autoassinados padrão e gerados internamente do AD FS.  

**Requisitos:**
- se a organização exigir que os certificados da PKI corporativa sejam usados para autenticação de token, isso poderá ser feito usando o parâmetro DecryptingCertificateThumbprint do cmdlet Install-AdfsFarm.
- Se você usar os certificados padrão gerados internamente ou os certificados registrados externamente, quando o certificado de descriptografia de tokens for alterado, você deverá verificar se todos os provedores de declarações estão atualizados com as novas informações de certificado.  Caso contrário, os logons que usam provedores de declarações não atualizados falharão.
  
> [!CAUTION]  
> Os certificados usados para a autenticação e criptografia\/descriptografia de tokens são fundamentais para a estabilidade do Serviço de Federação. Os clientes que gerenciam os próprios certificados de autenticação e criptografia\/descriptografia de tokens devem verificar se o backup desses certificados foi feito e se eles estão disponíveis de maneira independentemente durante um evento de recuperação.  

### <a name="user-certificates"></a>Certificados do usuário
- Ao usar a autenticação de certificado de usuário x509 com o AD FS, todos os certificados de usuário devem se ligar a uma autoridade de certificado raiz que tem a confiança de servidores AD FS e do Proxy de Aplicativo Web.

## <a name="hardware-requirements"></a><a name="BKMK_2"></a>Requisitos de hardware  
Os requisitos de hardware (físicos ou virtuais) do AD FS e do Proxy de Aplicativo Web são controlados na CPU. Portanto, você deve dimensionar seu farm para capacidade de processamento.  
- Use a [planilha do Planejamento da Capacidade do AD FS 2016](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx) para determinar o número de servidores AD FS e Proxy de Aplicativo Web de que você precisará.

Os requisitos de memória e disco do AD FS são razoavelmente estáticos, confira a tabela abaixo:


|**Requisito de hardware**|**Requisito mínimo**|**Requisito recomendado**|
|----- | ----- |-----|
|RAM|2 GB|4 GB |
|Espaço em disco|32 GB|100 GB |

**Requisitos de hardware do SQL Server**

Se você estiver usando o SQL Server para seu banco de dados de configuração do AD FS, dimensione-o de acordo com as recomendações mais básicas do SQL Server.  O tamanho do banco de dados do AD FS é muito pequeno e o AD FS não coloca uma carga de processamento significativa na instância de banco de dados.  O AD FS, no entanto, se conecta ao banco de dados várias vezes durante uma autenticação; portanto, a conexão de rede deve ser robusta.  Não há suporte para o SQL Azure no banco de dados de configuração do AD FS.
  
## <a name="proxy-requirements"></a><a name="BKMK_3"></a>Requisitos de proxy  
  
-   Para acesso à extranet, você deve implantar a parte \- do serviço de função do Proxy de Aplicativo Web da função de servidor do Acesso Remoto. 

-   Proxies de terceiros devem dar suporte ao [protocolo MS-ADFSPIP](/openspecs/windows_protocols/ms-adfspip/76deccb1-1429-4c80-8349-d38e61da5cbb) para ter suporte como um proxy do AD FS.  Para obter uma lista de fornecedores de terceiros, confira as [Perguntas frequentes](AD-FS-FAQ.md).

-   O AD FS 2016 requer servidores de Proxy de Aplicativo Web no Windows Server 2016.  Um proxy de nível inferior não pode ser configurado para um farm AD FS 2016 em execução no nível de comportamento do farm 2016.
  
-   Um servidor de federação e o serviço de função do Proxy de Aplicativo Web não podem ser instalados no mesmo computador.  
  
## <a name="ad-ds-requirements"></a><a name="BKMK_4"></a>Requisitos do AD DS  
**Requisitos de controlador de domínio**  
  
- O AD FS requer controladores de domínio que executam o Windows Server 2008 ou posterior.

- Pelo menos um controlador de domínio do Windows Server 2016 é necessário para o Microsoft Passport for Work.
  
> [!NOTE]  
> Todo o suporte para ambientes com controladores de domínio do Windows Server 2003 foi encerrado. Acesse [esta página](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) para obter informações adicionais sobre o Ciclo de Vida de Suporte da Microsoft.  
  
**Requisitos de nível\-funcional do domínio**  
  
 - Todos os domínios de conta de usuário e o domínio no qual os servidores AD FS são ingressados devem estar operando no nível funcional do domínio do Windows Server 2003 ou posterior.  
  
 - Um nível funcional de domínio do Windows Server 2008 ou superior é exigido para que a autenticação de certificados de clientes caso o certificado esteja associado explicitamente à conta de usuário do AD DS.  
  
**Requisitos de esquema**  
  
-   Novas instalações do AD FS 2016 exigem o esquema do Active Directory 2016 (versão mínima 85).

-   Aumentar o FBL (nível de comportamento do farm) do AD FS para o nível 2016 requer o esquema do Active Directory 2016 (versão mínima 85).  

  
**Requisitos de conta de serviço**  
  
-   Qualquer conta de domínio padrão pode ser usada como uma conta de serviço para o AD FS. Também há suporte para contas do Serviço Gerenciado de Grupo. As permissões necessárias em runtime serão adicionadas automaticamente quando você configurar o AD FS.

-   A Atribuição de Direitos de Usuário necessária para a conta de serviço do AD é ‘Fazer logon como um serviço’

-   As Atribuições de Direitos de Usuário necessárias para 'NT Service\adfssrv' e 'NT Service\drs' são 'Gerar Auditorias de Segurança' e 'Fazer logon como um serviço'.

-   As contas do Serviço Gerenciado de Grupo exigem pelo menos um controlador de domínio que execute o Windows Server 2012 ou posterior.  O GMSA deve residir no contêiner padrão 'CN=Managed Service Accounts'.  

-   Para a autenticação do Kerberos, o nome da entidade de serviço '`HOST/<adfs\_service\_name>`' deve ser registrado na conta de serviço do AD FS. Por padrão, o AD FS configurá isso ao criar um farm do AD FS.  Se isso falhar, como no caso de uma colisão ou permissões insuficientes, você verá um aviso e deverá adicioná-lo manualmente.  
   
**Requisitos de Domínio**  
  
-   Todos os servidores AD FS devem ser ingressados em um domínio do AD DS.  
  
-   Todos os servidores AD FS em um farm devem ser implantados no mesmo domínio.  
   
**Requisitos de várias florestas**  
  
-   O domínio no qual os servidores AD FS são ingressados deve confiar em todos os domínios ou florestas que contenham usuários que se autenticam no serviço do AD FS.  

-   A floresta, da qual a conta de serviço do AD FS é membro, deve confiar em todas as florestas de logon do usuário. 
  
-   A conta de serviço do AD FS deve ter permissões para ler atributos de usuário em cada domínio que contenha usuários que se autenticam no serviço do AD FS.  
  
## <a name="configuration-database-requirements"></a><a name="BKMK_5"></a>Requisitos de banco de dados de configuração  
Esta seção descreve os requisitos e as restrições para farms do AD FS que usam respectivamente o WID (Banco de Dados Interno do Windows) ou o SQL Server como o banco de dados:  
  
**WID**  
  
-   Não há suporte para o perfil de resolução do artefato do SAML 2.0 em um farm do WID.    

-   Não há suporte para a detecção de reprodução de tokens em um farm do WID. (Essa funcionalidade é usada apenas em cenários em que o AD FS está agindo como o provedor de federação e consumindo tokens de segurança de provedores de declarações externos).  
  
A tabela a seguir fornece um resumo de quantos servidores AD FS têm suporte em um WID versus um farm do SQL Server.    
  
  
|| 1 a 100 relações de confiança de RP (terceira parte confiável) configuradas no AD FS | Mais de 100 relações de confiança de RP configuradas  |
| --- |--- | --- |
|1 a 30 servidores AD FS|Suporte ao WID|Sem suporte usando o WID – é necessário ter o SQL Server |
|Mais de 30 servidores AD FS|Sem suporte usando o WID – é necessário ter o SQL Server|Sem suporte usando o WID – é necessário ter o SQL Server  
  
**SQL Server**  
  
- No AD FS no Windows Server 2016, há suporte para o SQL Server 2008 e versões posteriores.

- Há suporte para a resolução de artefato SAML e a detecção de reprodução de tokens em um farm do SQL Server.  
  
## <a name="browser-requirements"></a><a name="BKMK_6"></a>Requisitos de navegador  
Quando a autenticação do AD FS é executada por meio de um navegador ou controle de navegador, seu navegador deve atender aos seguintes requisitos:  
  
- O JavaScript deve estar habilitado  
  
- Para logon único, o navegador do cliente deve estar configurado para permitir cookies  
  
- Deve haver suporte para a SNI \(Indicação de Nome de Servidor\)  
  
- Para a autenticação de certificado do usuário e do dispositivo, o navegador deve dar suporte à autenticação de certificado de cliente SSL  

- Para o logon contínuo usando a Autenticação Integrada do Windows, o nome do Serviço de Federação (como https:\/\/fs.contoso.com) deve ser configurado na zona da intranet local ou na zona de sites confiáveis.
  ## <a name="network-requirements"></a><a name="BKMK_7"></a>Requisitos de rede  
 
**Requisitos de firewall**  
  
O firewall localizado entre o Proxy de Aplicativo Web e o farm de servidores de federação e o firewall entre os clientes e o Proxy de Aplicativo Web deve ter a porta TCP 443 habilitada para entrada.  
  
Além disso, se a autenticação de certificado de usuário cliente \(autenticação clientTLS que usa certificados de usuário X509\) for necessária e o ponto de extremidade certauth na porta 443 não estiver habilitado, o AD FS 2016 exigirá que a porta TCP 49443 seja habilitada para entrada no firewall entre os clientes e o Proxy de Aplicativo Web. Isso não é necessário no firewall entre o Proxy de aplicativo Web e os servidores de federação. 

Para obter mais informações sobre os requisitos de porta híbrida, confira [Portas de Identidade Híbrida e Protocolos](/azure/active-directory/connect/active-directory-aadconnect-ports). 

Para obter informações adicionais, confira [Melhores práticas para proteger os Serviços de Federação do Active Directory (AD FS)](../deployment/Best-Practices-Securing-AD-FS.md)
  
**Requisitos de DNS**  
  
-   Para acesso à intranet, todos os clientes que acessam o serviço do AD FS na rede corporativa interna \(intranet\) devem ser capazes de resolver o nome do serviço do AD FS para o balanceador de carga para os servidores AD FS ou o servidor AD FS.  
  
-   Para acesso à extranet, todos os clientes que acessam o serviço do AD FS fora da rede corporativa \(extranet\/Internet\) devem ser capazes de resolver o nome do serviço do AD FS para o balanceador de carga para os servidores Proxy de Aplicativo Web ou para o servidor Proxy de Aplicativo Web.  
  
-   Cada servidor Proxy de Aplicativo Web no DMZ deve poder resolver o nome do serviço do AD FS para o balanceador de carga dos servidores AD FS ou do servidor AD FS. Isso pode ser feito usando um servidor DNS alternativo na rede DMZ ou alterando a resolução do servidor local que usa o arquivo HOSTS.  
  
-   Para a autenticação integrada do Windows, você deve usar um registro DNS A \(não CNAME\) para o nome do Serviço de Federação.  

-   Para autenticação do certificado do usuário na porta 443, "certauth.\<federation service name\>" deve ser configurado no DNS para resolver para o servidor de federação ou o proxy do aplicativo Web.

-   Para o registro do dispositivo ou para a autenticação moderna em recursos locais usando clientes anteriores ao Windows 10, "enterpriseregistration.\<upn suffix\>", para cada sufixo UPN em uso na sua organização, deve estar configurado para resolver para o servidor de federação ou o proxy de aplicativo Web.

**Requisitos do Load Balancer**
- O balanceador de carga NÃO DEVE encerrar o SSL. O AD FS dá suporte a vários casos de uso com autenticação de certificado que será interrompida ao encerrar o SSL. Não há suporte para o encerramento do SSL no balanceador de carga em nenhum caso de uso. 
- É recomendável usar um balanceador de carga que dê suporte à SNI. Caso ele não dê, usar a associação de fallback 0.0.0.0 em seu servidor Proxy de Aplicativo Web/AD FS deverá fornecer uma solução alternativa.
- É recomendável usar os pontos de extremidade de investigação de integridade HTTP (não HTTPS) para executar verificações de integridade do balanceador de carga quanto ao tráfego de roteiros. Isso evita problemas relacionados à SNI. A resposta a esses pontos de extremidade de investigação é um HTTP 200 OK e é atendida localmente sem dependência de serviços de back-end. A investigação HTTP pode ser acessada por HTTP usando o caminho '/adfs/probe'
    - http://&lt;nome do Proxy de Aplicativo Web&gt;/adfs/probe
    - http://&lt;Nome do servidor do ADFS&gt;/adfs/probe
    - http://&lt;endereço IP do Proxy de Aplicativo Web&gt;/adfs/probe
    - http://&lt;Endereço IP do ADFS&gt;/adfs/probe
- NÃO é recomendável usar o round robin de DNS como uma maneira de balancear carga. O uso desse tipo de balanceamento de carga não fornece uma maneira automatizada de remover um nó do balanceador de carga usando investigações de integridade. 
- NÃO é recomendável usar a afinidade de sessão baseada em IP ou as sessões adesivas para o tráfego de autenticação para o AD FS no balanceador de carga. Isso pode causar uma sobrecarga de determinados nós ao usar o protocolo de autenticação herdado para clientes de email que se conectam os serviços de email do Office 365 (Exchange Online). 

## <a name="permissions-requirements"></a><a name="BKMK_13"></a>Requisitos de permissões  
O administrador que executa a instalação e a configuração inicial do AD FS deve ter permissões de administrador local no servidor AD FS.  Se o administrador local não tiver permissões para criar objetos no Active Directory, primeiro ele deverá fazer um administrador de domínio criar os objetos do AD necessários e configurar o farm do AD FS usando o parâmetro AdminConfiguration.  
  
  
