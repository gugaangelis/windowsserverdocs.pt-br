---
ms.assetid: 8ce6e7c4-cf8e-4b55-980c-048fea28d50f
title: Farm de servidores de federação usando SQL Server
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c6aa91956f4a90b32b82e6c970e68b3164c732f0
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191711"
---
# <a name="ad-fs-requirements"></a>Requisitos do AD FS

A seguir estão os vários requisitos que você deve estar em conformidade quando implantar o AD FS:  
  
-   [Requisitos de certificado](AD-FS-Requirements.md#BKMK_1)  
  
-   [Requisitos de hardware](AD-FS-Requirements.md#BKMK_2)  
  
-   [Requisitos de software](AD-FS-Requirements.md#BKMK_3)  
  
-   [Requisitos do AD DS](AD-FS-Requirements.md#BKMK_4)  
  
-   [Requisitos de banco de dados de configuração](AD-FS-Requirements.md#BKMK_5)  
  
-   [Requisitos de navegador](AD-FS-Requirements.md#BKMK_6)  
  
-   [Requisitos de extranet](AD-FS-Requirements.md#BKMK_extranet)  
  
-   [Requisitos de rede](AD-FS-Requirements.md#BKMK_7)  
  
-   [Requisitos de repositório de atributos](AD-FS-Requirements.md#BKMK_8)  
  
-   [Requisitos do aplicativo](AD-FS-Requirements.md#BKMK_9)  
  
-   [Requisitos de autenticação](AD-FS-Requirements.md#BKMK_10)  
  
-   [Requisitos de junção de local de trabalho](AD-FS-Requirements.md#BKMK_11)  
  
-   [Requisitos de criptografia](AD-FS-Requirements.md#BKMK_12)  
  
-   [Requisitos de permissões](AD-FS-Requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Requisitos de certificado  
Os certificados desempenham a função mais essencial na proteção das comunicações entre federação declarações de servidores, Proxies de aplicativo Web,\-clientes Web e aplicativos com reconhecimento. Os requisitos dos certificados variam, dependendo se você estiver configurando um servidor de Federação ou computador proxy, conforme descrito nesta seção.  
  
**Certificados de servidor de Federação**  
  
|||  
|-|-|  
|**Tipo de certificado**|**Requisitos de suporte e que você precisa saber**|  
|**Secure Sockets Layer \(SSL\) certificado:** Isso é um certificado SSL padrão que é usado para proteger as comunicações entre clientes e servidores de Federação.|-Esse certificado deve ser confiável publicamente\* X509 certificado v3.<br />-Todos os clientes que acessam qualquer ponto de extremidade do AD FS devem confiar nesse certificado. É altamente recomendável usar certificados emitidos por um público \(terceiro\-terceiros\) autoridade de certificação \(autoridade de certificação\). Você pode usar um self\-assinado SSL certificado com êxito em servidores de Federação em um ambiente de laboratório de teste. No entanto, para um ambiente de produção, recomendamos que você obtenha o certificado de uma CA pública.<br />-Oferece suporte a qualquer tamanho de chave com suporte pelo Windows Server 2012 R2 para certificados SSL.<br />-Não não suporte a certificados que usam chaves CNG.<br />-Quando usado em conjunto com o Workplace Join\/Device Registration Service, o nome alternativo da entidade do certificado SSL para o serviço AD FS deve conter o enterpriseregistration de valor é seguido pelo nome da entidade de usuário \(UPN\) sufixo da sua organização, por exemplo, enterpriseregistration.corp.contoso.com.<br />-Certificados curinga são suportados. Quando você cria um farm do AD FS, você precisará fornecer o nome do serviço para o serviço AD FS \(por exemplo, **adfs.contoso.com**.<br />– Ele é altamente recomendável usar o mesmo certificado SSL para o Proxy de aplicativo Web. No entanto, isso é **necessária** sejam iguais ao dar suporte a pontos de extremidade de autenticação integrada do Windows por meio do Proxy de aplicativo Web e a autenticação de proteção estendida está ativada \(configuração padrão\).<br />-O nome da entidade desse certificado é usado para representar o nome do serviço de federação para cada instância do AD FS que você implanta. Por esse motivo, você talvez queira considerar a escolha de um nome de assunto na nova autoridade de certificação\-que melhor representa o nome da sua empresa ou organização para os parceiros de certificados emitidos.<br />    A identidade do certificado deve corresponder ao nome de serviço de Federação \(por exemplo, fs.contoso.com\). A identidade é uma extensão de nome alternativo de assunto do tipo dNSName ou, se não houver nenhuma entrada de nome alternativo da entidade, o nome de assunto especificado como um nome comum. Várias entradas de nome alternativo da entidade podem ser presentes no certificado, desde que uma delas coincide com o nome do serviço de Federação.<br />-   **Importante:** é altamente recomendável usar o mesmo certificado SSL em todos os nós do farm do AD FS, bem como todos os proxies de aplicativo Web no farm do AD FS.|  
|**Certificado de comunicação de serviço:** Esse certificado habilita a segurança da mensagem WCF (Windows Communication Foundation) para proteger as comunicações entre os servidores de federação.|-Por padrão, o certificado SSL é usado como o certificado de comunicações de serviço.  Mas você também tem a opção de configurar outro certificado como o certificado de comunicação de serviço.<br />-   **Importante:** se você estiver usando o certificado SSL como o certificado de comunicação de serviço, quando expira o certificado SSL, certifique-se de configurar o certificado SSL renovado como seu certificado de comunicação de serviço. Isso não acontece automaticamente.<br />-Este certificado deve ser confiável pelos clientes do AD FS que usam a segurança de mensagem do WCF.<br />-É recomendável que você use um certificado de autenticação de servidor emitido por um público \(terceiro\-terceiros\) autoridade de certificação \(autoridade de certificação\).<br />-O certificado de comunicação de serviço não pode ser um certificado que usa chaves CNG.<br />-Esse certificado pode ser gerenciado usando o console de gerenciamento do AD FS.|  
|**Token\-certificado de assinatura:** Este é um certificado X509 padrão usado para autenticar com segurança todos os tokens que o servidor de federação emite.|-Por padrão, o AD FS cria um self\-certificado autoassinado com chaves de 2048 bits.<br />-Certificados de AC emitido também têm suporte e podem ser alterados usando o snap-gerenciamento do AD FS\-em<br />-Autoridade de certificação emitida os certificados precisa ser armazenada e acessada por meio de um provedor de criptografia do CSP.<br />-O certificado de assinatura de token não pode ser um certificado que usa chaves CNG.<br />-O AD FS não requer certificados registrados externamente para autenticação de tokens.<br />    O AD FS renova automaticamente esses self\-certificados autoassinados, antes de expirarem, configurando primeiro os novos certificados como certificados secundários para permitir a parceiros consumi-los, e em seguida, invertendo a primária em um processo chamado automática substituição do certificado. É recomendável que você use o padrão, gerados automaticamente certificados para autenticação de tokens.<br />    Se sua organização tiver políticas que exigem certificados diferentes para ser configurado para autenticação de token, você pode especificar os certificados no momento da instalação usando o Powershell \(usa o parâmetro – SigningCertificateThumbprint da instalação \-AdfsFarm cmdlet\).  Após a instalação, você pode exibir e gerenciar certificados de assinatura de token usando o console de gerenciamento do AD FS ou um conjunto de cmdlets do Powershell\-AdfsCertificate e Get\-AdfsCertificate.<br />    Quando forem usados certificados registrados externamente para autenticação de tokens, do AD FS não executa a renovação automática de certificados ou substituição.  Esse processo deve ser executado por um administrador.<br />    Para permitir a substituição do certificado quando ele estiver perto de expirar, um certificado de assinatura de token secundário pode ser configurado no AD FS. Por padrão, todos os certificados de autenticação de token são publicados em metadados de federação, mas apenas o token primário\-certificado de autenticação é usado pelo AD FS para realmente assinar tokens.|  
|**Token\-descriptografia\/certificado de criptografia:** Esse é um padrão X509 certificado que é usado para descriptografar\/criptografar todos os tokens recebidos. Ele também é publicado nos metadados de federação.|-Por padrão, o AD FS cria um self\-certificado autoassinado com chaves de 2048 bits.<br />-Certificados de AC emitido também têm suporte e podem ser alterados usando o snap-gerenciamento do AD FS\-em<br />-Autoridade de certificação emitida os certificados precisa ser armazenada e acessada por meio de um provedor de criptografia do CSP.<br />-O token\-descriptografia\/certificado de criptografia não pode ser um certificado que usa chaves CNG.<br />-Por padrão, o AD FS gera e usa seu próprio, internamente gerado e self\-assinado certificados de descriptografia de token.  O AD FS não requer certificados registrados externamente para essa finalidade.<br />    Além disso, o AD FS renova automaticamente esses self\-certificados autoassinados, antes de expirarem.<br />    **É recomendável que você use o padrão, gerados automaticamente certificados de descriptografia de token.**<br />    Se sua organização tiver políticas que exigem certificados diferentes para ser configurado para a descriptografia de token, você pode especificar os certificados no momento da instalação usando o Powershell \(usar o parâmetro – DecryptionCertificateThumbprint das Instale\-AdfsFarm cmdlet\).  Após a instalação, você pode exibir e gerenciar certificados de descriptografia de token usando o console de gerenciamento do AD FS ou um conjunto de cmdlets do Powershell\-AdfsCertificate e Get\-AdfsCertificate.<br />    **Quando certificados registrados externamente são usados para descriptografia de token, o AD FS não executa a renovação automática de certificados.  Esse processo deve ser executado por um administrador**.<br />-A conta de serviço do AD FS deve ter acesso ao token\-assinatura de chave privada do certificado no repositório pessoal do computador local. Isso é resolvido pela instalação. Você também pode usar o snap de gerenciamento do AD FS\-para garantir que esse acesso, se você alterar posteriormente o token\-certificado de assinatura.|  
  
> [!CAUTION]  
> Certificados que são usados para o token\-assinatura e o token\-descriptografar\/criptografar são essenciais para a estabilidade do serviço de Federação. Os clientes que gerenciam sua próprias token\-assinatura & token\-descriptografar\/certificados de criptografia deve garantir que esses certificados são obtidos em backup e estão disponíveis independentemente durante um evento de recuperação.  
  
> [!NOTE]  
> No AD FS, você pode alterar o algoritmo de Hash seguro \(SHA\) nível que é usado para assinaturas digitais para qualquer um dos SHA\-1 ou SHA\-256 \(mais seguro\). O AD FS não suporta o uso de certificados com outros métodos de hash, como MD5 \(o algoritmo de hash padrão que é usado com o comando Makecert.exe\-ferramenta de linha de\). Como prática recomendada de segurança, recomendamos que você use SHA\-256 \(que é definido por padrão\) para todas as assinaturas. SHA\-1 é recomendado para uso apenas em cenários em que você deve interoperar com um produto que não oferece suporte a comunicações usando SHA\-256, como um não\-versões herdado ou produto da Microsoft do AD FS.  
  
> [!NOTE]  
> Depois de receber um certificado de uma AC, certifique-se de que todos os certificados sejam importados para o repositório de certificados pessoais do computador local. Você pode importar certificados para o repositório pessoal com snap do MMC de certificados\-no.  
  
## <a name="BKMK_2"></a>Requisitos de hardware  
Os requisitos de hardware mínimos e recomendados a seguir se aplicam a servidores de Federação do AD FS no Windows Server 2012 R2:  
  
||||  
|-|-|-|  
|**Requisito de hardware**|**Requisito mínimo**|**Requisito recomendado**|  
|Velocidade da CPU|1,4 GHz e 64\-processador de bits|Quad\-núcleo, 2 GHz|  
|RAM|512 MB|4 GB|  
|Espaço em disco|32 GB|100 GB|  
  
## <a name="BKMK_3"></a>Requisitos de software  
Os seguintes requisitos do AD FS são para a funcionalidade do servidor que está embutida no sistema operacional Windows Server® 2012 R2:  
  
-   Para obter acesso à extranet, você deve implantar o serviço de função Proxy de aplicativo Web \- faz parte da função de servidor de acesso remoto do Windows Server® 2012 R2. Não há suporte para versões anteriores de um proxy do servidor de federação com AD FS no Windows Server® 2012 R2.  
  
-   Um servidor de Federação e o serviço de função Proxy de aplicativo Web não podem ser instalados no mesmo computador.  
  
## <a name="BKMK_4"></a>Requisitos do AD DS  
**Requisitos do controlador de domínio**  
  
Controladores de domínio em todos os domínios de usuário e o domínio ao qual os servidores do AD FS são Unidos devem estar executando o Windows Server 2008 ou posterior.  
  
> [!NOTE]  
> Todo o suporte para ambientes com controladores de domínio do Windows Server 2003 será encerrada após o Extended suporte final data para Windows Server 2003. Os clientes são altamente recomendáveis para atualizar seus controladores de domínio assim que possível. Visite [nesta página](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) para obter mais informações sobre o ciclo de vida de suporte Microsoft. Para problemas descobertos que são específicas para ambientes de controlador de domínio do Windows Server 2003, correções serão emitidas somente para problemas de segurança e se uma correção pode ser emitida antes da expiração da estendido o suporte do Windows Server 2003.  
  
**Funcional de domínio\-requisitos de nível**  
  
Todos os domínios de conta de usuário e o domínio ao qual os servidores do AD FS são Unidos deverão estar operando com o nível funcional de domínio do Windows Server 2003 ou superior.  
  
A maioria dos recursos do AD FS não exigem o AD DS funcional\-nível modificações para operar com êxito. No entanto, o nível funcional do domínio do Windows Server 2008 ou superior é exigido para que a autenticação de certificados de clientes funcione bem, caso o certificado esteja associado explicitamente à conta de usuário do AD DS.  
  
**Requisitos do esquema**  
  
-   O AD FS não requer alterações de esquema ou funcional\-nível modificações para o AD DS.  
  
-   Para usar a funcionalidade de ingresso no local, o esquema da floresta que ingressaram em servidores do AD FS deve ser definido para o Windows Server 2012 R2.  
  
**Requisitos de conta de serviço**  
  
-   Qualquer conta de serviço padrão pode ser usada como uma conta de serviço do AD FS. Também há suporte para contas de serviço gerenciado de grupo. Isso exige que pelo menos um controlador de domínio \(é recomendável que você implantar dois ou mais\) que executa o Windows Server 2012 ou posterior.  
  
-   Para autenticação Kerberos funcionar entre o domínio\-Unido clientes e o AD FS, o ' HOST\/< adfs\_service\_nome >' deve ser registrado como um SPN na conta de serviço. Por padrão, o AD FS irá configurar isso ao criar um novo farm do AD FS se ele tiver permissões suficientes para executar esta operação.  
  
-   A conta de serviço do AD FS deve ser confiável em cada domínio de usuário que contém a autenticação dos usuários para o serviço AD FS.  
  
**Requisitos de domínio**  
  
-   Todos os servidores do AD FS devem ser unido a um domínio AD DS.  
  
-   Todos os servidores do AD FS em um farm devem ser implantados em um único domínio.  
  
-   Os servidores do AD FS ingressados no domínio deve confiar que cada domínio de conta de usuário que contém usuários que se autenticam para o serviço AD FS.  
  
**Requisitos de várias florestas**  
  
-   Os servidores do AD FS ingressados no domínio deve confiar em cada domínio da conta de usuário ou a floresta que contém usuários que se autenticam para o serviço AD FS.  
  
-   A conta de serviço do AD FS deve ser confiável em cada domínio de usuário que contém a autenticação dos usuários para o serviço AD FS.  
  
## <a name="BKMK_5"></a>Requisitos de banco de dados de configuração  
A seguir estão os requisitos e restrições que se aplicam, com base no tipo de repositório de configuração:  
  
**WID**  
  
-   Um farm de WID tem um limite de 30 servidores de federação, se você tiver 100 ou menos objetos de confiança.  
  
-   Não há suporte para o perfil de resolução de artefato no SAML 2.0 no banco de dados de configuração do WID.  Não há suporte para a detecção de reprodução de token no banco de dados de configuração do WID. Essa funcionalidade só é usada apenas em cenários em que o AD FS está atuando como o provedor de Federação e consumindo tokens de segurança de provedores de declarações externas.  
  
-   Centros de implantar servidores do AD FS em dados distintos para failover ou balanceamento de carga geográfica tem suporte desde que o número de servidores não exceder 30.  
  
A tabela a seguir fornece um resumo para o uso de um farm de WID.  Usá-lo a planejar sua implementação.  
  
||||  
|-|-|-|  
||1 \- 100 relações de confiança RP|Relações de confiança RP mais de 100|  
|1 \- 30 AD FS nós|Suportada de WID|Não há suportada usando o WID \- SQL é necessário|  
|Mais de 30 AD FS nós|Não há suportada usando o WID \- SQL é necessário|Não há suportada usando o WID \- SQL é necessário|  
  
**SQL Server**  
  
Para o AD FS no Windows Server 2012 R2, você pode usar o SQL Server 2008 e superior  
  
## <a name="BKMK_6"></a>Requisitos de navegador  
Quando a autenticação do AD FS é realizada por meio de um navegador ou um controle de navegador, seu navegador deve estar em conformidade aos seguintes requisitos:  
  
-   JavaScript deve estar habilitado  
  
-   Os cookies devem ser ativados  
  
-   Indicação de nome de servidor \(SNI\) devem ter suporte  
  
-   Para autenticação de certificado de dispositivo de & certificado de usuário \(funcionalidade de junção de local de trabalho\), o navegador deve oferecer suporte a autenticação de certificado de cliente SSL  
  
Várias plataformas e navegadores principais foram submetidos a validação para renderização e a funcionalidade de detalhes que estão listados abaixo. Ainda há suporte para navegadores e dispositivos que não são abordados nesta tabela se eles atendem aos requisitos listados acima:  
  
|||  
|-|-|  
|**Navegadores**|**Plataformas**|  
|IE 10.0|Windows 7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2|  
|IE 11.0|Windows7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2|  
|Agente de autenticação do Windows da Web|Windows 8.1|  
|Firefox \[v21\]|Windows 7, Windows 8.1|  
|Safari \[v7\]|iOS 6, Mac OS\-X 10.7|  
|Chrome \[v27\]|Windows 7, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Mac OS\-X 10.7|  
  
> [!IMPORTANT]  
> Problema conhecido \- Firefox: Funcionalidade de junção de local de trabalho que identifica o dispositivo usando o certificado do dispositivo não está funciona em plataformas Windows. Firefox no momento não oferece suporte para executar autenticação de certificado de cliente SSL usando certificados provisionados para o repositório de certificados de usuário em clientes do Windows.  
  
**Cookies**  
  
AD FS cria sessão\-cookies persistentes e baseados em que devem ser armazenados em computadores cliente para fornecer logon\-, assinar\-, logon único\-na \(SSO\)e outras funcionalidades. Portanto, o navegador do cliente deve ser configurado para aceitar cookies. Cookies são usados para autenticação são sempre Secure Hypertext Transfer Protocol \(HTTPS\) cookies de sessão que são escritos para o servidor de origem. Se o navegador do cliente não estiver configurado para permitir esses cookies, o AD FS não poderá funcionar corretamente. Os cookies persistentes são usados para preservar a seleção de usuário do provedor de declarações. Você pode desabilitá-las por meio de uma definição de configuração no arquivo de configuração para a entrada do AD FS\-nas páginas. Suporte para TLS\/SSL é necessário por motivos de segurança.  
  
## <a name="BKMK_extranet"></a>Requisitos de extranet  
Para fornecer acesso à extranet para o serviço AD FS, você deve implantar o serviço de função Proxy de aplicativo Web como a função voltado para a extranet que solicitações de autenticação de proxies de maneira segura para o serviço AD FS. Isso fornece isolamento dos pontos de extremidade de serviço do AD FS, bem como o isolamento de todas as chaves de segurança \(, como certificados de assinatura de token\) de solicitações que se originam da internet. Além disso, recursos, como bloqueio de conta de Extranet reversível exigem o uso do Proxy de aplicativo Web. Para obter mais informações sobre o Proxy de aplicativo Web, consulte [Proxy de aplicativo Web](https://technet.microsoft.com/library/dn584107.aspx).  
  
Se você quiser usar um terceiro\-proxy para acesso à extranet, de terceiros neste terceiro\-proxy de terceiros deve dar suporte o protocolo definido nas [http:\/\/download.microsoft.com\/baixar\/9\/5\/eletrônico\/95EF66AF\-9026\-4BB0\-A41D\-A4F81802D92C\/% 5bMS\-ADFSPIP%5d.pdf](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf).  
  
## <a name="BKMK_7"></a>Requisitos de rede  
Configurar os serviços de rede a seguir apropriadamente é essencial para uma implantação bem-sucedida do AD FS em sua organização:  
  
**Configurando o Firewall corporativo**  
  
Firewall localizado entre o Proxy de aplicativo Web e o farm de servidores de Federação e o firewall entre os clientes e o Proxy de aplicativo Web deve ter a porta TCP 443 habilitado entrada.  
  
Além disso, se a autenticação de certificado de usuário do cliente \(autenticação do clientTLS usando X509 certificados de usuário\) é necessária, o AD FS no Windows Server 2012 R2 requer que a porta TCP 49443 seja habilitado entrada no firewall entre o os clientes e o Proxy de aplicativo Web. Isso não é necessário no firewall entre o Proxy de aplicativo Web e os servidores de Federação\).  
  
**Configurando o DNS**  
  
-   Para acesso à intranet, todos os clientes que acessam o AD FS do serviço dentro da rede corporativa interna \(intranet\) deve ser capaz de resolver o nome do serviço do AD FS \(o nome fornecido pelo certificado de SSL\) para carregar Balanceador de servidores do AD FS ou do servidor do AD FS.  
  
-   Para obter acesso à extranet, todos os clientes que acessam o AD FS do serviço de fora da rede corporativa \(extranet\/internet\) deve ser capaz de resolver o nome do serviço do AD FS \(nome fornecido pelo certificado de SSL\) ao balanceador de carga para os servidores de Proxy de aplicativo Web ou servidor de Proxy de aplicativo Web.  
  
-   Para obter acesso à extranet funcionar corretamente, cada servidor de Proxy de aplicativo Web na rede de Perímetro deve ser capaz de resolver o nome de serviço do AD FS \(o nome fornecido pelo certificado de SSL\) ao balanceador de carga para servidores do AD FS ou servidor do AD FS. Isso pode ser feito usando um servidor DNS alternativo na rede DMZ ou alterando a resolução de servidor local usando o arquivo de HOSTS.  
  
-   Para a autenticação integrada do Windows trabalhar dentro da rede e fora da rede para um subconjunto de pontos de extremidade expostos por meio do Proxy de aplicativo Web, você deve usar um registro \(CNAME não\) para apontar para os balanceadores de carga.  
  
Para obter informações sobre como configurar DNS corporativo para o serviço de Federação e Device Registration Service, consulte [configurar DNS corporativo para o serviço de Federação e o DRS](https://technet.microsoft.com/library/dn486786.aspx).  
  
Para obter informações sobre como configurar DNS corporativo para proxies de aplicativo Web, consulte a seção "Configurar o DNS" [etapa 1: Configurar a infraestrutura de Proxy de aplicativo Web](https://technet.microsoft.com/library/dn383644.aspx).  
  
Para obter informações sobre como configurar um endereço IP ou FQDN de cluster usando NLB, consulte especificando os parâmetros de Cluster em [http:\/\/go.microsoft.com\/fwlink\/? LinkId\=75282](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
## <a name="BKMK_8"></a>Requisitos de repositório de atributos  
O AD FS requer pelo menos um repositório de atributos a ser usado para autenticar usuários e extrair declarações de segurança para esses usuários. Para obter uma lista do atributo repositórios que dá suporte ao AD FS, consulte [função The dos repositórios de atributos](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
> [!NOTE]  
> O AD FS cria automaticamente um repositório de atributos de "Active Directory", por padrão. Requisitos de repositório de atributos dependem se a sua organização está agindo como parceiro de conta \(hospedando os usuários federados\) ou o parceiro de recurso \(hospedando o aplicativo federado\).  
  
**Repositórios de atributos LDAP**  
  
Quando você trabalha com outros Lightweight Directory Access Protocol \(LDAP\)\-com base em repositórios de atributos, você deve se conectar a um servidor LDAP que oferece suporte à autenticação integrada do Windows. A cadeia de conexão LDAP também deve ser escrita no formato de uma URL LDAP, como descrito no RFC 2255.  
  
Também é necessário que a conta de serviço para o serviço AD FS tem o direito de recuperar informações do usuário no repositório de atributos do LDAP.  
  
**Repositórios de atributos do SQL Server**  
  
Para o AD FS no Windows Server 2012 R2 para operar com êxito, computadores que hospedam o repositório de atributos do SQL Server devem estar executando o Microsoft SQL Server 2008 ou superior. Quando você trabalha com o SQL\-com base em repositórios de atributos, você também deve configurar uma cadeia de caracteres de conexão.  
  
**Repositórios de atributos personalizados**  
  
Você pode desenvolver repositórios de atributos personalizados para habilitar cenários avançados.  
  
-   A linguagem da política interna do AD FS pode referenciar repositórios de atributos personalizados para que qualquer um dos seguintes cenários seja aprimorado:  
  
    -   Criar declaração para um usuário autenticado localmente  
  
    -   Suplementar declarações para um usuário autenticado externamente  
  
    -   Autorizar um usuário a obter um token  
  
    -   Autorizar um serviço a obter um token sobre o comportamento de um usuário  
  
    -   Emitindo dados adicionais em tokens de segurança emitidos pelo AD FS para terceiras partes confiáveis.  
  
-   Todos os repositórios de atributos personalizados devem ser compilados sobre o .NET 4.0 ou superior.  
  
Quando você trabalha com um repositório de atributos personalizados, você também terá que configurar uma cadeia de caracteres de conexão. Nesse caso, você pode inserir um código personalizado de sua escolha que permite que uma conexão para seu repositório de atributos personalizados. A cadeia de caracteres de conexão nessa situação é um conjunto de nome\/valor pares que são interpretados como implementados pelo desenvolvedor do repositório de atributos personalizados. Para obter mais informações sobre como desenvolver e usar repositórios de atributos personalizados, consulte [visão geral do Store atributos](https://go.microsoft.com/fwlink/?LinkId=190782).  
  
## <a name="BKMK_9"></a>Requisitos do aplicativo  
O AD FS dá suporte a declarações\-com suporte a aplicativos que usam os seguintes protocolos:  
  
-   WS\-Federation  
  
-   WS\-confiar  
  
-   Protocolo SAML 2.0 usando perfis de IDPLite, SPLite & eGov1.5.  
  
-   Perfil de concessão de autorização do OAuth 2.0  
  
O AD FS também dá suporte à autenticação e autorização para qualquer não\-declarações\-aplicativos compatíveis que são compatíveis com o Proxy de aplicativo Web.  
  
## <a name="BKMK_10"></a>Requisitos de autenticação  
**A autenticação do AD DS \(autenticação primária\)**  
  
Para acesso à intranet, há suporte para os seguintes mecanismos de autenticação padrão para o AD DS:  
  
-   Autenticação integrada do Windows usando Negotiate para Kerberos e NTLM  
  
-   Usando o nome de usuário de autenticação de formulários\/senhas  
  
-   Autenticação de certificado usando certificados mapeados para contas de usuário no AD DS  
  
Para obter acesso à extranet, há suporte para os seguintes mecanismos de autenticação:  
  
-   Usando o nome de usuário de autenticação de formulários\/senhas  
  
-   Autenticação de certificado usando certificados que são mapeados para contas de usuário no AD DS  
  
-   Autenticação integrada do Windows usando Negotiate \(somente NTLM\) para WS\-confiar em pontos de extremidade que aceitam autenticação integrada do Windows.  
  
Para autenticação de certificado:  
  
-   Se estende para cartões inteligentes que podem ser protegido de pin.  
  
-   A GUI para o usuário insira seu pin não é fornecida pelo AD FS e é necessário para fazer parte do sistema operacional cliente que é exibido ao usar o cliente TLS.  
  
-   O leitor e o provedor de serviços criptográficos \(CSP\) para o cartão inteligente devem funcionar no computador em que o navegador está localizado.  
  
-   O certificado de cartão inteligente deve se liga a uma raiz confiável em todos os servidores do AD FS e servidores de Proxy de aplicativo Web.  
  
-   O certificado deve mapear para a conta de usuário no AD DS por qualquer um dos seguintes métodos:  
  
    -   O nome de assunto do certificado corresponde ao nome distinto LDAP de uma conta de usuário no AD DS.  
  
    -   Extensão de nome alternativa da entidade de certificado tem o nome UPN \(UPN\) de uma conta de usuário no AD DS.  
  
Para autenticação integrada do Windows contínuo usando o Kerberos na intranet,  
  
-   Ele é necessário para o nome do serviço ser parte de Sites confiáveis ou sites da Intranet Local.  
  
-   Além disso, o HOST\/< adfs\_service\_nome > SPN deve ser definida na conta de serviço que executa o farm do AD FS.  
  
**Várias\-fatores de autenticação**  
  
O AD FS dá suporte à autenticação adicional \(além da autenticação primária, com suporte pelo AD DS\) usando um modelo de provedor no qual fornecedores\/os clientes podem criar seu próprios multi\-adaptador da autenticação multifator Se um administrador pode registrar e usar durante o logon.  
  
Cada adaptador MFA deve ser criada sobre o .NET 4.5.  
  
Para obter mais informações sobre a MFA, consulte [gerenciar riscos com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  
  
**Autenticação de dispositivo**  
  
O AD FS dá suporte à autenticação de dispositivo usando certificados provisionados pelo serviço de registro de dispositivo durante o ato de um espaço de trabalho do usuário final ingressando em seu dispositivo.  
  
## <a name="BKMK_11"></a>Requisitos de junção de local de trabalho  
Os usuários finais podem ingresso no local de seus dispositivos para uma organização usando o AD FS. Isso é suportado pelo serviço de registro do dispositivo no AD FS. Como resultado, os usuários finais obtêm o benefício adicional de SSO entre os aplicativos com suporte pelo AD FS. Além disso, os administradores podem gerenciar o risco restringindo o acesso a aplicativos somente para dispositivos que tenham sido ingresso para a organização. Abaixo estão os seguintes requisitos para habilitar esse cenário.  
  
-   O AD FS dá suporte ao ingresso no local para Windows 8.1 e iOS 5\+ dispositivos  
  
-   Para usar a funcionalidade de ingresso no local, o esquema da floresta que ingressaram em servidores do AD FS deve ser Windows Server 2012 R2.  
  
-   Nome alternativo da entidade do certificado SSL para o serviço do AD FS deve conter o enterpriseregistration de valor é seguido pelo nome da entidade de usuário \(UPN\) sufixo da sua organização, por exemplo, enterpriseregistration.corp.contoso.com.  
  
## <a name="BKMK_12"></a>Requisitos de criptografia  
A tabela a seguir fornece informações de suporte de criptografia adicional sobre o token do AD FS de assinatura, criptografia de token\/funcionalidade de descriptografia:  
  
||||  
|-|-|-|  
|**algoritmo**|**Comprimentos de chave**|**Protocolos\/aplicativos\/comentários**|  
|TripleDES – padrão 192 \(suporte 192 a 256\) \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\# TripleDES\-cbc](http://www.w3.org/2001/04/xmlenc)|>\= 192|Algoritmo com suporte para descriptografar o token de segurança. Não há suporte para criptografar o token de segurança com este algoritmo.|  
|Aes128 \- http:\/\/www.w3.org\/2001\/04\/xmlenc\#aes128\-cbc|128|Algoritmo com suporte para descriptografar o token de segurança. Não há suporte para criptografar o token de segurança com este algoritmo.|  
|AES192 \- http:\/\/www.w3.org\/2001\/04\/xmlenc\#aes192\-cbc|192|Algoritmo com suporte para descriptografar o token de segurança. Não há suporte para criptografar o token de segurança com este algoritmo.|  
|AES256 \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#aes256\-cbc](http://www.w3.org/2001/04/xmlenc)|256|**Padrão**. Algoritmo com suporte para criptografar o token de segurança.|  
|TripleDESKeyWrap \- http:\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-tripledes|Todos os tamanhos de chave com suporte pelo .NET 4.0\+|Suporte para o algoritmo para criptografar a chave simétrica criptografada por um token de segurança.|  
|AES128KeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-aes128](http://www.w3.org/2001/04/xmlenc)|128|Suporte para o algoritmo para criptografar a chave simétrica que criptografa o token de segurança.|  
|AES192KeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-aes192](http://www.w3.org/2001/04/xmlenc)|192|Suporte para o algoritmo para criptografar a chave simétrica que criptografa o token de segurança.|  
|AES256KeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-aes256](http://www.w3.org/2001/04/xmlenc)|256|Suporte para o algoritmo para criptografar a chave simétrica que criptografa o token de segurança.|  
|RsaV15KeyWrap \- http:\/\/www.w3.org\/2001\/04\/xmlenc\#rsa\-1\_5|1024|Suporte para o algoritmo para criptografar a chave simétrica que criptografa o token de segurança.|  
|RsaOaepKeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#rsa\-oaep\-mgf1p](http://www.w3.org/2001/04/xmlenc)|1024|Padrão. Suporte para o algoritmo para criptografar a chave simétrica que criptografa o token de segurança.|  
|SHA1\-http:\/\/www.w3.org\/PICS\/DSig\/SHA1\_1\_0.html|N\/A|Usado pelo servidor do AD FS na geração de SourceId artefato:  Nesse cenário, o STS usa SHA1 \(pela recomendação no padrão SAML 2.0\) para criar um valor curto de 160 bits para o sourceiD de artefato.<br /><br />Também é usado pelo agente de web do ADFS \(componente herdado do período de tempo WS2003\) para identificar alterações em um tempo de "última atualização" para que ele saiba quando atualizar informações do STS de valor.|  
|SHA1withRSA\-<br /><br />http:\/\/www.w3.org\/PICS\/DSig\/RSA\-SHA1\_1\_0.html|N\/A|Usado em casos quando o servidor do AD FS valida a assinatura de SAML AuthenticationRequest, assinar a resposta ou solicitação de resolução de artefato, crie um token\-certificado de assinatura.<br /><br />Nesses casos, o SHA256 é o padrão e SHA1 é usado apenas se o parceiro \(terceira\) não é possível oferecer suporte a SHA256 e deve usar SHA1.|  
  
## <a name="BKMK_13"></a>Requisitos de permissões  
O administrador que executa a instalação e a configuração inicial do AD FS deve ter permissões de administrador de domínio no domínio local \(em outras palavras, o domínio ao qual o servidor de Federação ingressou em.\)  
  
## <a name="see-also"></a>Consulte também  
[Guia de design do AD FS no Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

