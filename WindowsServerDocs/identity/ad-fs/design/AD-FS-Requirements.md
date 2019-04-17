---
ms.assetid: 8ce6e7c4-cf8e-4b55-980c-048fea28d50f
title: "Farm de servidores de Federação usando o SQL Server"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e23154b20dfcd642178a5ac0de17f1b6a490f91f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-requirements"></a>Requisitos do AD FS

>Aplica-se a: Windows Server 2012 R2

Estes são os diversos requisitos que você deve estar em conformidade com ao implantar o AD FS:  
  
-   [Requisitos de certificado](AD-FS-Requirements.md#BKMK_1)  
  
-   [Requisitos de hardware](AD-FS-Requirements.md#BKMK_2)  
  
-   [Requisitos de software](AD-FS-Requirements.md#BKMK_3)  
  
-   [Requisitos do AD DS](AD-FS-Requirements.md#BKMK_4)  
  
-   [Requisitos de banco de dados de configuração](AD-FS-Requirements.md#BKMK_5)  
  
-   [Requisitos de navegador](AD-FS-Requirements.md#BKMK_6)  
  
-   [Requisitos de extranet](AD-FS-Requirements.md#BKMK_extranet)  
  
-   [Requisitos de rede](AD-FS-Requirements.md#BKMK_7)  
  
-   [Atributo requisitos da loja](AD-FS-Requirements.md#BKMK_8)  
  
-   [Requisitos de aplicativo](AD-FS-Requirements.md#BKMK_9)  
  
-   [Requisitos de autenticação](AD-FS-Requirements.md#BKMK_10)  
  
-   [Requisitos de ingresso no local de trabalho](AD-FS-Requirements.md#BKMK_11)  
  
-   [Requisitos de criptografia](AD-FS-Requirements.md#BKMK_12)  
  
-   [Requisitos de permissões](AD-FS-Requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Requisitos de certificado  
Certificados reproduzir a função mais importante na proteção das comunicações entre servidores de federação, Proxies de aplicativo da Web, aplicativos com reconhecimento de claims\ e clientes da Web. Requisitos de certificados variam, dependendo se você estiver configurando um servidor de Federação ou em um computador de proxy, conforme descrito nesta seção.  
  
**Certificados de servidor de Federação**  
  
|||  
|-|-|  
|**Tipo de certificado**|**Requisitos, suporte e coisas**|  
|**Seguro de certificados SSL \(SSL\):** se trata de um certificado SSL padrão que é usado para proteger as comunicações entre clientes e servidores de Federação.|-Esse certificado deve ser um publicamente trusted\ * X509 v3 certificado.<br />-Todos os clientes que acessam qualquer ponto de extremidade do AD FS devem confiar esse certificado. É altamente recomendável usar certificados são emitidos por uma autoridade de certificação pública \(third\-party\) \(CA\). Você pode usar um certificado SSL self\ assinado com êxito em servidores de Federação em um ambiente de laboratório de teste. No entanto, para um ambiente de produção, recomendamos que você obtenha o certificado de uma autoridade de certificação pública.<br />-Dá suporte a qualquer tamanho da chave suportado pelo Windows Server 2012 R2 para certificados SSL.<br />-Faz não certificados de suporte que usam chaves CNG.<br />-Quando usado junto com o serviço de registro do local de trabalho Join\/dispositivo, o nome alternativo de assunto do certificado SSL para o serviço do AD FS deve conter o valor enterpriseregistration que é seguido pelo sufixo do nome UPN \(UPN\) da sua organização, por exemplo, enterpriseregistration.contoso.com.<br />-Certificados curinga são compatíveis. Quando você cria seu farm AD FS, você será solicitado a fornecer o nome do serviço para o serviço do AD FS \ (por exemplo, **adfs.contoso.com**.<br />-É altamente recomendável usar o mesmo certificado SSL para o Proxy de aplicativo Web. Entretanto, isso é **necessária** para ser o mesmo ao oferecer suporte a pontos de extremidade de autenticação integrada do Windows por meio do Proxy de aplicativo Web e estendido autenticação de proteção é ativada \(default setting\).<br />-O nome do requerente do certificado é usado para representar o nome do serviço de federação para cada instância do AD FS que você implantar. Por esse motivo, convém considerar a escolher um nome de assunto em quaisquer certificados emitidos CA\ novo que melhor represente o nome da sua empresa ou organização para parceiros.<br />    A identidade do certificado deve corresponder ao nome do serviço de Federação \ (por exemplo, fs.contoso.com\). A identidade é uma extensão de nome alternativas de assunto do tipo dNSName ou, se não houver nenhuma entrada de nome alternativas de assunto, o nome do requerente especificado como um nome comum. Várias entradas de nome alternativas de assunto podem estar presentes no certificado, desde que um deles corresponde ao nome do serviço de Federação.<br />-   **Importante:** altamente é recomendável para usar o mesmo certificado SSL em todos os nós do farm AD FS, bem como todos os proxies de aplicativo Web no seu farm AD FS.|  
|**Certificado de comunicação de serviço:** esse certificado permite que a segurança de mensagem WCF para proteger as comunicações entre servidores de Federação.|-Por padrão, o certificado SSL é usado como o certificado de comunicações de serviço.  Mas você também tem a opção de configurar outro certificado como o certificado de comunicação do serviço.<br />-   **Importante:** se você estiver usando o certificado SSL como o certificado de comunicação do serviço, quando o certificado SSL expira, certifique-se de configurar o certificado SSL renovado como o certificado de comunicação do serviço. Isso não acontece automaticamente.<br />-Esse certificado deve ser confiável pelos clientes do AD FS que usam segurança de mensagem do WCF.<br />-Nós recomendamos que você use um certificado de autenticação de servidor é emitido por uma autoridade de certificação pública \(third\-party\) \(CA\).<br />-O certificado de comunicação do serviço não pode ser um certificado que usa chaves CNG.<br />-Esse certificado pode ser gerenciado usando o console de gerenciamento do AD FS.|  
|**Certificado de assinatura de Token\:** este é um padrão X509 certificado usado para assinar com segurança todos os tokens que o servidor de Federação emite.|-Por padrão, o AD FS cria um certificado assinado self\ com chaves de 2048 bits.<br />-Certificados emitido CA também são compatíveis e podem ser alterados usando o AD FS snap\-in Gerenciamento<br />-CA certificados emitido precisa ser armazenado e acessado por meio de um provedor de criptografia de CSP.<br />-O token de certificado de assinatura não pode ser um certificado que usa chaves CNG.<br />-AD FS não requer certificados externamente registrados para o token de assinatura.<br />    AD FS renova automaticamente esses certificados assinados self\ antes que elas expirem, primeiro Configurando os novos certificados como certificados secundários para permitir parceiros para usá-los, em seguida, invertendo à principal em um processo chamado sobreposição de certificado automática. Recomendamos que você use o padrão, os certificados gerados automaticamente para o token de assinatura.<br />    Se sua organização tiver políticas que exigem certificados diferentes que será configurada para o token de assinatura, você pode especificar os certificados no momento da instalação usando o Powershell \ (use o parâmetro – SigningCertificateThumbprint do cmdlet\ Install\ AdfsFarm).  Após a instalação, você pode exibir e gerenciar certificados de assinatura tokens usando o console de gerenciamento do AD FS ou cmdlets do Powershell Set \ AdfsCertificate e Get\ AdfsCertificate.<br />    Quando certificados externamente registrados são usados para o token de assinatura, AD FS não executa a renovação automática de certificados ou sobreposição.  Esse processo deve ser realizado por um administrador.<br />    Para permitir a substituição de certificado quando um certificado está próximo expirando, um certificado de assinatura de token secundário pode ser configurado no AD FS. Por padrão, todos os certificados de assinatura de tokens são publicados nos metadados de federação, mas apenas o certificado de assinatura de token\ principal é usado pelo AD FS realmente entre tokens.|  
|**Certificado de Token\-decryption\/criptografia:** este é um padrão X509 de certificado ou seja usada para decrypt\/criptografar tokens qualquer entradas. Ele também é publicado nos metadados de Federação.|-Por padrão, o AD FS cria um certificado assinado self\ com chaves de 2048 bits.<br />-Certificados emitido CA também são compatíveis e podem ser alterados usando o AD FS snap\-in Gerenciamento<br />-CA certificados emitido precisa ser armazenado e acessado por meio de um provedor de criptografia de CSP.<br />-O certificado token\-decryption\/criptografia não pode ser um certificado que usa chaves CNG.<br />-Por padrão, o AD FS gera e usa seus próprio, certificados internamente gerados e assinado self\ para descriptografia token.  AD FS não requer certificados externamente registrados para essa finalidade.<br />    Além disso, o AD FS renova automaticamente esses certificados assinados self\ antes que elas expirem.<br />    **Recomendamos que você use o padrão, os certificados gerados automaticamente para descriptografia token.**<br />    Se sua organização tem políticas que exigem certificados diferentes que será configurada para descriptografia token, você pode especificar os certificados no momento da instalação usando o Powershell \ (use o parâmetro – DecryptionCertificateThumbprint do cmdlet\ Install\ AdfsFarm).  Após a instalação, você pode exibir e gerenciar certificados de descriptografia token usando o console de gerenciamento do AD FS ou cmdlets do Powershell Set \ AdfsCertificate e Get\ AdfsCertificate.<br />    **Quando certificados externamente registrados são usados para descriptografia token, o AD FS não executa a renovação automática de certificado. Esse processo deve ser realizado por um administrador**.<br />-A conta de serviço do AD FS deve ter acesso à chave privada do certificado de assinatura de token\ no armazenamento pessoal do computador local. Isso é controlado pela instalação. Você também pode usar o AD FS snap\-in Gerenciamento para garantir que esse acesso se você alterar o certificado de assinatura de token\ posteriormente.|  
  
> [!CAUTION]  
> Certificados que são usados para assinar token\ e token\-decrypting\/criptografar são fundamentais para a estabilidade do serviço de Federação. Clientes que gerenciam seus próprios certificados de assinatura de token\ & token\-decrypting\/criptografando devem garantir que esses certificados são armazenados em backup e estão disponíveis independentemente durante um evento de recuperação.  
  
> [!NOTE]  
> No AD FS, você pode alterar o nível de Secure Hash Algorithm \(SHA\) que é usado para assinaturas digitais para \(more secure\) SHA\-1 ou SHA\-256. AD FS não suporta o uso de certificados com outros métodos de hash, como MD5 \ (o algoritmo de hash padrão que é usado com a tool\ de linha de command\ Makecert.exe). Como prática recomendada de segurança, recomendamos que você use SHA\-256 \ (que é definido pelo default\) para todas as assinaturas. SHA\-1 é recomendado para uso somente em cenários nos quais você deve interoperar com um produto que não oferece suporte a comunicações usando SHA\-256, como versões de produto ou herdado um non\ da Microsoft do AD FS.  
  
> [!NOTE]  
> Depois que você recebe um certificado de uma autoridade de certificação, certifique-se de que todos os certificados são importados para o repositório de certificados pessoal do computador local. Você pode importar certificados para o armazenamento pessoal com o MMC Certificados snap\-in.  
  
## <a name="BKMK_2"></a>Requisitos de hardware  
Os requisitos de hardware mínimos e recomendados a seguir se aplicam aos servidores de Federação do AD FS no Windows Server 2012 R2:  
  
||||  
|-|-|-|  
|**Requisito de hardware**|**Requisito mínimo**|**Requisito recomendado**|  
|Velocidade da CPU|Processador de 64\ bits de 1,4 GHz|Quad\-core, 2 GHz|  
|RAM|512 MB|4 GB|  
|Espaço em disco|32 GB|100 GB|  
  
## <a name="BKMK_3"></a>Requisitos de software  
Os seguintes requisitos do AD FS destinam-se a funcionalidade do servidor é integrada ao sistema operacional Windows Server® 2012 R2:  
  
-   Para obter acesso à extranet, você deve implantar o serviço de função de Proxy de aplicativo Web \ - parte da função de servidor de acesso remoto do Windows Server® 2012 R2. Não há suporte para versões anteriores de um proxy de servidor de federação com o AD FS no Windows Server® 2012 R2.  
  
-   Um servidor de Federação e o serviço de função de Proxy de aplicativo Web não podem ser instalados no mesmo computador.  
  
## <a name="BKMK_4"></a>Requisitos do AD DS  
**Requisitos de controlador de domínio**  
  
Controladores de domínio em todos os domínios de usuário e o domínio ao qual ingressam os servidores do AD FS devem estar executando o Windows Server 2008 ou posterior.  
  
> [!NOTE]  
> Todo o suporte para ambientes com controladores de domínio do Windows Server 2003 terminará após o estendido suporte final data para Windows Server 2003. Os clientes são altamente recomendados para atualizar os controladores de domínio assim que possível. Visite [nesta página](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) para obter informações adicionais sobre o ciclo de vida de suporte Microsoft. Para problemas descobertos que são específicos para ambientes de controlador de domínio do Windows Server 2003, correções serão emitidas somente para problemas de segurança e se uma correção pode ser emitida antes da expiração do estendido suporte para o Windows Server 2003.  
  
**Requisitos de nível de functional\ de domínio**  
  
Todos os domínios de conta de usuário e o domínio ao qual os servidores do AD FS ingressam deverão estar operando com o nível funcional de domínio do Windows Server 2003 ou superior.  
  
A maioria dos recursos do AD FS não exigem modificações de nível functional\ do AD DS para operar com êxito. No entanto, funcional do domínio do Windows Server 2008 nível ou superior é necessária para autenticação de certificado de cliente operar com êxito se o certificado é mapeado para uma conta do usuário no AD DS.  
  
**Requisitos de esquema**  
  
-   AD FS não exige alterações no esquema ou modificações de nível de functional\ no AD DS.  
  
-   Para usar a funcionalidade Workplace Join, o esquema da floresta que ingressaram em servidores do AD FS deve ser definido para o Windows Server 2012 R2.  
  
**Requisitos de conta de serviço**  
  
-   Qualquer conta de serviço padrão pode ser usada como uma conta de serviço do AD FS. Também há suporte para contas de serviço gerenciado do grupo. Isso requer pelo menos um controlador de domínio \ (é recomendável que você implantar dois ou more\) que está executando o Windows Server 2012 ou superior.  
  
-   Para autenticação Kerberos funcione entre clientes domain\ associado e AD FS, o ' HOST\ / < adfs\_service\_name >' deve ser registrado como um SPN da conta de serviço. Por padrão, o AD FS configurará isso ao criar um novo farm AD FS se ele tiver permissões suficientes para executar essa operação.  
  
-   A conta de serviço do AD FS deve ser confiável em todos os domínios de usuário que contém a autenticação de usuários para o serviço do AD FS.  
  
**Requisitos de domínio**  
  
-   Todos os servidores do AD FS devem ser associados a um domínio do AD DS.  
  
-   Todos os servidores do AD FS dentro de um farm devem ser implantados em um único domínio.  
  
-   O que os servidores do AD FS fazem parte de domínio deve confiar cada domínio da conta de usuário que contém os usuários que se autenticam para o serviço do AD FS.  
  
**Requisitos de várias florestas**  
  
-   O que os servidores do AD FS fazem parte de domínio deve confiar cada domínio da conta de usuário ou floresta que contém os usuários que se autenticam para o serviço do AD FS.  
  
-   A conta de serviço do AD FS deve ser confiável em todos os domínios de usuário que contém a autenticação de usuários para o serviço do AD FS.  
  
## <a name="BKMK_5"></a>Requisitos de banco de dados de configuração  
Estes são os requisitos e restrições que se aplicam com base no tipo de repositório de configuração:  
  
**TRABALHO**  
  
-   Um farm de trabalho tem um limite de 30 servidores de federação se você tiver 100 ou menos terceira parte relações de confiança.  
  
-   Perfil de resolução artefato em SAML 2.0 não é compatível com o banco de dados de configuração do trabalho.  Detecção de repetição token não é compatível com o banco de dados de configuração do trabalho. Essa funcionalidade só é usada apenas em cenários onde o AD FS é atuando como o provedor de Federação e consumindo tokens de segurança de provedores de declarações externo.  
  
-   Implantar servidores do AD FS nos dados distintos centros de failover ou balanceamento de carga geográfica é compatível como o número de servidores não exceder 30.  
  
A tabela a seguir fornece um resumo para usar um farm de trabalho.  Usá-lo a planejar sua implementação.  
  
||||  
|-|-|-|  
||1 \-100 relações de confiança RP|Mais de 100 relações de confiança RP|  
|1 \-30 AD FS nós|Trabalho com suporte|Não permitido usando trabalho \-SQL necessária|  
|Mais de 30 AD FS nós|Não permitido usando trabalho \-SQL necessária|Não permitido usando trabalho \-SQL necessária|  
  
**SQL Server**  
  
Para o AD FS no Windows Server 2012 R2, você pode usar o SQL Server 2008 e superior  
  
## <a name="BKMK_6"></a>Requisitos de navegador  
Quando o AD FS autenticação é realizada por meio de um navegador ou controle de navegador, o navegador deve estar em conformidade aos seguintes requisitos:  
  
-   JavaScript deve estar habilitado  
  
-   Cookies devem ser ativados  
  
-   Server Name indicação \(SNI\) deve ter suporte  
  
-   Para a autenticação de certificado de certificado e o dispositivo do usuário \ (functionality\ de ingresso no local de trabalho), o navegador deve dar suporte a autenticação de certificado de cliente SSL  
  
Várias plataformas e navegadores chaves passaram por validação para renderização e funcionalidade os detalhes dos quais estão listados abaixo. Ainda há suporte para navegadores e dispositivos que não são abordadas nesta tabela se eles atendem aos requisitos listados acima:  
  
|||  
|-|-|  
|**Navegadores**|**Plataformas**|  
|O IE 10.0|Windows 7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2|  
|O IE 11.0|Windows 7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2|  
|Agente de autenticação de Web do Windows|Windows 8.1|  
|Firefox \[v21\]|Windows 7, Windows 8.1|  
|Safari \[v7\]|iOS 6, Mac OS\-X 10.7|  
|Chrome \[v27\]|Windows 7, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Mac OS\-X 10.7|  
  
> [!IMPORTANT]  
> Problema conhecido \-Firefox: funcionalidade Workplace Join que identifica o dispositivo usando o certificado do dispositivo não está funciona em plataformas do Windows. Atualmente, o Firefox não dá suporte para a desempenho autenticação de certificado de cliente SSL usando certificados provisionados para o repositório de certificados de usuário em clientes do Windows.  
  
**Cookies**  
  
AD FS cria cookies persistentes e baseada em session\ que devem ser armazenados em computadores cliente para fornecer sign\-in, sign\-out, único \(SSO\) sign\-on e outras funcionalidades. Portanto, o navegador do cliente deve ser configurado para aceitar cookies. Cookies são usados para autenticação sempre são os cookies de sessão Secure Hypertext Transfer Protocol \(HTTPS\) que são escritos para o servidor de origem. Se o navegador cliente não estiver configurado para permitir que esses cookies, AD FS não funcionará corretamente. Cookies persistentes são usados para preservar a seleção do usuário do provedor de declarações. Você pode desativá-los usando uma configuração no arquivo de configuração para as páginas do AD FS sign\-in. Suporte para TLS\/SSL é necessário por motivos de segurança.  
  
## <a name="BKMK_extranet"></a>Requisitos de extranet  
Para fornecer acesso à extranet para o serviço do AD FS, você deve implantar o serviço de função de Proxy de aplicativo Web com a função oposta extranet que solicitações de autenticação de proxies de maneira segura para o serviço do AD FS. Isso proporciona isolamento dos pontos de extremidade de serviço do AD FS, bem como o isolamento de todas as chaves de segurança \ (como certificates\ token de assinatura) de solicitações que se originam da internet. Além disso, recursos como bloqueio de conta Extranet Virtual exigem o uso de Proxy de aplicativo Web. Para obter mais informações sobre o Proxy de aplicativo Web, consulte [Proxy de aplicativo Web](https://technet.microsoft.com/library/dn584107.aspx).  
  
Se você quiser usar um proxy de terceiros third\ para acesso à extranet, esse proxy third\ terceiros deve suportar o protocolo definido em [http:///\/download.microsoft.com\/download\/9\/5\/E\/95EF66AF\-9026\-4BB0\-A41D\-A4F81802D92C\/%5bMS\-ADFSPIP%5d.pdf](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf).  
  
## <a name="BKMK_7"></a>Requisitos de rede  
Configurar os seguintes serviços de rede adequadamente é crítica para a implantação bem-sucedida do AD FS em sua organização:  
  
**Configurando o Firewall corporativo**  
  
Firewall localizado entre o Proxy de aplicativo Web e o farm de servidores de Federação e o firewall entre os clientes e o Proxy da Web do aplicativo devem ter a porta TCP 443 habilitado entrada.  
  
Além disso, se a autenticação de certificado de usuário do cliente \ (autenticação clientTLS usando X509 certificates\ do usuário) é necessária, o AD FS no Windows Server 2012 R2 requer que a porta TCP 49443 esteja habilitado entrada no firewall entre os clientes e o Proxy de aplicativo Web. Isso não é necessário no firewall entre o Proxy de aplicativo Web e o servers\ de federação).  
  
**Configurando o DNS**  
  
-   Para acessar a intranet, todos os clientes que acessam serviços AD FS dentro \(intranet\) a rede corporativa interna devem ser capazes de resolver o nome de serviço do AD FS \ (nome fornecido pelo certificate\ SSL) para o balanceador para os servidores do AD FS ou o servidor do AD FS.  
  
-   Para obter acesso à extranet, todos os clientes que acessam o serviço AD FS de fora a rede corporativa \(extranet\/internet\) devem ser capazes de resolver o nome de serviço do AD FS \ (nome fornecido pelo certificate\ SSL) ao balanceador de carga para os servidores Proxy de aplicativo Web ou o servidor Proxy de aplicativo Web.  
  
-   Para obter acesso à extranet funcionar corretamente, cada servidor Proxy de aplicativo Web no DMZ deve ser capaz de resolver o nome de serviço do AD FS \ (nome fornecido pelo certificate\ SSL) ao balanceador de carga para os servidores do AD FS ou o servidor do AD FS. Isso pode ser feito usando um servidor DNS alternativo na rede DMZ ou alterando a resolução de servidor local usando o arquivo de HOSTS.  
  
-   Para a autenticação integrada do Windows funcionar dentro da rede e fora da rede para um subconjunto de pontos de extremidade expostos por meio do Proxy de aplicativo Web, você deve usar um um registro \(not CNAME\) para apontar para os balanceadores de carga.  
  
Para obter informações sobre como configurar DNS corporativo do serviço de Federação e serviço de registro de dispositivo, consulte [configurar corporativa DNS para o serviço de Federação e DRS](https://technet.microsoft.com/library/dn486786.aspx).  
  
Para obter informações sobre como configurar o DNS corporativa para proxies de aplicativo Web, consulte a seção "Configurar DNS" em [etapa 1: configurar a infraestrutura de Proxy de aplicativo Web](https://technet.microsoft.com/library/dn383644.aspx).  
  
Para obter informações sobre como configurar um endereço IP do cluster ou FQDN usando NLB do cluster, consulte especificar os parâmetros de Cluster em [http:///\/go.microsoft.com\/fwlink\/? LinkId\ = 75282](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
## <a name="BKMK_8"></a>Atributo requisitos da loja  
AD FS requer pelo menos um repositório de atributo a ser usado para autenticar usuários e extrair declarações de segurança para esses usuários. Para uma lista de atributo armazena AD FS dá suporte, consulte [função The do atributo armazena](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
> [!NOTE]  
> AD FS cria automaticamente um repositório de atributo "Active Directory", por padrão. Requisitos da loja atributo dependem se sua organização está atuando como o parceiro de conta \ (hospedagem os usuários \ federado) ou o parceiro de recurso \ (que hospeda o application\ federados).  
  
**Repositórios de atributo LDAP**  
  
Quando você trabalha com outros Lightweight Directory Access Protocol \ (LDAP\) \-based atributo lojas, você deve se conectar a um servidor LDAP que oferece suporte à autenticação integrada do Windows. A cadeia de caracteres de conexão LDAP também deve ser gravada no formato de uma URL LDAP, conforme descrito em RFC 2255.  
  
Também é necessário que a conta de serviço para o serviço do AD FS tem o direito de recuperar informações do usuário no repositório de atributo LDAP.  
  
**Repositórios de atributo do SQL Server**  
  
Do AD FS no Windows Server 2012 R2 para operar com êxito, computadores que hospedam o repositório de atributo do SQL Server devem estar executando o Microsoft SQL Server 2008 ou posterior. Quando você trabalha com o atributo com base em SQL\ lojas, você também deve configurar uma cadeia de caracteres de conexão.  
  
**Repositórios de atributo personalizado**  
  
Você pode desenvolver repositórios de atributo personalizado para habilitar cenários avançados.  
  
-   O idioma de política é integrado ao AD FS pode fazer referência a repositórios de atributo personalizado para que qualquer um dos cenários a seguir podem ser aperfeiçoados:  
  
    -   Criando declarações para um usuário autenticado localmente  
  
    -   Complementando declarações para um usuário autenticado externamente  
  
    -   Autorizar um usuário obtenha um token  
  
    -   Autorizar um serviço para obter um token no comportamento de um usuário  
  
    -   A emissão de dados adicionais nos tokens de segurança emitidos pelo AD FS para partes confiantes.  
  
-   Todas as lojas de atributo personalizado devem ser criadas na parte superior do .NET 4.0 ou posterior.  
  
Quando você trabalha com um repositório de atributo personalizado, você também terá que configurar uma cadeia de caracteres de conexão. Nesse caso, você pode inserir um código personalizado de sua escolha que permite que uma conexão para o repositório de atributo personalizado. A cadeia de caracteres de conexão nesta situação é um conjunto de pares de name\/valor que são interpretados como implementado pelo desenvolvedor do repositório de atributo personalizado. Para obter mais informações sobre como desenvolver e usando o atributo personalizado lojas, consulte [visão geral do atributo da Store](https://go.microsoft.com/fwlink/?LinkId=190782).  
  
## <a name="BKMK_9"></a>Requisitos de aplicativo  
AD FS dá suporte a aplicativos com reconhecimento de claims\ que usam os seguintes protocolos:  
  
-   Federação WS\  
  
-   Confiança WS\  
  
-   Protocolo SAML 2.0 usando perfis IDPLite, SPLite e eGov1.5.  
  
-   Perfil de concessão de autorização do OAuth 2.0  
  
AD FS também dá suporte à autenticação e autorização para quaisquer aplicativos com reconhecimento de claims\ non\ que são compatíveis com o Proxy de aplicativo Web.  
  
## <a name="BKMK_10"></a>Requisitos de autenticação  
**AD DS autenticação \(Primary Authentication\)**  
  
Para acessar a intranet, há suporte para os seguintes mecanismos de autenticação padrão para o AD DS:  
  
-   Autenticação integrada do Windows usando negociar para Kerberos e NTLM  
  
-   Autenticação de formulários usando username\/senhas  
  
-   Autenticação de certificado usando certificados mapeados para contas de usuário no AD DS  
  
Para obter acesso à extranet, os mecanismos de autenticação a seguir têm suporte:  
  
-   Autenticação de formulários usando username\/senhas  
  
-   Autenticação de certificado usando certificados que são mapeados para contas de usuário no AD DS  
  
-   Autenticação integrada do Windows usando negociar \(NTLM only\) para pontos de extremidade de confiança WS\ que aceitam a autenticação integrada do Windows.  
  
Para autenticação de certificado:  
  
-   Se estende para cartões inteligentes que podem ser protegido de pin.  
  
-   A interface gráfica do usuário para o usuário insira o pin não é fornecido pelo AD FS e é necessário para fazer parte do sistema operacional cliente que é exibido ao usar o cliente TLS.  
  
-   O leitor e o provedor de serviço criptográfico \(CSP\) para o cartão inteligente deve funcionar no computador onde o navegador está localizado.  
  
-   O certificado de cartão inteligente deve encadear em uma raiz confiável em todos os servidores do AD FS e servidores de Proxy de aplicativo Web.  
  
-   O certificado deve mapear para a conta de usuário no AD DS por qualquer um dos seguintes métodos:  
  
    -   Nome do requerente do certificado corresponde ao nome diferenciado LDAP de uma conta de usuário no AD DS.  
  
    -   A extensão de altname de assunto do certificado tem a entidade de segurança do usuário nomear \(UPN\) de uma conta de usuário no AD DS.  
  
Para perfeita autenticação integrada do Windows usando Kerberos na intranet,  
  
-   Isso é necessário para o nome do serviço ser parte de Sites confiáveis ou sites da Intranet Local.  
  
-   Além disso, a HOST\ / < adfs\_service\_name > SPN deve ser definida na conta de serviço que sítio AD FS é executado em.  
  
**Autenticação de fator Multi\**  
  
AD FS dá suporte à autenticação adicional \ (além da autenticação primária suportada pelo AD DS\) usando um provedor de modelo por meio da qual vendors\/os clientes podem criar seu próprios adaptador de autenticação de fator de multi\ que um administrador pode se registrar e usar durante o logon.  
  
Cada adaptador MFA deve ser criado com base em .NET 4.5.  
  
Para obter mais informações sobre MFA, consulte [gerenciar risco com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  
  
**Autenticação de dispositivo**  
  
AD FS oferece suporte à autenticação de dispositivo usando certificados provisionados pelo serviço de registro de dispositivo durante o ato de um espaço de trabalho do usuário final ingressar o dispositivo.  
  
## <a name="BKMK_11"></a>Requisitos de ingresso no local de trabalho  
Os usuários finais podem ingresso no local de trabalho seus dispositivos para uma organização usando o AD FS. Isso é compatível com o serviço de registro de dispositivo no AD FS. Como resultado, os usuários finais obtenham o benefício adicional de logon único em todos os aplicativos com suporte pelo AD FS. Além disso, os administradores podem gerenciar risco restringindo o acesso aos aplicativos apenas para dispositivos que foram empresa associada à organização. Abaixo estão os seguintes requisitos para habilitar esse cenário.  
  
-   AD FS suporta ingresso no local de trabalho para Windows 8.1 e o iOS 5 \ + dispositivos  
  
-   Para usar a funcionalidade Workplace Join, o esquema da floresta que ingressaram em servidores do AD FS deve ser Windows Server 2012 R2.  
  
-   O nome alternativo de assunto do certificado SSL para o serviço do AD FS deve conter o valor enterpriseregistration que é seguido pelo sufixo do nome UPN \(UPN\) da sua organização, por exemplo, enterpriseregistration.corp.contoso.com.  
  
## <a name="BKMK_12"></a>Requisitos de criptografia  
A tabela a seguir fornece informações de suporte de criptografia adicionais sobre o token do AD FS assinatura, funcionalidade de encryption\/descriptografia token:  
  
||||  
|-|-|-|  
|**Algoritmo**|**Tamanhos de chave**|**Applications\/Protocols\/comentários**|  
|Triplo DES – padrão 192 \ (256\ 192 – com suporte) \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#tripledes\-cbc](http://www.w3.org/2001/04/xmlenc)|>\= 192|Algoritmos compatíveis para criptografar o token de segurança.|  
|Aes128 \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#aes128\-cbc|128|Algoritmos compatíveis para criptografar o token de segurança.|  
|AES192 \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#aes192\-cbc|192|Algoritmos compatíveis para criptografar o token de segurança.|  
|AES256 \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#aes256\-cbc](http://www.w3.org/2001/04/xmlenc)|256|**Padrão**. Algoritmos compatíveis para criptografar o token de segurança.|  
|TripleDESKeyWrap \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-tripledes|Todos os tamanhos de chave suportados pelo .NET 4.0\+|Suporte para o algoritmo para criptografar a chave simétrica criptografada por um token de segurança.|  
|AES128KeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-aes128](http://www.w3.org/2001/04/xmlenc)|128|Suporte para o algoritmo para criptografar a chave simétrica que criptografa o token de segurança.|  
|AES192KeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-aes192](http://www.w3.org/2001/04/xmlenc)|192|Suporte para o algoritmo para criptografar a chave simétrica que criptografa o token de segurança.|  
|AES256KeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-aes256](http://www.w3.org/2001/04/xmlenc)|256|Suporte para o algoritmo para criptografar a chave simétrica que criptografa o token de segurança.|  
|RsaV15KeyWrap \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#rsa\-1\_5|1024|Suporte para o algoritmo para criptografar a chave simétrica que criptografa o token de segurança.|  
|RsaOaepKeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#rsa\-oaep\-mgf1p](http://www.w3.org/2001/04/xmlenc)|1024|Padrão. Suporte para o algoritmo para criptografar a chave simétrica que criptografa o token de segurança.|  
|SHA1\ http: / / / \ / www.w3.org \/PICS\/DSig\/SHA1\_1\_0.html|N\/A|Usado pelo servidor do AD FS na geração de SourceId artefato: neste cenário, o STS usa SHA1 \ (de acordo com a recomendação no padrão 2.0 SAML) para criar um valor de bit de 160 curto para o sourceiD artefato.<br /><br />Também é usado pelo agente de web do AD FS \ (componente herdado de WS2003 timeframe\) para identificar as alterações em um tempo de "última atualização" valor para que ele saiba quando atualizar informações do STS.|  
|SHA1withRSA\-<br /><br />http:///\/ www.w3.org \/PICS\/DSig\/RSA\-SHA1\_1\_0.html|N\/A|Usado em casos quando o servidor do AD FS valida a assinatura de SAML AuthenticationRequest, entre a solicitação de resolução de artefato ou resposta, crie um certificado de assinatura de token\.<br /><br />Nesses casos, SHA256 é o padrão e SHA1 só é usada quando o parceiro \(relying party\) não dá suporte ao SHA256 e deve usar SHA1.|  
  
## <a name="BKMK_13"></a>Requisitos de permissões  
O administrador que executa a instalação e a configuração inicial do AD FS deve ter permissões de administrador de domínio no domínio local \ (em outras palavras, o domínio ao qual o servidor de Federação está associado à. \)  
  
## <a name="see-also"></a>Consulte também  
[Guia de Design do AD FS no Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

