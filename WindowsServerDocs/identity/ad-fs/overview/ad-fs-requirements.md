---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: Requisitos do AD FS 2016
description: Requisitos para instalar os serviços de Federação do Active Directory.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9a5155dd31b9f8f86fac532aa33cd9f7389d8a48
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815117"
---
# <a name="ad-fs-requirements"></a>Requisitos do AD FS

>Aplica-se a: Windows Server 2016

A seguir está os requisitos para implantar o AD FS:  
  
-   [Requisitos de certificado](ad-fs-requirements.md#BKMK_1)  
  
-   [Requisitos de hardware](ad-fs-requirements.md#BKMK_2)  
  
-   [Requisitos de proxy](ad-fs-requirements.md#BKMK_3)  
  
-   [Requisitos do AD DS](ad-fs-requirements.md#BKMK_4)  
  
-   [Requisitos de banco de dados de configuração](ad-fs-requirements.md#BKMK_5)  
  
-   [Requisitos de navegador](ad-fs-requirements.md#BKMK_6)  

-   [Requisitos de rede](ad-fs-requirements.md#BKMK_7)  
  
-   [Requisitos de permissões](ad-fs-requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Requisitos de certificado  
  
### <a name="ssl-certificates"></a>Certificados SSL

Cada servidor do AD FS e Proxy de aplicativo Web tem um certificado SSL para atender a solicitações HTTPS para o serviço de Federação.  O Proxy de aplicativo Web pode ter um certificado SSL adicional para solicitações de serviço para aplicativos publicados.

**Recomendação:** Use o mesmo certificado SSL para todos os servidores de Federação do AD FS e proxies de aplicativo Web. 

**Requisitos:**

Certificados SSL em servidores de Federação devem atender aos seguintes requisitos
- Certificado é confiável publicamente (para implantações de produção)
- Certificado contém o valor de servidor autenticação Enhanced Key Usage (EKU)
- Certificado contém o nome do serviço de federação, por exemplo, "fs.contoso.com" no assunto ou no nome alternativo da entidade (SAN)
- Para autenticação de certificado de usuário na porta 443, o certificado contém "certauth. \<nome do serviço de Federação\>", como"certauth.fs.contoso.com"na SAN
- Para o registro de dispositivos ou para autenticação moderna para recursos locais usando clientes de versões anteriores ao Windows 10, a SAN deve conter "enterpriseregistration. \<sufixo upn\>"para cada sufixo UPN em uso na sua organização.

Certificados SSL em Proxy de aplicativo Web devem atender aos seguintes requisitos
- Se o proxy é usado para solicitações de proxy do AD FS que usam autenticação integrada do Windows, o certificado SSL de proxy devem ser o mesmo (use a mesma chave) que o certificado SSL do servidor de Federação
- Se a propriedade do AD FS "ExtendedProtectionTokenCheck" é habilitado (a configuração padrão no AD FS), o certificado SSL de proxy deve ser o mesmo (use a mesma chave) que o certificado SSL do servidor de Federação
- Caso contrário, os requisitos para o certificado SSL de proxy são os mesmos para o certificado SSL do servidor de Federação

### <a name="service-communication-certificate"></a>Certificado de comunicação de serviço
Esse certificado não é necessário para a maioria dos cenários de AD FS, incluindo o Azure AD e o Office 365. Por padrão, o AD FS configura o certificado SSL fornecido após a configuração inicial, como o certificado de comunicação de serviço.

**Recomendação:**
- Use o mesmo certificado que o use para SSL.  

### <a name="token-signing-certificate"></a>Certificado de assinatura de token
Esse certificado é usado para assinar tokens emitidos para partes confiáveis, portanto, aplicativos de terceira parte confiável devem reconhecer o certificado e ela está associada a chave como conhecido e confiável. Quando as assinatura certificado alterações em tokens, como quando ele expirar e você configurem um novo certificado, todas as terceiras partes confiáveis devem ser atualizadas.

**Recomendação:** Use o padrão do AD FS, os certificados de assinatura de token gerado internamente, autoassinado.  

**Requisitos:**
- Se sua organização exigir que certificados da PKI corporativa ser usados para autenticação de tokens, isso pode ser feito usando o parâmetro SigningCertificateThumbprint do cmdlet Install-AdfsFarm.
- Se você usar os certificados de padrão gerada internamente ou externamente inscritos certificados, quando o certificado de autenticação de token é alterado, você devem garantir que todas as terceiras partes são atualizadas com as novas informações de certificado.  Caso contrário, falhará logons para qualquer terceiras partes confiáveis não atualizadas.

### <a name="token-encryptingdecrypting-certificate"></a>Certificado de criptografia/descriptografia de token
Esse certificado é usado pelos provedores de declarações que criptografar tokens emitidos para o AD FS.

**Recomendação:** Use o padrão do AD FS, os certificados de descriptografia de token gerado internamente, autoassinado.  

**Requisitos:**
- Se sua organização exigir que certificados da PKI corporativa ser usados para autenticação de tokens, isso pode ser feito usando o parâmetro DecryptingCertificateThumbprint do cmdlet Install-AdfsFarm.
- Se você usar os certificados de padrão gerada internamente ou externamente inscritos certificados, quando o certificado de descriptografia de token é alterado, você devem garantir que todos os provedores de declarações são atualizados com as novas informações de certificado.  Caso contrário, logons usando qualquer não atualizadas de provedores de declarações falhará.
  
> [!CAUTION]  
> Certificados que são usados para o token\-assinatura e o token\-descriptografar\/criptografar são essenciais para a estabilidade do serviço de Federação. Os clientes que gerenciam sua próprias token\-assinatura & token\-descriptografar\/certificados de criptografia deve garantir que esses certificados são obtidos em backup e estão disponíveis independentemente durante um evento de recuperação.  

### <a name="user-certificates"></a>Certificados de usuário
- Ao usar a autenticação de certificado de usuário com o AD FS, todos os certificados de usuário deve se liga a uma autoridade de certificação raiz confiável para os servidores do AD FS e Proxy de aplicativo Web do x509.

## <a name="BKMK_2"></a>Requisitos de hardware  
Requisitos de hardware do AD FS e Proxy de aplicativo Web (físicos ou virtuais) estão ligados a CPU, portanto, você deve dimensionar seu farm para a capacidade de processamento.  
- Use o [planilha de planejamento de capacidade do AD FS 2016](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx) para determinar o número de servidores do AD FS e Proxy de aplicativo Web, será necessário.

Os requisitos de memória e disco para o AD FS são razoavelmente estáticos, consulte a tabela a seguir:


|**Requisito de hardware**|**Requisito mínimo**|**Requisito recomendado**|
|----- | ----- |-----|
|RAM|2 GB|4 GB |
|Espaço em disco|32 GB|100 GB |

**Requisitos de Hardware do SQL Server**

Se você estiver usando o SQL Server do banco de dados de configuração do AD FS, dimensione o SQL Server de acordo com as recomendações mais básicas do SQL Server.  O tamanho do banco de dados do AD FS é muito pequeno, e o AD FS não coloca uma carga de processamento significativo na instância do banco de dados.  AD FS, no entanto, conecte-se ao banco de dados várias vezes durante uma autenticação, portanto, a conexão de rede deve ser robusto.  Infelizmente, o SQL Azure não há suporte para o banco de dados de configuração do AD FS.
  
## <a name="BKMK_3"></a>Requisitos de proxy  
  
-   Para obter acesso à extranet, você deve implantar o serviço de função Proxy de aplicativo Web \- faz parte da função de servidor de acesso remoto. 

-   Proxies de terceiros devem oferecer suporte a [protocolo MS-ADFSPIP](https://msdn.microsoft.com/en-us/library/dn392811.aspx) a ser aceito como um proxy do AD FS.  Para obter uma lista de 3ª parte fornecedores consulte os [perguntas frequentes sobre](AD-FS-FAQ.md#what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip).

-   AD FS 2016 requer servidores de Proxy de aplicativo Web no Windows Server 2016.  Um proxy de nível inferior não pode ser configurado para um farm de AD FS 2016 em execução no nível de comportamento de farm 2016.
  
-   Um servidor de Federação e o serviço de função Proxy de aplicativo Web não podem ser instalados no mesmo computador.  
  
## <a name="BKMK_4"></a>Requisitos do AD DS  
**Requisitos do controlador de domínio**  
  
- O AD FS requer que os controladores de domínio executando o Windows Server 2008 ou posterior.

- Pelo menos um controlador de domínio do Windows Server 2016 é necessário para o Microsoft Passport for Work.
  
> [!NOTE]  
> Todo o suporte para ambientes com controladores de domínio do Windows Server 2003 foi encerrada. Visite [nesta página](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) para obter mais informações sobre o ciclo de vida do suporte Microsoft.  
  
**Funcional de domínio\-requisitos de nível**  
  
 - Todos os domínios de conta de usuário e o domínio ao qual os servidores do AD FS são Unidos deverão estar operando com o nível funcional de domínio do Windows Server 2003 ou superior.  
  
 - Um domínio do Windows Server 2008 funcional nível ou superior é necessário para autenticação de certificado do cliente se o certificado explicitamente é mapeado para uma conta de usuário no AD DS.  
  
**Requisitos do esquema**  
  
-   Novas instalações do AD FS 2016 exigem o esquema do Active Directory de 2016 (versão mínima 85).

-   Elevar o nível de comportamento de farm do AD FS (FBL) para o nível de 2016 requer o esquema do Active Directory de 2016 (versão mínima 85).  

  
**Requisitos de conta de serviço**  
  
-   Qualquer conta de domínio padrão pode ser usada como uma conta de serviço do AD FS. Também há suporte para contas de serviço gerenciado de grupo. As permissões necessárias em tempo de execução serão adicionadas automaticamente quando você configura o AD FS.

-   A atribuição de direitos de usuário necessários para a conta de serviço do AD é 'Fazer logon como um serviço'

-   As atribuições de direitos de usuário necessários para o 'NT Service\adfssrv' e 'NT Service\drs' são 'Gerar auditorias de segurança' e 'Fazer logon como um serviço'.

-   Contas de serviço gerenciado de grupo requerem pelo menos um controlador de domínio executando o Windows Server 2012 ou posterior.  A GMSA deverão residir sob o padrão ' CN = contêiner de contas de serviço gerenciado.  

-   Para a autenticação Kerberos, o nome da entidade de serviço '`HOST/<adfs\_service\_name>`' deve ser registrado na conta de serviço do AD FS. Por padrão, o AD FS irá configurar isso ao criar um novo farm do AD FS.  Se isso falhar, como no caso de uma colisão ou permissões insuficientes, você verá um aviso e você deve adicioná-lo manualmente.  
   
**Requisitos de domínio**  
  
-   Todos os servidores do AD FS devem ser unido a um domínio AD DS.  
  
-   Todos os servidores do AD FS em um farm devem ser implantados no mesmo domínio.  
   
**Requisitos de várias florestas**  
  
-   O domínio ao qual os servidores do AD FS são Unidos deve confiar em cada domínio ou floresta que contém usuários que se autenticam para o serviço AD FS.  

-   A floresta, o que a conta de serviço do AD FS é membro, deve confiar em todas as florestas de logon do usuário. 
  
-   A conta de serviço do AD FS deve ter permissões para ler os atributos de usuário em cada domínio que contém a autenticação dos usuários para o serviço AD FS.  
  
## <a name="BKMK_5"></a>Requisitos de banco de dados de configuração  
Esta seção descreve os requisitos e restrições para farms do AD FS que usam respectivamente o banco de dados interno do Windows (WID) ou o SQL Server como banco de dados:  
  
**WID**  
  
-   Não há suporte para o perfil de resolução de artefato de SAML 2.0 em um farm WID.    

-   Detecção de reprodução de token não é suporte para um farm de WID. (Essa funcionalidade só é usada apenas em cenários em que o AD FS está atuando como o provedor de Federação e consumindo tokens de segurança de provedores de declarações externas.)  
  
A tabela a seguir fornece um resumo de quantos servidores do AD FS são um farm do SQL Server com suporte no vs WID.    
  
  
|| 1 - 100, terceira (RP) configurado no AD FS | Relações de confiança RP mais de 100 configuradas  |
| --- |--- | --- |
|Servidores de 1-30 AD FS|Suportada de WID|Não há suportada usando o WID - necessários do SQL Server |
|Servidores de mais de 30 AD FS|Não há suportada usando o WID - necessários do SQL Server|Não há suportada usando o WID - necessários do SQL Server  
  
**SQL Server**  
  
- Para AD FS no Windows Server 2016, SQL Server 2008 e versões posteriores têm suporte.

- Resolução do artefato SAML e detecção de reprodução de token têm suporte em um farm do SQL Server.  
  
## <a name="BKMK_6"></a>Requisitos de navegador  
Quando a autenticação do AD FS é realizada por meio de um navegador ou um controle de navegador, seu navegador deve estar em conformidade aos seguintes requisitos:  
  
-   JavaScript deve estar habilitado  
  
-   Para logon único, o navegador do cliente deve ser configurado para permitir cookies  
  
-   Indicação de nome de servidor \(SNI\) devem ter suporte  
  
-   Certificado & dispositivo certificado para autenticação de usuário, o navegador deve oferecer suporte a autenticação de certificado de cliente SSL  

-   Para o logon usando a autenticação integrada do Windows, o nome do serviço de Federação contínuo (como https:\/\/fs.contoso.com) deve ser configurado na zona de intranet local ou a zona de sites confiáveis.
## <a name="BKMK_7"></a>Requisitos de rede  
 
**Requisitos de firewall**  
  
Firewall localizado entre o Proxy de aplicativo Web e o farm de servidores de Federação e o firewall entre os clientes e o Proxy de aplicativo Web deve ter a porta TCP 443 habilitado entrada.  
  
Além disso, se a autenticação de certificado de usuário do cliente \(autenticação do clientTLS usando X509 certificados de usuário\) é necessária e o ponto de extremidade certauth na porta 443 não está habilitado, o AD FS 2016 requer que a porta TCP 49443 seja habilitada a entrada no firewall entre os clientes e o Proxy de aplicativo Web. Isso não é necessário no firewall entre o Proxy de aplicativo Web e os servidores de Federação\). 

Para obter mais informações sobre porta híbrida requisitos, consulte [protocolos e portas de identidade híbrida](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports). 

Para obter mais informações, consulte [práticas recomendadas para proteger os serviços de Federação do Active Directory](..\deployment\Best-Practices-Securing-AD-FS.md)
  
**Requisitos de DNS**  
  
-   Para acesso à intranet, todos os clientes que acessam o AD FS do serviço dentro da rede corporativa interna \(intranet\) deve ser capaz de resolver o nome do serviço do AD FS para o balanceador de carga para servidores do AD FS ou servidor do AD FS.  
  
-   Para obter acesso à extranet, todos os clientes que acessam o AD FS do serviço de fora da rede corporativa \(extranet\/internet\) deve ser capaz de resolver o nome do serviço do AD FS para o balanceador de carga para os servidores de Proxy de aplicativo Web ou o servidor de Proxy de aplicativo Web.  
  
-   Cada servidor de Proxy de aplicativo Web na rede de Perímetro deve ser capaz de resolver o nome de serviço do AD FS para o balanceador de carga para servidores do AD FS ou servidor do AD FS. Isso pode ser feito usando um servidor DNS alternativo na rede DMZ ou alterando a resolução de servidor local usando o arquivo de HOSTS.  
  
-   Para a autenticação integrada do Windows, você deve usar um registro DNS A \(CNAME não\) para o nome do serviço de Federação.  

-   Para autenticação de certificado de usuário na porta 443, "certauth. \<nome do serviço de Federação\>"deve ser configurado no DNS para resolver para o proxy do aplicativo web ou servidor de Federação.

-   Para o registro de dispositivos ou para autenticação moderna para recursos locais usando clientes de versões anteriores ao Windows 10, "enterpriseregistration. \<sufixo upn\>", para cada sufixo UPN em uso na sua organização, deve ser configurado para resolver para o proxy do aplicativo web ou servidor de Federação.

**Requisitos de Balanceador de carga**
- O balanceador de carga não deve terminar SSL. O AD FS dá suporte a vários casos de uso com a autenticação de certificado que será interrompido quando a terminação SSL. Não há suporte para a terminação SSL no balanceador de carga para qualquer caso de uso. 
- É recomendável usar um balanceador de carga que dá suporte ao SNI. No evento não existir, usando o 0.0.0.0 fallback de associação no seu AD FS / servidor de Proxy de aplicativo Web deve fornecer uma solução alternativa.
- É recomendável usar os pontos de extremidade de investigação de integridade HTTP (não HTTPS) para executar verificações de integridade do balanceador de carga para rotear o tráfego. Isso evita problemas relacionados ao SNI. A resposta a esses pontos de extremidade de investigação é um Okey de 200 HTTP e é servida localmente com nenhuma dependência no serviços de back-end. A investigação HTTP pode ser acessada via HTTP usando o caminho '/ adfs/investigação'
    - http://&lt;nome do Proxy de aplicativo Web &gt; /adfs/teste
    - http://&lt;nome do servidor ADFS &gt; /adfs/teste
    - http://&lt;endereço IP de Proxy de aplicativo Web &gt; /adfs/teste
    - http://&lt;endereço IP do ADFS &gt; /adfs/teste
- NÃO é recomendável usar o DNS round robin como uma maneira de balancear a carga. Usando esse tipo de balanceamento de carga não fornece uma forma automatizada para remover um nó do balanceador de carga usando as investigações de integridade. 
- É recomendável não usar afinidade de sessão com base em IP ou sessões Autoadesivas para tráfego de autenticação no AD FS no balanceador de carga. Isso pode causar uma sobrecarga de determinados nós ao usar o protocolo de autenticação herdados para os clientes de email para se conectar aos serviços de email do Office 365 (Exchange Online). 

## <a name="BKMK_13"></a>Requisitos de permissões  
O administrador que executa a instalação e a configuração inicial do AD FS deve ter permissões de administrador local no servidor do AD FS.  Se o administrador local não tem permissões para criar objetos no Active Directory, eles primeiro devem ter um administrador de domínio crie os objetos necessários do AD e, em seguida, configurar o farm do AD FS usando o parâmetro AdminConfiguration.  
  
  

