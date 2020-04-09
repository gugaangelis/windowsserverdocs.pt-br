---
ms.assetid: 8ce6e7c4-cf8e-4b55-980c-048fea28d50f
title: Farm de servidores de federação usando SQL Server
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9f8375ffb73ff6be290b534d59e6ce5c8a7be27b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858059"
---
# <a name="ad-fs-requirements"></a>Requisitos do AD FS

A seguir estão os vários requisitos que você deve obedecer ao implantar AD FS:  
  
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
  
-   [Requisitos de ingresso no local de trabalho](AD-FS-Requirements.md#BKMK_11)  
  
-   [Requisitos de criptografia](AD-FS-Requirements.md#BKMK_12)  
  
-   [Requisitos de permissões](AD-FS-Requirements.md#BKMK_13)  
  
## <a name="certificate-requirements"></a><a name="BKMK_1"></a>Requisitos de certificado  
Os certificados desempenham a função mais crítica na proteção de comunicações entre servidores de Federação, proxies de aplicativos Web, declarações\-aplicativos com reconhecimento e clientes Web. Os requisitos de certificados variam, dependendo se você estiver configurando um servidor de Federação ou um computador proxy, conforme descrito nesta seção.  
  
**Certificados do servidor de Federação**  
  
|||  
|-|-|  
|**Tipo de certificado**|**Requisitos, suporte & coisas a saber**|  
|**Protocolo SSL \(SSL\) certificado:** Esse é um certificado SSL padrão que é usado para proteger as comunicações entre servidores de Federação e clientes.|-Este certificado deve ser um certificado\* X509 v3 publicamente confiável.<br />-Todos os clientes que acessam qualquer ponto de extremidade de AD FS devem confiar nesse certificado. É altamente recomendável usar certificados emitidos por um \(público\-terceiros\) autoridade de certificação \(\)de certificação. Você pode usar um certificado SSL assinado por auto\-com êxito em servidores de Federação em um ambiente de laboratório de teste. No entanto, para um ambiente de produção, recomendamos que você obtenha o certificado de uma CA pública.<br />– Dá suporte a qualquer tamanho de chave com suporte do Windows Server 2012 R2 para certificados SSL.<br />-Não oferece suporte a certificados que usam chaves CNG.<br />-Quando usado junto com Workplace Join\/serviço de registro de dispositivo, o nome alternativo da entidade do certificado SSL para o serviço de AD FS deve conter o valor enterpriseregistration que é seguido pelo nome principal do usuário \(o sufixo de\) UPN da sua organização, por exemplo, enterpriseregistration.contoso.com.<br />-Há suporte para certificados curinga. Ao criar seu farm de AD FS, você será solicitado a fornecer o nome do serviço para o serviço de AD FS \(por exemplo, **ADFS.contoso.com**.<br />-É altamente recomendável usar o mesmo certificado SSL para o proxy de aplicativo Web. No entanto, isso é **necessário** para ser o mesmo ao oferecer suporte a pontos de extremidade de autenticação integrada do Windows por meio do proxy de aplicativo Web e quando a autenticação de proteção estendida está ativada \(configuração padrão\).<br />-O nome do assunto desse certificado é usado para representar o nome do Serviço de Federação para cada instância do AD FS que você implanta. Por esse motivo, talvez você queira considerar a escolha de um nome de entidade em qualquer nova AC\-certificados emitidos que melhor representem o nome da sua empresa ou organização aos parceiros.<br />    A identidade do certificado deve corresponder ao nome do serviço de Federação \(por exemplo, fs.contoso.com\). A identidade é uma extensão de nome alternativo da entidade do tipo dNSName ou, se não houver entradas de nome alternativo da entidade, o nome da entidade especificado como um nome comum. Várias entradas de nome alternativo da entidade podem estar presentes no certificado, desde que uma delas corresponda ao nome do serviço de Federação.<br />-   **importante:** é altamente recomendável usar o mesmo certificado SSL em todos os nós do seu farm de AD FS, bem como todos os proxies de aplicativo Web em seu farm de AD FS.|  
|**Certificado de comunicação do serviço:** Esse certificado habilita a segurança de mensagens do WCF para proteger as comunicações entre os servidores de Federação.|-Por padrão, o certificado SSL é usado como o certificado de comunicações do serviço.  Mas você também tem a opção de configurar outro certificado como o certificado de comunicação do serviço.<br />-   **importante:** se você estiver usando o certificado SSL como o certificado de comunicação do serviço, quando o certificado SSL expirar, certifique-se de configurar o certificado SSL renovado como seu certificado de comunicação do serviço. Isso não ocorre automaticamente.<br />-Esse certificado deve ser confiável para clientes de AD FS que usam a segurança de mensagem do WCF.<br />-Recomendamos que você use um certificado de autenticação de servidor emitido por um \(público\-terceiros\) autoridade de certificação \(\)de certificação.<br />-O certificado de comunicação do serviço não pode ser um certificado que usa chaves CNG.<br />-Esse certificado pode ser gerenciado usando o console de gerenciamento do AD FS.|  
|**Certificado de assinatura de\-de token:** Esse é um certificado X509 padrão que é usado para assinar com segurança todos os tokens que o servidor de Federação emite.|-Por padrão, AD FS cria um certificado auto\-assinado com chaves de 2048 bits.<br />-Certificados emitidos por AC também têm suporte e podem ser alterados usando o snap\-de gerenciamento de AD FS no<br />-Certificados emitidos por AC devem ser armazenados & acessados por meio de um provedor de criptografia CSP.<br />-O certificado de autenticação de token não pode ser um certificado que usa chaves CNG.<br />-AD FS não requer certificados registrados externamente para autenticação de token.<br />    AD FS renova automaticamente esses certificados auto\-assinados antes que eles expirem, primeiro Configurando os novos certificados como certificados secundários para permitir que os parceiros os consumam e, em seguida, invertendo para primário em um processo chamado substituição automática de certificado. Recomendamos que você use os certificados padrão gerados automaticamente para autenticação de token.<br />    Se sua organização tiver políticas que exigem certificados diferentes a serem configurados para autenticação de token, você poderá especificar os certificados no momento da instalação usando o PowerShell \(usar o parâmetro – SigningCertificateThumbprint do cmdlet Install\-AdfsFarm\).  Após a instalação, você pode exibir e gerenciar certificados de assinatura de token usando o console de gerenciamento do AD FS ou cmdlets do PowerShell definidos\-AdfsCertificate e obter\-AdfsCertificate.<br />    Quando certificados registrados externamente são usados para autenticação de token, AD FS não executa a renovação ou substituição automática de certificado.  Esse processo deve ser executado por um administrador.<br />    Para permitir a substituição de certificado quando um certificado está perto de expirar, um certificado de assinatura de token secundário pode ser configurado em AD FS. Por padrão, todos os certificados de assinatura de token são publicados em metadados de Federação, mas somente o token primário\-certificado de autenticação é usado pelo AD FS para assinar tokens.|  
|**Token\-descriptografia\/certificado de criptografia:** Esse é um certificado X509 padrão que é usado para descriptografar\/criptografar quaisquer tokens de entrada. Ele também é publicado nos metadados de federação.|-Por padrão, AD FS cria um certificado auto\-assinado com chaves de 2048 bits.<br />-Certificados emitidos por AC também têm suporte e podem ser alterados usando o snap\-de gerenciamento de AD FS no<br />-Certificados emitidos por AC devem ser armazenados & acessados por meio de um provedor de criptografia CSP.<br />-O token\-descriptografia\/certificado de criptografia não pode ser um certificado que usa chaves CNG.<br />-Por padrão, o AD FS gera e usa seus próprios certificados internos, gerados internamente e auto\-assinados para descriptografia de token.  AD FS não requer certificados registrados externamente para essa finalidade.<br />    Além disso, AD FS renova automaticamente esses certificados auto\-assinados antes que eles expirem.<br />    **Recomendamos que você use os certificados padrão gerados automaticamente para a descriptografia de token.**<br />    Se sua organização tiver políticas que exigem certificados diferentes a serem configurados para descriptografia de token, você poderá especificar os certificados no momento da instalação usando o PowerShell \(usar o parâmetro – DecryptionCertificateThumbprint do cmdlet Install\-AdfsFarm\).  Após a instalação, você pode exibir e gerenciar certificados de descriptografia de token usando o console de gerenciamento do AD FS ou cmdlets do PowerShell definidos\-AdfsCertificate e obter\-AdfsCertificate.<br />    **Quando certificados registrados externamente são usados para descriptografia de token, AD FS não executa a renovação automática de certificado.  Esse processo deve ser executado por um administrador**.<br />-A conta de serviço de AD FS deve ter acesso ao token\-chave privada do certificado de assinatura no repositório pessoal do computador local. Isso é resolvido pela instalação. Você também pode usar o\-snap de gerenciamento de AD FS no para garantir esse acesso se você alterar posteriormente o token\-certificado de autenticação.|  
  
> [!CAUTION]  
> Os certificados usados para a autenticação e criptografia\-descriptografia de tokens são fundamentais para a estabilidade do Serviço de Federação. Os clientes que gerenciam os próprios certificados de autenticação e criptografia\-descriptografia de tokens devem verificar se o backup desses certificados foi feito e se eles estão disponíveis de maneira independentemente durante um evento de recuperação.  
  
> [!NOTE]  
> No AD FS você pode alterar o algoritmo de hash seguro \(nível de\) do SHA que é usado para assinaturas digitais para o SHA\-1 ou SHA\-256 \(mais seguro\). AD FS não dá suporte ao uso de certificados com outros métodos de hash, como MD5 \(algoritmo de hash padrão usado com a ferramenta de linha de\-comando MakeCert. exe\). Como prática recomendada de segurança, recomendamos que você use o SHA\-256 \(que é definido por padrão\) para todas as assinaturas. O SHA\-1 é recomendado para uso somente em cenários nos quais você deve interoperar com um produto que não oferece suporte a comunicações usando o SHA\-256, como um produto da Microsoft não\-ou versões herdadas do AD FS.  
  
> [!NOTE]  
> Depois de receber um certificado de uma AC, certifique-se de que todos os certificados sejam importados para o repositório de certificados pessoais do computador local. Você pode importar certificados para o repositório pessoal com o snap\-do MMC de certificados no.  
  
## <a name="hardware-requirements"></a><a name="BKMK_2"></a>Requisitos de hardware  
Os requisitos de hardware mínimos e recomendados a seguir se aplicam ao AD FS servidores de Federação no Windows Server 2012 R2:  
  
||||  
|-|-|-|  
|**Requisito de hardware**|**Requisito mínimo**|**Requisito recomendado**|  
|Velocidade de CPU|processador de 1,4 GHz 64\-bits|Quad\-Core, 2 GHz|  
|RAM|512 MB|4 GB|  
|Espaço em disco|32 GB|100 GB|  
  
## <a name="software-requirements"></a><a name="BKMK_3"></a>Requisitos de software  
Os requisitos de AD FS a seguir são para a funcionalidade de servidor que é criada no sistema operacional Windows Server&reg; 2012 R2:  
  
-   Para acesso à extranet, você deve implantar o serviço de função do proxy de aplicativo Web \- parte da função de servidor de acesso remoto do Windows Server&reg; 2012 R2. Não há suporte para versões anteriores de um proxy de servidor de Federação com AD FS no Windows Server&reg; 2012 R2.  
  
-   Um servidor de federação e o serviço de função do Proxy de Aplicativo Web não podem ser instalados no mesmo computador.  
  
## <a name="ad-ds-requirements"></a><a name="BKMK_4"></a>Requisitos do AD DS  
**Requisitos de controlador de domínio**  
  
Os controladores de domínio em todos os domínios de usuário e o domínio ao qual os servidores de AD FS são associados devem estar executando o Windows Server 2008 ou posterior.  
  
> [!NOTE]  
> Todo o suporte para ambientes com controladores de domínio do Windows Server 2003 será encerrado após a data de término do suporte estendido para o Windows Server 2003. Os clientes são altamente recomendáveis para atualizar seus controladores de domínio assim que possível. Visite [esta página](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) para obter informações adicionais sobre suporte da Microsoft ciclo de vida. Para problemas descobertos que são específicos para os ambientes do controlador de domínio do Windows Server 2003, as correções serão emitidas apenas para problemas de segurança e se uma correção puder ser emitida antes da expiração do suporte estendido para o Windows Server 2003.  



>[!NOTE]
> AD FS requer um controlador de domínio gravável completo para funcionar em oposição a um controlador de domínio somente leitura. Se uma topologia planejada incluir um controlador de domínio somente leitura, o controlador de domínio somente leitura poderá ser usado para autenticação, mas o processamento de declarações LDAP exigirá uma conexão com o controlador de domínio gravável.
  
**Requisitos de nível\-funcional do domínio**  
  
Todos os domínios de conta de usuário e o domínio no qual os servidores AD FS são ingressados devem estar operando no nível funcional do domínio do Windows Server 2003 ou posterior.  
  
A maioria dos recursos de AD FS não exigem modificações AD DS funcionais de nível de\-para operar com êxito. No entanto, o nível funcional do domínio do Windows Server 2008 ou superior é exigido para que a autenticação de certificados de clientes funcione bem, caso o certificado esteja associado explicitamente à conta de usuário do AD DS.  
  
**Requisitos de esquema**  
  
-   AD FS não requer alterações de esquema ou modificações de nível de\-funcional para AD DS.  
  
-   Para usar Workplace Join funcionalidade, o esquema da floresta para o qual AD FS servidores estão associados deve ser definido como Windows Server 2012 R2.  
  
**Requisitos de conta de serviço**  
  
-   Qualquer conta de serviço padrão pode ser usada como uma conta de serviço para AD FS. Também há suporte para contas do Serviço Gerenciado de Grupo. Isso requer pelo menos um controlador de domínio \(é recomendável que você implante dois ou mais\) que esteja executando o Windows Server 2012 ou superior.  
  
-   Para que a autenticação Kerberos funcione entre os clientes ingressados no domínio\-e o AD FS, o serviço de\_de HOST\/< ADFS\_nome > ' deve ser registrado como um SPN na conta de serviço. Por padrão, AD FS configurará isso ao criar um novo farm de AD FS se ele tiver permissões suficientes para executar essa operação.  
  
-   A conta de serviço de AD FS deve ser confiável em cada domínio de usuário que contenha usuários que se autenticam no serviço AD FS.  
  
**Requisitos de Domínio**  
  
-   Todos os servidores AD FS devem ser ingressados em um domínio do AD DS.  
  
-   Todos os servidores de AD FS em um farm devem ser implantados em um único domínio.  
  
-   O domínio ao qual os servidores AD FS estão associados deve confiar em cada domínio de conta de usuário que contenha usuários que se autenticam no serviço AD FS.  
  
**Requisitos de várias florestas**  
  
-   O domínio ao qual os servidores AD FS estão associados deve confiar em cada domínio de conta de usuário ou floresta que contenha usuários que se autenticam no serviço de AD FS.  
  
-   A conta de serviço de AD FS deve ser confiável em cada domínio de usuário que contenha usuários que se autenticam no serviço AD FS.  
  
## <a name="configuration-database-requirements"></a><a name="BKMK_5"></a>Requisitos de banco de dados de configuração  
A seguir estão os requisitos e as restrições que se aplicam com base no tipo de repositório de configuração:  
  
**WID**  
  
-   Um farm WID tem um limite de 30 servidores de Federação se você tiver 100 ou menos confianças de terceira parte confiável.  
  
-   O perfil de resolução de artefatos no SAML 2,0 não tem suporte no banco de dados de configuração WID.  A detecção de reprodução de token não tem suporte no banco de dados de configuração de WID. Essa funcionalidade só é usada em cenários em que AD FS está agindo como o provedor de Federação e consumindo tokens de segurança de provedores de declarações externos.  
  
-   Há suporte para a implantação de servidores AD FS em data centers distintos para failover ou balanceamento de carga geográfico, desde que o número de servidores não exceda 30.  
  
A tabela a seguir fornece um resumo para usar um farm WID.  Use-o para planejar sua implementação.  
  
||||  
|-|-|-|  
||1 \- de confianças do RP 100|Mais de 100 RP relações de confiança|  
|1 \- 30 nós AD FS|Suporte ao WID|Sem suporte usando o WID \- SQL necessário|  
|Mais de 30 nós AD FS|Sem suporte usando o WID \- SQL necessário|Sem suporte usando o WID \- SQL necessário|  
  
**SQL Server**  
  
Para AD FS no Windows Server 2012 R2, você pode usar SQL Server 2008 e superior  
  
## <a name="browser-requirements"></a><a name="BKMK_6"></a>Requisitos de navegador  
Quando a autenticação do AD FS é executada por meio de um navegador ou controle de navegador, seu navegador deve atender aos seguintes requisitos:  
  
-   O JavaScript deve estar habilitado  
  
-   Os cookies devem estar ativados  
  
-   Deve haver suporte para a SNI \(Indicação de Nome de Servidor\)  
  
-   Para certificado de usuário & autenticação de certificado de dispositivo \(funcionalidade de ingresso no local de trabalho\), o navegador deve dar suporte à autenticação de certificado de cliente SSL  
  
Vários navegadores e plataformas principais passaram por validação para a renderização e a funcionalidade dos detalhes listados abaixo. Os navegadores e dispositivos que não são abordados nesta tabela ainda terão suporte se atenderem aos requisitos listados acima:  
  
|||  
|-|-|  
|**Navigator**|**Compatíveis**|  
|IE 10,0|Windows 7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2|  
|IE 11,0|Windows7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2|  
|Agente de autenticação da Web do Windows|Windows 8.1|  
|Firefox \[v21\]|Windows 7, Windows 8.1|  
|Safari \[v7\]|iOS 6, Mac OS\-X 10,7|  
|Chrome \[V27\]|Windows 7, Windows 8.1, Windows Server 2012, Windows Server 2012 R2 Mac OS\-X 10,7|  
  
> [!IMPORTANT]  
> Problema conhecido \- Firefox: a funcionalidade Workplace Join que identifica o dispositivo usando o certificado de dispositivo não é funcional em plataformas Windows. No momento, o Firefox não dá suporte à execução de autenticação de certificado de cliente SSL usando certificados provisionados para o repositório de certificados do usuário em clientes Windows.  
  
**Arar**  
  
AD FS cria\-de sessão com cookies persistentes e com base em sessões que devem ser armazenados em computadores cliente para fornecer\-de entrada, entrada\-saída,\-de logon único \(SSO\)e outras funcionalidades. Portanto, o navegador do cliente deve estar configurado para aceitar cookies. Cookies que são usados para autenticação são sempre seguros protocolo de transferência de hipertexto \(cookies de sessão de\) HTTPS que são gravados para o servidor de origem. Se o navegador do cliente não estiver configurado para permitir esses cookies, o AD FS não poderá funcionar corretamente. Os cookies persistentes são usados para preservar a seleção de usuário do provedor de declarações. Você pode desabilitá-los usando uma definição de configuração no arquivo de configuração para o\-de AD FS de entrada em páginas. O suporte para TLS\/SSL é necessário por motivos de segurança.  
  
## <a name="extranet-requirements"></a><a name="BKMK_extranet"></a>Requisitos de extranet  
Para fornecer acesso à extranet para o serviço de AD FS, você deve implantar o serviço de função do proxy de aplicativo Web como a função de extranet que os proxies solicitam de autenticação de forma segura para o serviço AD FS. Isso fornece isolamento dos pontos de extremidade de serviço AD FS, bem como isolamento de todas as chaves de segurança \(como certificados de assinatura de token\) de solicitações originadas pela Internet. Além disso, recursos como o bloqueio de conta de extranet reversível exigem o uso do proxy de aplicativo Web. Para obter mais informações sobre o proxy de aplicativo Web, consulte [proxy de aplicativo Web](https://technet.microsoft.com/library/dn584107.aspx).  
  
Se você quiser usar um terceiro proxy de\-de terceiros para acesso à extranet, esse terceiro proxy de\-deverá dar suporte ao protocolo definido em [http:\/\/download.microsoft.com\/download\/9\/5\/E\/95EF66AF\-9026\-4BB0\-A41D\-A4F81802D92C\/% 5bMS\-ADFSPIP %5 d. pdf](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf).  
  
## <a name="network-requirements"></a><a name="BKMK_7"></a>Requisitos de rede  
Configurar os seguintes serviços de rede adequadamente é essencial para uma implantação bem-sucedida de AD FS em sua organização:  
  
**Configurando o firewall corporativo**  
  
O firewall localizado entre o Proxy de Aplicativo Web e o farm de servidores de federação e o firewall entre os clientes e o Proxy de Aplicativo Web deve ter a porta TCP 443 habilitada para entrada.  
  
Além disso, se a autenticação de certificado de usuário do cliente \(autenticação clientTLS usando certificados de usuário X509\) for necessária, AD FS no Windows Server 2012 R2 exigirá que a porta TCP 49443 seja habilitada para entrada no firewall entre os clientes e o proxy de aplicativo Web. Isso não é necessário no firewall entre o Proxy de Aplicativo Web e os servidores de federação\).  

> [!NOTE]
> também garantir que a porta 49443 não seja usada por nenhum outro serviço no AD FS e no servidor de proxy de aplicativo Web.

**Configuração do DNS**  
  
-   Para acesso à intranet, todos os clientes que acessam AD FS serviço na rede corporativa interna \(intranet\) devem ser capazes de resolver o nome do serviço de AD FS \(nome fornecido pelo certificado SSL\) para o balanceador de carga para os servidores AD FS ou o servidor AD FS.  
  
-   Para acesso à extranet, todos os clientes que acessam AD FS serviço de fora da rede corporativa \(extranet\/Internet\) devem ser capazes de resolver o nome do serviço AD FS \(nome fornecido pelo certificado SSL\) para o balanceador de carga para os servidores de proxy de aplicativo Web ou o servidor proxy de aplicativo Web.  
  
-   Para que o acesso à extranet funcione corretamente, cada servidor proxy de aplicativo Web na DMZ deve ser capaz de resolver AD FS nome do serviço \(nome fornecido pelo certificado SSL\) ao balanceador de carga para os servidores AD FS ou o servidor AD FS. Isso pode ser feito usando um servidor DNS alternativo na rede DMZ ou alterando a resolução do servidor local usando o arquivo de HOSTs.  
  
-   Para que a autenticação integrada do Windows funcione dentro da rede e fora da rede para um subconjunto de pontos de extremidade expostos por meio do proxy de aplicativo Web, você deve usar um registro A \(não\) CNAME para apontar para os balanceadores de carga.  
  
Para obter informações sobre como configurar o DNS corporativo para o serviço de Federação e o serviço de registro de dispositivo, consulte [Configurar o DNS corporativo para o serviço de Federação e o DRS](https://technet.microsoft.com/library/dn486786.aspx).  
  
Para obter informações sobre como configurar o DNS corporativo para proxies de aplicativo Web, consulte a seção "configurar DNS" na [etapa 1: configurar a infraestrutura de proxy de aplicativo Web](https://technet.microsoft.com/library/dn383644.aspx).  
  
Para obter informações sobre como configurar um endereço IP de cluster ou FQDN de cluster usando o NLB, consulte especificando os parâmetros de cluster em [http:\/\/go.microsoft.com\/fwlink\/? LinkId\=75282](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
## <a name="attribute-store-requirements"></a><a name="BKMK_8"></a>Requisitos de repositório de atributos  
AD FS exige que pelo menos um repositório de atributos seja usado para autenticar usuários e extrair declarações de segurança para esses usuários. Para obter uma lista de repositórios de atributos aos quais AD FS dá suporte, consulte [a função de repositórios de atributos](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
> [!NOTE]  
> O AD FS cria automaticamente um repositório de atributos "Active Directory", por padrão. Os requisitos de repositório de atributos dependem se a sua organização está agindo como o parceiro de conta \(que hospeda os usuários federados\) ou o parceiro de recurso \(que hospeda o\)de aplicativos federados.  
  
**Repositórios de atributos LDAP**  
  
Ao trabalhar com outro protocolo de acesso a diretório \(LDAP\)\-armazenamentos de atributos baseados, você deve se conectar a um servidor LDAP que dá suporte à autenticação integrada do Windows. A cadeia de conexão LDAP também deve ser escrita no formato de uma URL LDAP, como descrito no RFC 2255.  
  
Também é necessário que a conta de serviço do serviço de AD FS tenha o direito de recuperar informações de usuário no repositório de atributos LDAP.  
  
**Repositórios de atributos SQL Server**  
  
Para AD FS no Windows Server 2012 R2 operar com êxito, os computadores que hospedam o repositório de atributos SQL Server devem estar executando Microsoft SQL Server 2008 ou superior. Ao trabalhar com repositórios de atributos baseados no SQL\-, você também deve configurar uma cadeia de conexão.  
  
**Repositórios de atributos personalizados**  
  
Você pode desenvolver repositórios de atributos personalizados para habilitar cenários avançados.  
  
-   A linguagem da política interna do AD FS pode referenciar repositórios de atributos personalizados para que qualquer um dos seguintes cenários seja aprimorado:  
  
    -   Criar declaração para um usuário autenticado localmente  
  
    -   Suplementar declarações para um usuário autenticado externamente  
  
    -   Autorizar um usuário a obter um token  
  
    -   Autorizar um serviço a obter um token sobre o comportamento de um usuário  
  
    -   Emitir dados adicionais em tokens de segurança emitidos por AD FS para terceiras partes confiáveis.  
  
-   Todos os repositórios de atributos personalizados devem ser criados com base no .NET 4,0 ou superior.  
  
Quando você trabalha com um repositório de atributos personalizado, também pode ser necessário configurar uma cadeia de conexão. Nesse caso, você pode inserir um código personalizado de sua escolha que permite uma conexão com o repositório de atributos personalizado. A cadeia de conexão nessa situação é um conjunto de pares de nome\/valor que são interpretados como implementados pelo desenvolvedor do repositório de atributos personalizado. Para obter mais informações sobre como desenvolver e usar repositórios de atributos personalizados, consulte [visão geral do repositório de atributos](https://go.microsoft.com/fwlink/?LinkId=190782).  
  
## <a name="application-requirements"></a><a name="BKMK_9"></a>Requisitos do aplicativo  
O AD FS dá suporte a aplicativos que reconhecem declarações\-que usam os seguintes protocolos:  
  
-   Federação do WS\-  
  
-   \-de confiança do WS  
  
-   Protocolo SAML 2,0 usando IDPLite, divida & perfis eGov 1.5.  
  
-   Perfil de concessão de autorização do OAuth 2,0  
  
O AD FS também dá suporte à autenticação e autorização para qualquer declaração não\-\-aplicativos compatíveis com o proxy de aplicativo Web.  
  
## <a name="authentication-requirements"></a><a name="BKMK_10"></a>Requisitos de autenticação  
**Autenticação de AD DS \(autenticação primária\)**  
  
Para acesso à intranet, há suporte para os mecanismos de autenticação padrão a seguir para AD DS:  
  
-   Autenticação integrada do Windows usando Negotiate para Kerberos & NTLM  
  
-   Autenticação de formulários usando nome de usuário\/senhas  
  
-   Autenticação de certificado usando certificados mapeados para contas de usuário no AD DS  
  
Para acesso à extranet, há suporte para os seguintes mecanismos de autenticação:  
  
-   Autenticação de formulários usando nome de usuário\/senhas  
  
-   Autenticação de certificado usando certificados mapeados para contas de usuário no AD DS  
  
-   A autenticação integrada do Windows usando Negotiate \(NTLM somente\) para os pontos de extremidade de confiança do WS\-que aceitam a autenticação integrada do Windows.  
  
Para autenticação de certificado:  
  
-   Estende-se para cartões inteligentes que podem ser protegidos por PIN.  
  
-   A GUI para o usuário inserir seu PIN não é fornecida pelo AD FS e é necessária para fazer parte do sistema operacional do cliente que é exibido ao usar o TLS do cliente.  
  
-   O provedor de serviços de criptografia e leitor \(CSP\) para o cartão inteligente deve funcionar no computador onde o navegador está localizado.  
  
-   O certificado de cartão inteligente deve se encadear a uma raiz confiável em todos os servidores de AD FS e servidores proxy de aplicativo Web.  
  
-   O certificado deve mapear para a conta de usuário no AD DS por um dos seguintes métodos:  
  
    -   O nome da entidade do certificado corresponde ao nome distinto LDAP de uma conta de usuário no AD DS.  
  
    -   A extensão altName da entidade do certificado tem o nome principal do usuário \(\) UPN de uma conta de usuário no AD DS.  
  
Para autenticação integrada do Windows direta usando o Kerberos na intranet,  
  
-   É necessário que o nome do serviço faça parte dos sites confiáveis ou sites da intranet local.  
  
-   Além disso, o HOST\/< ADFS\_serviço\_nome > SPN deve ser definido na conta de serviço na qual o farm de AD FS é executado.  
  
**Autenticação de fator\-múltiplo**  
  
O AD FS dá suporte à autenticação adicional \(além da autenticação primária com suporte pelo AD DS\) usando um modelo de provedor no qual os fornecedores\/clientes podem criar seu próprio adaptador de autenticação de fator\-que um administrador pode registrar e usar durante o logon.  
  
Todo adaptador MFA deve ser criado com base no .NET 4,5.  
  
Para obter mais informações sobre MFA, consulte [gerenciar riscos com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  
  
**Autenticação do dispositivo**  
  
O AD FS dá suporte à autenticação de dispositivo usando certificados provisionados pelo serviço de registro de dispositivo durante o ato de um local de trabalho do usuário final ingressando em seu dispositivo.  
  
## <a name="workplace-join-requirements"></a><a name="BKMK_11"></a>Requisitos de ingresso no local de trabalho  
Os usuários finais podem ingressar seus dispositivos em uma organização usando AD FS. Isso é suportado pelo serviço de registro de dispositivo no AD FS. Como resultado, os usuários finais obtêm o benefício adicional do SSO nos aplicativos com suporte pelo AD FS. Além disso, os administradores podem gerenciar riscos restringindo o acesso a aplicativos somente a dispositivos que foram ingressados no local de trabalho na organização. Abaixo estão os requisitos a seguir para habilitar esse cenário.  
  
-   AD FS dá suporte ao ingresso no local de trabalho para dispositivos Windows 8.1 e iOS 5\+  
  
-   Para usar Workplace Join funcionalidade, o esquema da floresta para o qual AD FS servidores estão associados deve ser o Windows Server 2012 R2.  
  
-   O nome alternativo da entidade do certificado SSL para AD FS serviço deve conter o valor enterpriseregistration que é seguido pelo nome principal \(sufixo de\) UPN da sua organização, por exemplo, enterpriseregistration.corp.contoso.com.  
  
## <a name="cryptography-requirements"></a><a name="BKMK_12"></a>Requisitos de criptografia  
A tabela a seguir fornece informações adicionais de suporte de criptografia sobre a assinatura de token AD FS, criptografia de token\/funcionalidade de descriptografia:  
  
||||  
|-|-|-|  
|**Algoritmo**|**Comprimentos de chave**|**Protocolos\/aplicativos\/comentários**|  
|TripleDES – padrão 192 \(com suporte 192 – 256\) \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#TripleDES\-CBC](http://www.w3.org/2001/04/xmlenc#tripledes-cbc)|>\= 192|Algoritmo com suporte para descriptografar o token de segurança. Não há suporte para a criptografia do token de segurança com esse algoritmo.|  
|AES128 \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#AES128\-CBC](http://www.w3.org/2001/04/xmlenc#aes128-cbc)|128|Algoritmo com suporte para descriptografar o token de segurança. Não há suporte para a criptografia do token de segurança com esse algoritmo.|  
|AES192 \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#AES192\-CBC](http://www.w3.org/2001/04/xmlenc#aes192-cbc)|192|Algoritmo com suporte para descriptografar o token de segurança. Não há suporte para a criptografia do token de segurança com esse algoritmo.|  
|AES256 \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#aes256\-CBC](http://www.w3.org/2001/04/xmlenc#aes256-cbc)|256|**Padrão**. Algoritmo com suporte para criptografar o token de segurança.|  
|TripleDESKeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-TripleDES](http://www.w3.org/2001/04/xmlenc#kw-tripledes)|Todos os tamanhos de chave com suporte no .NET 4,0\+|Algoritmo com suporte para criptografar a chave simétrica que criptografa um token de segurança.|  
|AES128KeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-aes128](http://www.w3.org/2001/04/xmlenc#kw-aes128)|128|Algoritmo com suporte para criptografar a chave simétrica que criptografa o token de segurança.|  
|AES192KeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-aes192](http://www.w3.org/2001/04/xmlenc#kw-aes192)|192|Algoritmo com suporte para criptografar a chave simétrica que criptografa o token de segurança.|  
|AES256KeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-aes256](http://www.w3.org/2001/04/xmlenc#kw-aes256)|256|Algoritmo com suporte para criptografar a chave simétrica que criptografa o token de segurança.|  
|RsaV15KeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#rsa\-1\_5](http://www.w3.org/2001/04/xmlenc#rsa-1_5)|1024|Algoritmo com suporte para criptografar a chave simétrica que criptografa o token de segurança.|  
|RsaOaepKeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#rsa\-oaep\-mgf1p](http://www.w3.org/2001/04/xmlenc#rsa-oaep-mgf1p)|1024|Default. Algoritmo com suporte para criptografar a chave simétrica que criptografa o token de segurança.|  
|SHA1\-[http:\/\/www.w3.org\/imgs\/DSig\/SHA1\_1\_0. html](http://www.w3.org/PICS/DSig/SHA1_1_0.html)|N\/um|Usado pelo servidor de AD FS na geração de SourceID de artefato: nesse cenário, o STS usa a \(SHA1 de acordo com a recomendação no\) padrão SAML 2,0 para criar um valor de bit 160 curto para o artefato SourceID.<p>Também usado pelo agente Web do ADFS \(componente herdado do WS2003 TimeTime\) para identificar alterações em um valor de tempo "última atualização" para que ele saiba quando atualizar as informações do STS.|  
|\- SHA1withRSA<p>[http:\/\/www.w3.org\/PICS\/DSig\/RSA\-SHA1\_1\_0. html](http://www.w3.org/PICS/DSig/RSA-SHA1_1_0.html)|N\/um|Usado em casos em que o servidor AD FS valida a assinatura de AuthenticationRequest SAML, assine a solicitação de resolução de artefato ou a resposta, crie o token\-certificado de assinatura.<p>Nesses casos, SHA256 é o padrão, e SHA1 é usado somente se o parceiro \(terceira parte confiável\) não dá suporte a SHA256 e deve usar SHA1.|  
  
## <a name="permissions-requirements"></a><a name="BKMK_13"></a>Requisitos de permissões  
O administrador que executa a instalação e a configuração inicial de AD FS deve ter permissões de administrador de domínio no domínio local \(em outras palavras, o domínio ao qual o servidor de Federação está ingressado.\)  
  
## <a name="see-also"></a>Consulte também  
[Guia de design do AD FS no Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  
