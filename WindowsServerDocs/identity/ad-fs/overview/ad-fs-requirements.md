---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: Requisitos do AD FS 2016
description: "Requisitos para instalar os serviços de Federação do Active Directory."
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 81b51c751d5f54436b14450ef21bf49feb864290
ms.sourcegitcommit: 556361fe7c73c75d6cdc46a67dc88679fbe89c61
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/08/2018
---
# <a name="ad-fs-requirements"></a>Requisitos do AD FS

>Aplica-se a: Windows Server 2016

Estes são os requisitos para implantação do AD FS:  
  
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

Cada servidor AD FS e Proxy de aplicativo Web tem um certificado SSL para solicitações de serviço de HTTPS para o serviço de Federação.  O Proxy de aplicativo Web pode ter certificados SSL adicionais para solicitações de serviço a aplicativos publicados.

**Recomendação:** usam o mesmo certificado SSL para todos os servidores de Federação do AD FS e proxies de aplicativo Web. 

**Requisitos:**

Certificados SSL em servidores de Federação devem atender aos seguintes requisitos
- Certificado é publicamente confiável (para implantações de produção)
- Certificado contém o valor de uso de chave avançado (EKU) do servidor autenticação
- Certificado contém o nome do serviço de federação, como "fs.contoso.com" no assunto ou nome de alternativa de assunto (SAN)
- Para a autenticação de certificado de usuário na porta 443, certificado contém "certauth. \ < name\ de serviço de Federação >", como "certauth.fs.contoso.com" na SAN
- Para registro de dispositivo ou para autenticação em recursos locais usando anterior ao Windows 10 clientes moderna, SAN deve conter "enterpriseregistration. \ < nome upn suffix\ >" para cada sufixo em uso em sua organização.

Certificados SSL no Proxy de aplicativo da Web devem atender aos seguintes requisitos
- Se o proxy é usado para solicitações de proxy do AD FS que usam a autenticação integrada do Windows, o certificado SSL proxy devem ser os mesmos (use a mesma chave) como o certificado SSL do servidor de Federação
- Se a propriedade AD FS "ExtendedProtectionTokenCheck" está habilitado (a configuração padrão no AD FS), o certificado SSL proxy deve ser o mesmo (use a mesma chave) que o certificado SSL do servidor de Federação
- Caso contrário, os requisitos para o certificado SSL proxy são as mesmas para o certificado SSL do servidor de Federação

### <a name="service-communication-certificate"></a>Certificado de comunicação do serviço
Esse certificado não é necessário para a maioria dos cenários do AD FS incluindo do Azure AD e o Office 365. Por padrão, o AD FS configura o certificado SSL fornecido após a configuração inicial como o certificado de comunicação do serviço.

**Recomendação:**
- Use o mesmo certificado como você usa para SSL.  

### <a name="token-signing-certificate"></a>Certificado de assinatura de token
Esse certificado é tokens usados entrada emitida para partes confiantes, portanto terceira aplicativos de terceiros devem reconhecer o certificado e associado chave como conhecida e confiável. Quando as token alterações de certificado assinatura, como quando ele expirar e configurar um novo certificado, todas as partes confiantes devem ser atualizadas.

**Recomendação:** usar o AD FS gerado por default, internamente, token autoassinado certificados de assinatura.  

**Requisitos:**
- Se sua organização requer que os certificados da PKI empresa seja usada para o token de assinatura, isso pode ser feito usando o parâmetro SigningCertificateThumbprint do cmdlet Install-AdfsFarm.
- Se você usar os certificados padrão gerado internamente ou externamente inscrito certificados, quando o certificado de assinatura do token é alterado e você deve garantir que todas as partes confiantes são atualizadas com as novas informações de certificado.  Caso contrário, ocorrerá uma falha logons para qualquer partes confiantes não atualizados.

### <a name="token-encryptingdecrypting-certificate"></a>Certificado de criptografia/descriptografia token
Esse certificado é usado pelos provedores de declarações que criptografar tokens emitidos para AD FS.

**Recomendação:** usar o AD FS gerado por default, internamente, autoassinado token de descriptografia de certificados.  

**Requisitos:**
- Se sua organização requer que os certificados da PKI empresa seja usada para o token de assinatura, isso pode ser feito usando o parâmetro DecryptingCertificateThumbprint do cmdlet Install-AdfsFarm.
- Se você usar os certificados padrão gerado internamente ou externamente inscrito certificados, quando o token de descriptografia de certificado é alterado e você deve garantir que todos os provedores de declarações são atualizados com as novas informações de certificado.  Caso contrário, logons usando qualquer declarações provedores não atualizados falhará.
  
> [!CAUTION]  
> Certificados que são usados para assinar token\ e token\-decrypting\/criptografar são fundamentais para a estabilidade do serviço de Federação. Clientes que gerenciam seus próprios certificados de assinatura de token\ & token\-decrypting\/criptografando devem garantir que esses certificados são armazenados em backup e estão disponíveis independentemente durante um evento de recuperação.  

### <a name="user-certificates"></a>Certificados de usuário
- Ao usar x509 autenticação de certificado de usuário com o AD FS, todos os certificados de usuário deve encadear a uma autoridade de certificação raiz confiável para os servidores do AD FS e Proxy de aplicativo Web.

## <a name="BKMK_2"></a>Requisitos de hardware  
Requisitos de hardware do AD FS e Proxy de aplicativo Web (físicos ou virtuais) são limitados na CPU, portanto, você deve dimensionar seu farm para capacidade de processamento.  
- Use o [planilha de planejamento da capacidade do AD FS 2016](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx) para determinar o número de servidores do AD FS e Proxy de aplicativo Web, será necessário.

Os requisitos de memória e do disco do AD FS são relativamente estáticos, consulte a tabela a seguir:


|**Requisito de hardware**|**Requisito mínimo**|**Requisito recomendado**|
|----- | ----- |-----|
|RAM|2 GB|4 GB |
|Espaço em disco|32 GB|100 GB |

**Requisitos de Hardware do SQL Server**

Se você estiver usando o SQL Server para seu banco de dados de configuração do AD FS, dimensione o SQL Server acordo com as recomendações mais básicas do SQL Server.  O tamanho de banco de dados do AD FS é muito pequeno e AD FS não colocar uma carga de processamento significativas na instância do banco de dados.  AD FS, no entanto, se conectar ao banco de dados várias vezes durante uma autenticação, portanto, a conexão de rede deve ser robusto.  Infelizmente, SQL Azure não há suporte para o banco de dados de configuração do AD FS.
  
## <a name="BKMK_3"></a>Requisitos de proxy  
  
-   Para obter acesso à extranet, você deve implantar o serviço de função de Proxy de aplicativo Web \ - parte da função de servidor de acesso remoto. 

-   Proxies de terceiros devem dar suporte a [protocolo MS-ADFSPIP](https://msdn.microsoft.com/en-us/library/dn392811.aspx) com suporte como um proxy do AD FS.  Veja uma lista de terceiros 3ª fornecedores o [perguntas frequentes sobre](AD-FS-FAQ.md#what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip).

-   AD FS 2016 requer servidores Proxy de aplicativo Web no Windows Server 2016.  Um proxy de nível inferior não pode ser configurado para um farm AD FS 2016 executado no nível de comportamento de fazenda de 2016.
  
-   Um servidor de Federação e o serviço de função de Proxy de aplicativo Web não podem ser instalados no mesmo computador.  
  
## <a name="BKMK_4"></a>Requisitos do AD DS  
**Requisitos de controlador de domínio**  
  
- AD FS requer controladores de domínio que executam o Windows Server 2008 ou posterior.

- Pelo menos um controlador de domínio do Windows Server 2016 é necessário para o Microsoft Passport for Work.
  
> [!NOTE]  
> Todo o suporte para ambientes com controladores de domínio do Windows Server 2003 terminou. Visite [nesta página](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) para obter informações adicionais sobre o ciclo de vida de suporte do Microsoft.  
  
**Requisitos de nível de functional\ de domínio**  
  
 - Todos os domínios de conta de usuário e o domínio ao qual os servidores do AD FS ingressam deverão estar operando com o nível funcional de domínio do Windows Server 2003 ou superior.  
  
 - Um domínio do Windows Server 2008 funcional nível ou superior é necessário para autenticação de certificado cliente se o certificado explicitamente é mapeado para uma conta do usuário no AD DS.  
  
**Requisitos de esquema**  
  
-   Novas instalações do AD FS 2016 exigem o esquema do Active Directory 2016 (versão mínima 85).

-   Aumentar o nível de comportamento de farm do AD FS (FBL) para o nível de 2016 requer o esquema do Active Directory 2016 (versão mínima 85).  

  
**Requisitos de conta de serviço**  
  
-   Qualquer conta de domínio padrão pode ser usada como uma conta de serviço do AD FS. Também há suporte para contas de serviço gerenciado do grupo. As permissões necessárias no tempo de execução serão adicionadas automaticamente quando você configura o AD FS.

-   Contas de serviço do grupo gerenciado exigem pelo menos um controlador de domínio que executam o Windows Server 2012 ou superior.  O GMSA deve residir em padrão ' CN = contêiner de contas de serviço gerenciadas.  

-   Para a autenticação Kerberos, nome da entidade de serviço '`HOST/<adfs\_service\_name>`' deve ser registrado na conta de serviço do AD FS. Por padrão, o AD FS configurará isso ao criar um novo farm AD FS.  Se isso falhar, como no caso de uma colisão ou permissões insuficientes, você verá um aviso e você deve adicioná-lo manualmente.  
   
**Requisitos de domínio**  
  
-   Todos os servidores do AD FS devem ser associados a um domínio do AD DS.  
  
-   Todos os servidores do AD FS dentro de um farm devem ser implantados no mesmo domínio.  
   
**Requisitos de várias florestas**  
  
-   O domínio ao qual ingressam os servidores do AD FS deve confiar em cada domínio ou floresta que contém os usuários que se autenticam para o serviço do AD FS.  

-   Floresta, que a conta de serviço do AD FS for membro de, deve confiar em todas as florestas de logon do usuário. 
  
-   A conta de serviço do AD FS deve ter permissões para ler os atributos de usuário em cada domínio que contém a autenticação de usuários para o serviço do AD FS.  
  
## <a name="BKMK_5"></a>Requisitos de banco de dados de configuração  
Esta seção descreve os requisitos e as restrições para farms AD FS que usam respectivamente o banco de dados interno do Windows (trabalho) ou o SQL Server como o banco de dados:  
  
**TRABALHO**  
  
-   Não há suporte para o perfil de resolução artefato de SAML 2.0 em um farm de trabalho.    

-   Detecção de repetição token não é suportado um farm de trabalho. (Essa funcionalidade só é usada apenas em cenários onde o AD FS é atuando como o provedor de Federação e consumindo tokens de segurança de provedores de declarações externa.)  
  
A tabela a seguir fornece um resumo dos servidores do AD FS quantos são compatíveis em um trabalho vs um farm de servidores do SQL.    
  
  
|| 1 - 100 dependência relações de confiança de terceiros (RP) configuradas no AD FS | Mais de 100 relações de confiança RP configuradas  |
| --- |--- | --- |
|Servidores para 1-30 AD FS|Trabalho com suporte|Não permitido usando trabalho - SQL Server necessária |
|Servidores do mais de 30 AD FS|Não permitido usando trabalho - SQL Server necessária|Não permitido usando trabalho - SQL Server necessária  
  
**SQL Server**  
  
- Do AD FS no Windows Server 2016, SQL Server 2008 e versões posteriores têm suporte.

- Resolução de artefato SAML e detecção de repetição token tem suporte em um farm de servidores SQL.  
  
## <a name="BKMK_6"></a>Requisitos de navegador  
Quando o AD FS autenticação é realizada por meio de um navegador ou controle de navegador, o navegador deve estar em conformidade aos seguintes requisitos:  
  
-   JavaScript deve estar habilitado  
  
-   Para SSO, o navegador do cliente deve ser configurado para permitir cookies  
  
-   Server Name indicação \(SNI\) deve ter suporte  
  
-   Certificado e dispositivos certificados para autenticação do usuário, o navegador deve dar suporte a autenticação de certificado de cliente SSL  

-   Para perfeita de logon usando a autenticação integrada do Windows, o nome do serviço de Federação (como https:\/\/fs.contoso.com) deve ser configurado na zona da intranet local ou zona de sites confiáveis.
## <a name="BKMK_7"></a>Requisitos de rede  
 
**Requisitos de firewall**  
  
Firewall localizado entre o Proxy de aplicativo Web e o farm de servidores de Federação e o firewall entre os clientes e o Proxy da Web do aplicativo devem ter a porta TCP 443 habilitado entrada.  
  
Além disso, se a autenticação de certificado de usuário do cliente \ (autenticação clientTLS usando X509 certificates\ do usuário) é necessária e o ponto de extremidade certauth na porta 443 não estiver habilitado, o AD FS 2016 requer a ativação de porta TCP 49443 entrada no firewall entre os clientes e o Proxy de aplicativo Web. Isso não é necessário no firewall entre o Proxy de aplicativo Web e o servers\ de federação). 

Para obter informações adicionais na porta híbrida requisitos, consulte [protocolos e portas de identidade híbrida](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports). 

Para obter informações adicionais, consulte [as práticas recomendadas para proteger os serviços de Federação do Active Directory](..\deployment\Best-Practices-Securing-AD-FS.md)
  
**Requisitos de DNS**  
  
-   Para acessar a intranet, todos os clientes que acessam serviços AD FS dentro \(intranet\) a rede corporativa interna devem ser capazes de resolver o nome do serviço do AD FS ao balanceador de carga para os servidores do AD FS ou o servidor do AD FS.  
  
-   Para obter acesso à extranet, todos os clientes que acessam o serviço AD FS de fora a rede corporativa \(extranet\/internet\) devem ser capazes de resolver o nome do serviço do AD FS ao balanceador de carga para os servidores Proxy de aplicativo Web ou o servidor Proxy de aplicativo Web.  
  
-   Cada servidor Proxy de aplicativo Web no DMZ deve ser capaz de resolver o nome de serviço do AD FS ao balanceador de carga para os servidores do AD FS ou o servidor do AD FS. Isso pode ser feito usando um servidor DNS alternativo na rede DMZ ou alterando a resolução de servidor local usando o arquivo de HOSTS.  
  
-   Para a autenticação integrada do Windows, você deve usar um DNS A registro \(not CNAME\) para o nome do serviço de Federação.  

-   Para a autenticação de certificado de usuário na porta 443, "certauth. \ < name\ de serviço de Federação >" deve ser configurada no DNS para resolver o servidor de Federação ou o proxy de aplicativo web.

-   Para registro de dispositivo ou para autenticação em recursos locais usando anterior ao Windows 10 clientes moderna, "enterpriseregistration. \ < nome upn suffix\ >", para cada sufixo em uso em sua organização, deve ser configurado para resolver o servidor de Federação ou o proxy de aplicativo web.

**Requisitos de Balanceador de carga**
- O balanceador não deve encerrar SSL. AD FS dá suporte a vários casos de uso com a autenticação de certificado que será interrompida quando encerramento SSL. Não há suporte para qualquer caso de uso encerramento SSL no balanceador de carga. 
- É recomendável usar um balanceador que dá suporte a SNI. Além de eventos não forem, usando a associação de fallback 0.0.0.0 no seu o AD FS / servidor Proxy de aplicativo da Web deve fornecer uma solução alternativa.
- É recomendável usar os pontos de extremidade do teste de integridade do HTTP (não HTTPS) para executar verificações de integridade de Balanceador de carga para o tráfego de roteamento. Isso evita problemas referentes à SNI. A resposta a esses pontos de extremidade de teste é um Okey de 200 HTTP e é atendida localmente com nenhuma dependência sobre serviços back-end. O teste HTTP pode ser acessado por HTTP usando o caminho '/ adfs/teste'
    - http://&lt;nome de Proxy de aplicativo Web&gt;/adfs/teste
    - http://&lt;nome do servidor ADFS&gt;/adfs/teste
    - http://&lt;endereço IP de Proxy de aplicativo Web&gt;/adfs/teste
    - http://&lt;endereço ADFS IP&gt;/adfs/teste
- NÃO é recomendável usar DNS rodízio como uma maneira de balanceamento de carga. Usar esse tipo de balanceamento de carga não fornece uma maneira automatizada para remover um nó de Balanceador de carga usando testes de integridade. 
- NÃO é recomendável usar IP com base em sessão afinidade ou sessões Autoadesivas para tráfego de autenticação para AD FS dentro balanceador de carga. Isso pode causar uma sobrecarga de determinados nós ao usar o protocolo de autenticação herdados para clientes de email para se conectar aos serviços de email do Office 365 (Exchange Online). 

## <a name="BKMK_13"></a>Requisitos de permissões  
O administrador que executa a instalação e a configuração inicial do AD FS deve ter permissões de administrador local no servidor do AD FS.  Se o administrador local não tem permissões para criar objetos no Active Directory, primeiro deve ter um administrador de domínio criar os objetos de anúncios necessários, em seguida, configurar o farm AD FS usando o parâmetro AdminConfiguration.  
  
  

