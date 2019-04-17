---
ms.assetid: ac727bd1-a892-47ed-a7ba-439b34187d4e
title: "Instalação do AD DS e descrições de página do Assistente de remoção"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fa023398822e79ca8c3e93d44bb1e87fc9190cee
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-ds-installation-and-removal-wizard-page-descriptions"></a>Instalação do AD DS e descrições de página do Assistente de remoção

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico apresenta descrições para os controles nas seguintes páginas do assistente que compõem a instalação de função de servidor do AD DS e a remoção no Gerenciador do servidor.  
  
-   [Configuração da implantação](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DepConfigPage)  
  
-   [Opções de controlador de domínio](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage)  
  
-   [Opções de DNS](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage)  
  
-   [Opções de RODC](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RODCOptionsPage)  
  
-   [Opções adicionais](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdditionalOptionsPage)  
  
-   [Caminhos](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Paths)  
  
-   [Opções de preparação](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdprepCreds)  
  
-   [Opções de revisão](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ViewInstallOptionsPage)  
  
-   [Seleção de pré-requisitos](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_PrerqCheckPage)  
  
-   [Resultados](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Results)  
  
-   [Credenciais de remoção de função](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalCredsPage)  
  
-   [Avisos e opções de remoção do AD DS](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalOptionsPage)  
  
-   [Nova senha de administrador](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_NewAdminPwdPage)  
  
-   [Confirmar seleções de remoção de função](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ConfirmRoleRemovalPage)  
  
## <a name="BKMK_DepConfigPage"></a>Configuração da implantação  
Gerenciador do servidor começa a instalação de cada controlador de domínio com o **implantação configuração** página. As opções restantes e campos obrigatórios alterar essa página e as páginas subsequentes, dependendo de qual operação de implantação que você selecionar. Por exemplo, se você criar uma nova floresta, o **opções de preparação** página não será exibida, mas não se você instalar o primeiro controlador de domínio que executa o Windows Server 2012 em uma floresta existente ou de domínio.  
  
Alguns testes validações são executadas nesta página e novamente mais tarde como parte de verificações de pré-requisito. Por exemplo, se você tentar instalar o primeiro controlador de domínio do Windows Server 2012 em uma floresta que tem o nível funcional do Windows 2000, um erro é exibido nesta página.  
  
As seguintes opções aparecem quando você cria uma nova floresta.  
  
![Instalação do AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
-   Quando você cria uma nova floresta, você deve especificar um nome de domínio raiz da floresta. O nome de domínio de raiz da floresta não pode ser rotulada única (por exemplo, ele deve ser "contoso.com" em vez de "contoso"). Ele deve usar permitidas convenções de nomenclatura do domínio DNS. Você pode especificar um nome de domínio internacionalizado (IDN). Para obter mais informações sobre convenções de nomenclatura de domínio DNS, consulte [KB 909264](https://support.microsoft.com/kb/909264).  
  
-   Não crie novos florestas do Active Directory com o mesmo nome de seu nome DNS externo. Por exemplo, se sua URL de DNS de Internet é http://contoso.com, você deve escolher um nome diferente para sua floresta interna evitar problemas de compatibilidade futura. Esse nome deve ser exclusivo e improváveis para o tráfego da web, como corp.contoso.com.  
  
-   Você deve ser um membro do grupo Administradores no servidor em que você deseja criar uma nova floresta.  
  
Para obter mais informações sobre como criar uma floresta, consulte [instalar um novo Windows Server 2012 floresta do Active Directory & #40; Nível 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
As seguintes opções aparecem quando você cria um novo domínio.  
  
![Instalação do AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_ChildDomain.gif)  
  
> [!NOTE]  
> Se você criar um novo domínio de árvore, você precisa especificar o nome do domínio raiz da floresta em vez do domínio pai, mas os demais páginas do assistente e as opções são as mesmas.  
  
-   Clique em **selecione** para navegar para o domínio pai ou a árvore do Active Directory ou digite um nome de domínio ou árvore de pai válido. Digite o nome do novo domínio em **novo nome de domínio**.  
  
-   Domínio de árvore: fornecer um nome de domínio totalmente qualificado raiz; o nome não pode ser rotulada única e deve usar requisitos de nome de domínio DNS.  
  
-   Domínio filho: fornecer um filho válido, de rótulo único nome de domínio; o nome deve usar requisitos de nome de domínio DNS.  
  
-   O Assistente de configuração de serviços de domínio Active Directory solicita credenciais de domínio se suas credenciais atuais não são do domínio. Clique em **alteração** para fornecer as credenciais de domínio.  
  
Para obter mais informações sobre como criar um domínio, consulte [instalar um novo Windows Server 2012 Active Directory filho ou árvore de domínio & #40; Nível 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
As seguintes opções aparecem quando você adiciona um novo controlador de domínio a um domínio existente.  
  
![Instalação do AD DS](./media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Replica.gif)  
  
-   Clique em **selecione** para navegar no domínio ou digite um nome de domínio válido.  
  
-   Gerenciador do servidor solicita credenciais válidas se necessário. Instalando um controlador de domínio adicional requer a associação ao grupo Administradores do domínio.  
  
    Além disso, instalando o primeiro controlador de domínio que executa o Windows Server 2012 em uma floresta requer credenciais que incluem participações em grupos os administradores corporativos e os administradores de esquema. O Assistente de configuração de serviços de domínio Active Directory solicita que você mais tarde se suas credenciais atuais não tiver as permissões adequadas ou membros do grupo.  
  
Para obter mais informações sobre como adicionar um controlador de domínio a um domínio existente, consulte [instalar um controlador de domínio da réplica Windows Server 2012 em um domínio existente & #40; Nível 200 & #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_DCOptionsPage"></a>Opções de controlador de domínio  
Se você estiver criando uma nova floresta, a página Opções de controlador de domínio possui as seguintes opções:  
  
![Instalação do AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Forest.gif)  
  
-   Por padrão, os níveis funcionais de floresta e domínio são definidos para o Windows Server 2012.  
  
    Há um novo recurso disponível no nível funcional de domínio do Windows Server 2012: o suporte para controle de acesso dinâmico e proteção de política de modelo administrativo KDC Kerberos tem duas configurações (sempre fornecer declarações e falhar solicitações de autenticação unarmored) que exigem o nível funcional de domínio do Windows Server 2012. Para obter mais informações, consulte "Suporte para declarações, autenticação composta e proteção Kerberos" em [as novidades na autenticação Kerberos](https://technet.microsoft.com/library/hh831747.aspx).    
    O nível funcional da floresta Windows Server 2012 não fornece os novos recursos, mas garante que qualquer novo domínio criado na floresta automaticamente funcionará no nível funcional de domínio do Windows Server 2012. O nível funcional de domínio do Windows Server 2012 não fornece nenhum novo outros recursos ao lado de suporte para controle de acesso dinâmico e proteção Kerberos, mas garante que qualquer controlador de domínio no domínio seja executado em Windows Server 2012. Para saber mais sobre outros recursos que estão disponíveis em diferentes níveis funcionais, consulte [níveis funcionais de Noções básicas sobre serviços Active Directory domínio (AD DS)](../active-directory-functional-levels.md).  
  
    Além dos níveis funcionais, um controlador de domínio que executa o Windows Server 2012 fornece recursos adicionais que não estão disponíveis em um controlador de domínio que executa uma versão anterior do Windows Server. Por exemplo, um controlador de domínio que executa o Windows Server 2012 pode ser usado para clonagem de controlador de domínio virtual, enquanto um controlador de domínio que executa uma versão anterior do Windows Server não é possível.  
  
-   Servidor DNS é selecionado por padrão quando você cria uma nova floresta. O primeiro controlador de domínio na floresta deve ser um servidor de catálogo global (GC), e não pode ser um controlador de domínio somente leitura (RODC).  
  
-   A senha do modo de restauração de serviços de diretório (DSRM) é necessária para fazer logon um controlador de domínio em que o AD DS não está em execução. A senha especificada deve cumprir a política de senha aplicada ao servidor, que, por padrão, não exige uma senha forte; apenas uma senha não está em branco. Sempre escolha uma senha forte e complexa ou preferencialmente, uma senha. Para obter informações sobre como sincronizar a senha DSRM com a senha de uma conta de usuário de domínio, consulte [KB 961320](https://support.microsoft.com/kb/961320).  
  
Para obter mais informações sobre como criar uma floresta, consulte [instalar um novo Windows Server 2012 floresta do Active Directory & #40; Nível 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
Se você estiver criando um domínio filho, a página Opções de controlador de domínio possui as seguintes opções:  
  
![Instalação do AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Child.gif)  
  
-   Por padrão, o nível funcional do domínio é definido para o Windows Server 2012. Você pode especificar qualquer valor que tenha pelo menos o valor de nível funcional da floresta ou superior.  
  
-   As opções de controlador de domínio configuráveis incluem **servidor DNS** e **Catálogo Global**; Você não pode configurar o controlador de domínio somente leitura como o primeiro controlador de domínio em um novo domínio.  
  
    A Microsoft recomenda que todos os controladores de domínio fornecem serviços de catálogo global para alta disponibilidade em ambientes distribuídos, por isso, o assistente permite que essas opções por padrão ao criar um novo domínio e DNS.  
  
-   O **opções de controlador de domínio** página também permite que você escolha Active Directory apropriados lógico **nome do site** da configuração do floresta. Por padrão, ele selecionará o site com a sub-rede mais correta. Se houver apenas um site, ele selecionará esse site automaticamente.  
  
    > [!IMPORTANT]  
    > Se o servidor não pertence a uma sub-rede do Active Directory e não há mais de um site, nada é selecionado e o **próxima** botão não está disponível até que você escolha um site na lista.  
  
Para obter mais informações sobre como criar um domínio, consulte [instalar um novo Windows Server 2012 Active Directory filho ou árvore de domínio & #40; Nível 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
Se você estiver adicionando um controlador de domínio em um domínio, a página Opções de controlador de domínio possui as seguintes opções:  
  
![Instalação do AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Replica.gif)  
  
-   As opções de controlador de domínio configuráveis incluem **servidor DNS** e **Catálogo Global**, e **controlador de domínio somente leitura**.  
  
    A Microsoft recomenda que todos os controladores de domínio fornecem serviços de catálogo global para alta disponibilidade em ambientes distribuídos, por isso, o assistente permite que essas opções por padrão e DNS. Para obter mais informações sobre a implantação RODCs, consulte [planejamento de controlador de domínio somente leitura e guia de implantação](https://technet.microsoft.com/library/cc771744(v=WS.10).aspx).  
  
Para obter mais informações sobre como adicionar um controlador de domínio a um domínio existente, consulte [instalar um controlador de domínio da réplica Windows Server 2012 em um domínio existente & #40; Nível 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_DNSOptionsPage"></a>Opções de DNS  
Se você instalar o servidor DNS, o seguinte **DNS opções** página será exibida:  
  
![Instalação do AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DNSOptions_Replica.gif)  
  
Quando você instala o servidor DNS, registros de delegação que apontam para o servidor DNS como autoritativos para a zona devem ser criados na zona de sistema de nome de domínio (DNS) pai. Registros de delegação transferir a autoridade de resolução de nome e fornecem referência correta a outros servidores DNS e clientes dos novos servidores que estão se tornando autoritativos para a nova região. Esses registros de recurso incluem o seguinte:  
  
-   Um registro de recurso de servidor (NS) de nome para efetuar a delegação. Este registro de recurso anuncia que o servidor chamado ns1.na.example.microsoft.com é um servidor de autorização para o subdomínio delegado.  
  
-   Um registro de recurso de host (A ou AAAA) também conhecido como registro cola deve estar presente para resolver o nome do servidor que é especificado no registro de recurso de servidor (NS) nome para seu endereço IP. O processo de resolução de nome do host neste registro de recurso para o servidor DNS delegado no registro de recurso de servidor (NS) nome, às vezes, é conhecido como "buscar cola."  
  
Você pode ter o Assistente de configuração de serviços de domínio Active Directory criá-las automaticamente. O assistente verifica se existem registros apropriados na zona DNS pai depois de clicar em **próxima** sobre o **opções de controlador de domínio** página. Se o assistente não puder verificar se existem registros no domínio pai, o assistente fornece automaticamente com a opção de criar uma nova delegação de DNS para um novo domínio (ou atualizar a delegação existente) e continue com a nova instalação de controlador de domínio.  
  
Como alternativa, você pode criar esses registros de delegação DNS antes de instalar o servidor DNS. Para criar uma delegação de zona, abra **Gerenciador DNS**, clique com botão direito no domínio pai e, em seguida, clique em **nova delegação**. Siga as etapas no Assistente de nova delegação para criar a delegação.  
  
O processo de instalação tenta criar a delegação para garantir que os computadores em outros domínios podem resolver consultas do DNS para hosts, incluindo controladores de domínio e computadores membro, no subdomínio DNS. Observe que os registros de delegação podem ser criados automaticamente apenas em servidores DNS da Microsoft. Se a zona de domínio DNS pai reside em servidores DNS de terceiros, como BIND, um aviso sobre a falha para criar registros de delegação DNS será exibida na página de seleção de pré-requisitos. Para saber mais sobre o aviso, consulte [problemas para instalar o AD DS conhecidos](https://technet.microsoft.com/library/cc754463(v=WS.10).aspx).  
  
Delegações entre o domínio pai e o subdomínio sendo promovido podem ser criadas e validadas antes ou após a instalação. Não há nenhuma razão para atrasar a instalação de um novo controlador de domínio, pois você não pode criar ou atualizar a delegação de DNS.  
  
Para obter mais informações sobre delegação, consulte [delegação de zona de Noções básicas sobre](https://go.microsoft.com/fwlink/?LinkId=164773) (https://go.microsoft.com/fwlink/?LinkId=164773). Se a delegação de zona não for possível sua situação, você pode considerar outros métodos para fornecer a resolução de nomes de outros domínios para os hosts no seu domínio. Por exemplo, o administrador DNS de outro domínio pode configurar o encaminhamento condicional, zonas de stub ou secundária zonas para resolver nomes no seu domínio. Para obter mais informações, consulte os seguintes tópicos:  
  
-   [Noções básicas sobre tipos de zona](https://go.microsoft.com/fwlink/?LinkID=157399) (https://go.microsoft.com/fwlink/?LinkID=157399)  
  
-   [Noções básicas sobre as zonas de stub](https://go.microsoft.com/fwlink/?LinkId=164776) (https://go.microsoft.com/fwlink/?LinkId=164776)  
  
-   [Noções básicas sobre encaminhadores](https://go.microsoft.com/fwlink/?LinkId=164778) (https://go.microsoft.com/fwlink/?LinkId=164778)  
  
## <a name="BKMK_RODCOptionsPage"></a>Opções de RODC  
As seguintes opções aparecem quando você instala um controlador de domínio somente leitura (RODC).  
  
![Instalação do AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_RODCOptions.gif)  
  
-   Contas de administrador delegado obtém permissões administrativas locais no RODC. Esses usuários podem operar com privilégios equivalentes ao grupo de administradores do computador local. Eles não são membros do Admins. do domínio ou os grupos de administradores internos do domínio. Essa opção é útil para a delegação de administração do office ramificação sem divulgar permissões administrativas do domínio. Não é necessário configurar a delegação de administração. Para obter mais informações, consulte [separação de função de administrador](https://technet.microsoft.com/library/cc753170(v=WS.10).aspx).  
  
-   A política de replicação de senha atua como uma lista de controle de acesso (ACL). Ela determina se um RODC deve ter permissão para armazenar em cache uma senha. Depois que o RODC recebe uma solicitação de logon do usuário ou computador autenticada, ele se refere à política de replicação de senha para determinar se a senha da conta deve ser armazenada em cache. A mesma conta pode então executar logons subsequentes com mais eficiência.  
  
    A política de replicação de senha (PRP) lista as contas cujas senhas podem ser armazenadas em cache e contas cujas senhas são negadas explicitamente no cache. A lista de contas de usuário e computador que têm permissão para ser armazenado em cache não implica que o RODC tem necessariamente armazenada em cache as senhas para essas contas. Um administrador pode, por exemplo, especificar com antecedência todas as contas que um RODC armazenará em cache. Dessa forma, este poderá autenticar essas contas, mesmo se o link da WAN para o site de hub está offline.  
  
    Os usuários ou computadores que não são permitidos (incluindo implícita) ou fazer negado não armazenar em cache sua senha. Se os usuários ou computadores não têm acesso a um controlador de domínio gravável, eles não podem acessar recursos de AD DS fornecidos ou funcionalidade. Para saber mais sobre a PRP, consulte [política de replicação de senha](https://technet.microsoft.com/library/cc730883(v=ws.10).aspx). Para obter mais informações sobre como gerenciar a PRP, consulte [administrando a política de replicação de senha](https://technet.microsoft.com/library/rodc-guidance-for-administering-the-password-replication-policy(v=ws.10).aspx).  
  
Para obter mais informações sobre como instalar RODCs, consulte [instalar um Windows Server 2012 Active Directory Read-Only controlador de domínio & #40; RODC & #41; & #40; Nível 200 & #41;](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md).  
  
## <a name="BKMK_AdditionalOptionsPage"></a>Opções adicionais  
A seguinte opção aparecerá no **opções adicionais** página se você estiver criando um novo domínio:  
  
![Instalação do AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Child.gif)  
  
As seguintes opções aparecem no **opções adicionais** página se você instalar um controlador de domínio adicional em um domínio existente:  
  
![Instalação do AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Replica.gif)  
  
-   Você pode especificar um controlador de domínio como a origem de replicação ou permitir que o assistente escolher qualquer controlador de domínio como a origem de replicação.  
  
-   Você também pode escolher instalar o controlador de domínio usando o backup de mídia usando a instalação da opção de mídia (IFM). Se a mídia de instalação é armazenada localmente, o **instalar da mídia caminho** opção permite que você navegue até o local do arquivo. A opção Procurar não está disponível para uma instalação remota. Você pode clicar em **verificar** para garantir que o caminho fornecido é mídia válida. Mídia usada pela opção IFM deve ser criada com o Backup do Windows Server ou Ntdsutil.exe de outro existente Windows Server 2012 computador Você não pode usar um Windows Server 2008 R2 ou o sistema operacional anterior para criar mídia para um controlador de domínio do Windows Server 2012. Se a mídia está protegida com um SYSKEY, Gerenciador do servidor solicita senha da imagem durante a verificação.  
  
Para obter mais informações sobre como criar um domínio, consulte [instalar um novo Windows Server 2012 Active Directory filho ou árvore de domínio & #40; Nível 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md). Para obter mais informações sobre como adicionar um controlador de domínio a um domínio existente, consulte [instalar um controlador de domínio da réplica Windows Server 2012 em um domínio existente & #40; Nível 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_Paths"></a>Caminhos  
As seguintes opções aparecem no **caminhos** página.  
  
![Instalação do AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_Paths.gif)  
  
-   O **caminhos** página permite substituir os locais de pastas padrão do AD DS banco de dados, os logs de transação de banco de dados, e o SYSVOL compartilhar. Os locais padrão estão sempre em % systemroot %.  
  
Especifique o local do banco de dados do AD DS (NTDS.DIT), faça logon arquivos e SYSVOL. Para uma instalação local, você pode navegar até o local onde deseja armazenar os arquivos.  
  
## <a name="BKMK_AdprepCreds"></a>Opções de preparação  
![Instalação do AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PreparationOptions.gif)  
  
Se você não fez logon com credenciais suficientes para executar comandos adprep.exe e adprep é necessário para executar para concluir a instalação do AD DS, você é solicitado a fornecer as credenciais para executar adprep.exe. Adprep é necessária para executar para adicionar o primeiro controlador de domínio que executa o Windows Server 2012 a um domínio ou floresta existente. Mais especificamente:  
  
-   Adprep /forestprep deve ser executado para adicionar o primeiro controlador de domínio que executa o Windows Server 2012 para uma floresta existente. Esse comando deve ser executado por um membro do grupo Administradores de empresa, do grupo Administradores de esquema e o grupo Admins. do domínio do domínio que hospeda o mestre de esquema. Para esse comando concluído com êxito, deve haver conectividade entre o computador em que você executar o comando e o mestre de esquema da floresta.  
  
-   Adprep /domainprep deve ser executado para adicionar o primeiro controlador de domínio que executa o Windows Server 2012 a um domínio existente. Esse comando deve ser executado por um membro do grupo Admins. do domínio do domínio onde você está instalando o controlador de domínio que executa o Windows Server 2012. Para esse comando concluído com êxito, deve haver conectividade entre o computador em que você executar o comando e o mestre de infraestrutura para o domínio.  
  
-   Adprep /rodcprep deve ser executado para adicionar o primeiro RODC a uma floresta existente. Esse comando deve ser executado por um membro do grupo Administradores corporativos. Para esse comando concluído com êxito, deve haver conectividade entre o computador em que você executar o comando e o mestre de infraestrutura para cada partição de diretório do aplicativo na floresta.  
  
Para saber mais sobre Adprep.exe, consulte [Adprep.exe integração](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_NewAdprep) e veja [executando Adprep.exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx).  
  
## <a name="BKMK_ViewInstallOptionsPage"></a>Opções de revisão  
![Instalação do AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_ReviewOptions.gif)  
  
-   O **opções de revisão** página permite que você validar as configurações e certifique-se de que atendam aos requisitos antes de iniciar a instalação. Isso não é a última oportunidade de parar a instalação usando o Gerenciador do servidor. Esta página simplesmente permite que você revise e confirme as configurações antes de continuar a configuração.  
  
-   O **opções de revisão** página no Gerenciador do servidor também oferece um recurso opcional **Exibir Script** botão para criar um arquivo de texto Unicode que contém a configuração ADDSDeployment atual como um único script do Windows PowerShell. Isso permite que você use a interface gráfica do Gerenciador do servidor como um estúdio de implantação do Windows PowerShell. Use o Assistente de configuração de serviços de domínio Active Directory para configurar as opções, exportar a configuração e, em seguida, cancelá-lo. Esse processo cria um exemplo válido e sintaticamente correto para ainda mais a modificação ou uso direto.  
  
## <a name="BKMK_PrerqCheckPage"></a>Seleção de pré-requisitos  
![Instalação do AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PrerequisitesCheck.gif)  
  
Algumas dos avisos que aparecem nesta página incluem:  
  
-   Controladores de domínio que executam o Windows Server 2008 ou posterior tem uma configuração de padrão para "Permitir algoritmos de criptografia compatíveis com o Windows NT 4" que impede que os algoritmos de criptografia mais fracos ao estabelecer sessões do canal seguro. Para obter mais informações sobre o impacto potencial e uma solução alternativa, consulte o artigo KB [942564](https://support.microsoft.com/kb/942564).  
  
-   Delegação de DNS não poderia ser criada ou atualizada. Para obter mais informações, consulte [DNS opções](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage).  
  
-   A verificação de pré-requisitos requer chamadas WMI. Eles podem falhar se eles são blocos de regras de firewall bloqueado e retornam um servidor RPC erro não está disponível.  
  
Para saber mais sobre as verificações de pré-requisito específicas que são executadas para instalação do AD DS, consulte [testes de pré-requisito](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_ADDSInstallPrerequisiteTests).  
  
## <a name="BKMK_Results"></a>Resultados  
![Instalação do AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_SMResultsBeta.gif)  
  
Nessa página, você pode analisar os resultados da instalação.  
  
Você também pode optar por reiniciar o servidor de destino depois que o assistente for concluído, mas se a instalação for bem-sucedida, o servidor sempre será reiniciado, independentemente de se você selecionar essa opção. Em alguns casos depois que o assistente for concluído em um servidor de destino que não ingressou no domínio antes da instalação, o estado do sistema do servidor de destino de tornar o servidor inacessível na rede ou o estado do sistema pode evitar que você tenha permissões para gerenciar o servidor remoto.  
  
Se o servidor de destino não reiniciar nesse caso, você deve reiniciá-lo manualmente. Ferramentas, como shutdown.exe ou do Windows PowerShell não é possível reiniciá-lo. Você pode usar os serviços de área de trabalho remota para fazer logon e desligar remotamente o servidor de destino.  
  
## <a name="BKMK_RemovalCredsPage"></a>Credenciais de remoção de função  
![Instalação do AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Credentials.gif)  
  
Configurar opções de rebaixamento no **credenciais** página. Forneça as credenciais necessárias para executar o rebaixamento na lista a seguir:  
  
-   Rebaixar um controlador de domínio adicionais requer credenciais de administrador do domínio. Selecionando **forçar remoção do controlador de domínio** rebaixa o controlador de domínio sem remover os metadados do objeto de controlador de domínio do Active Directory.  
  
    > [!IMPORTANT]  
    > Não selecione essa opção, a menos que o controlador de domínio não pode entrar em contato com outros controladores de domínio e não há *nenhuma maneira razoável* para resolver esse problema de rede. Rebaixamento forçado deixa órfã metadados no Active Directory nos controladores de domínio restantes na floresta. Além disso, todas as alterações não replicadas no controlador de domínio, como senhas ou novas contas de usuário, são perdidas para sempre. Metadados órfã são a causa raiz em uma porcentagem significativa dos casos de atendimento ao cliente Microsoft para o AD DS, Exchange, SQL e outro software. Se você rebaixar forçadamente um controlador de domínio, você *deve* executar manualmente a limpeza de metadados imediatamente. Para obter etapas, examine [limpa metadados do servidor](https://technet.microsoft.com/library/cc816907(WS.10).aspx).  
  
-   Rebaixar o último controlador de domínio em um domínio requer a membros do grupo Administradores corporativos, como isso remove o próprio domínio (se esse for o último domínio na floresta, isso remove da floresta). Gerenciador do servidor informa se o controlador de domínio atual é o último controlador de domínio no domínio. Selecione **último controlador de domínio no domínio** para confirmar o domínio controlador é o último controlador de domínio no domínio.  
  
Para obter mais informações sobre como remover o AD DS, consulte [remover Active Directory Domain Services (nível 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) e [rebaixando controladores de domínio e domínios & #40; Nível 200 & #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_RemovalOptionsPage"></a>Avisos e opções de remoção do AD DS  
Se você precisar de ajuda com a página de opções de análise, veja opções de revisão  
  
Se o controlador de domínio hospeda funções adicionais, como a função de servidor DNS ou servidor de catálogo global, aparecerá a seguinte página de aviso:  
  
![Instalação do AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Warnings.gif)  
  
Você deve clicar **prosseguir com a remoção de** para confirmar que as funções adicionais não estará mais disponíveis antes de poder clicar **próxima** para continuar.  
  
Se você forçar a remoção de um controlador de domínio, as alterações de objeto do Active Directory que não foram replicados para outros controladores de domínio no domínio serão perdidas. Além disso, se o controlador de domínio hospeda funções mestre de operações, o catálogo global ou a função de servidor DNS, operações essenciais na floresta e domínio podem ser afetadas da seguinte maneira. Antes de remover um controlador de domínio que hospeda qualquer função mestre de operações, tente transferir a função para outro controlador de domínio. Se não for possível transferir a função, primeiro remova os serviços de domínio do Active Directory neste computador e, em seguida, usar Ntdsutil.exe para capturar a função. Use Ntdsutil no controlador de domínio que você pretende capturar a função Se possível, use um parceiro de replicação recentes no mesmo site como neste controlador de domínio. Para saber mais sobre transferindo e capturando funções mestre de operações, consulte [255504 do artigo](https://go.microsoft.com/fwlink/?LinkId=80395) na Base de Conhecimento Microsoft. Se o assistente não pode determinar se o controlador de domínio hospedar uma função de mestre de operações, execute o comando netdom.exe para determinar se esse controlador de domínio executa qualquer funções mestre de operações.  
  
-   Catálogo global: os usuários podem ter dificuldades para fazer logon em domínios na floresta. Antes de remover um servidor de catálogo global, verifique se o suficiente servidores de catálogo global estão na floresta e no site para logons de usuário do serviço. Se necessário, designar outro servidor de catálogo global e atualize clientes e aplicativos com as novas informações.  
  
-   Servidor DNS: todos os dados DNS armazenados nas zonas integradas ao Active Directory serão perdidos. Depois de remover o AD DS, esse servidor DNS não será capaz de executar a resolução de nomes para as zonas DNS que estavam integrada do Active Directory. Portanto, recomendamos que você atualize a configuração DNS de todos os computadores que atualmente se referem ao endereço IP do servidor DNS para resolução de nomes com o endereço IP de um novo servidor DNS.  
  
-   Mestre de infraestrutura: os clientes no domínio podem ter dificuldade para localizar objetos em outros domínios. Antes de continuar, transferir a função mestre de infraestrutura para um controlador de domínio que não é um servidor de catálogo global.  
  
-   Mestre RID: você pode ter problemas ao criar novas contas de usuário, contas de computador e grupos de segurança. Antes de continuar, transferir a função mestre RID para um controlador de domínio no mesmo domínio deste controlador de domínio.  
  
-   Emulador do domínio primário (PDC) do controlador: operações que são executadas com o emulador do PDC, como atualizações de política de grupo e redefinições de senha não-contas do AD DS, não funcionará corretamente. Antes de continuar, transferir a função mestre de emulador do PDC para um controlador de domínio que esteja no mesmo domínio deste controlador de domínio.  
  
-   Mestre de esquema: você não poderá modificar o esquema para essa floresta. Antes de continuar, transferir a função mestre do esquema para um controlador de domínio no domínio raiz da floresta.  
  
-   Mestre de nomeação de domínio: você não poderá adicionar ou remover domínios dessa floresta. Antes de continuar, transferir a função mestre para um controlador de domínio no domínio raiz da floresta de nomeação de domínio.  
  
-   Todas as partições de diretório do aplicativo neste controlador de domínio do Active Directory serão removidas. Se um controlador de domínio contém a última réplica de um ou mais partições de diretório, quando a operação de remoção for concluída, essas partições deixará de existir.  
  
Lembre-se de que o domínio deixará de existir após a desinstalação do Active Directory Domain Services do último controlador de domínio no domínio.  
  
Se o controlador de domínio é um servidor DNS que seja delegado para hospedar a região DNS, a página a seguir fornecerão a opção para remover o servidor DNS de delegação de zona DNS.  
  
![Instalação do AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_RemovalOptions.gif)  
  
Para obter mais informações sobre como remover o AD DS, consulte [remover Active Directory Domain Services (nível 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) e [rebaixando controladores de domínio e domínios & #40; Nível 200 & #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_NewAdminPwdPage"></a>Nova senha de administrador  
O **nova senha de administrador** página exige que você forneça uma senha de conta de administrador do computador local interno, depois que o rebaixamento é concluída e o computador se torna um servidor membro do domínio ou o computador do grupo de trabalho.  
  
![Instalação do AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_NewAdminPwd.gif)  
  
Para obter mais informações sobre como remover o AD DS, consulte [remover Active Directory Domain Services (nível 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) e [rebaixando controladores de domínio e domínios & #40; Nível 200 & #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_ConfirmRoleRemovalPage"></a>Opções de revisão  
O **opções de revisão** página fornece a chance de exportar as definições de configuração para rebaixamento para um script do Windows PowerShell para que você pode automatizar rebaixamentos adicionais. Clique em **diminuir níveis** para remover o AD DS.  
  
![Instalação do AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_ReviewOptions.gif)  
  


