---
ms.assetid: b3d6fb87-c4d4-451c-b3de-a53d2402d295
title: Instalar uma nova floresta do Active Directory do Windows Server 2012 (nível 200)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 187db7e201e98ae97268b96c2e4faa202a9a5372
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874827"
---
# <a name="install-a-new-windows-server-2012-active-directory-forest-level-200"></a>Instalar uma nova floresta do Active Directory do Windows Server 2012 (nível 200)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica o novo recurso de promoção de controlador de domínio dos Serviços de Domínio Active Directory do Windows Server 2012 em um nível introdutório. No Windows Server 2012, o AD DS substitui a ferramenta Dcpromo por um Gerenciador do Servidor e sistema de implantação baseado em Windows PowerShell.  
  
-   [Administração simplificada dos serviços de domínio do Active Directory](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SimplifiedAdmin)  
  
-   [Visão geral técnica](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_TechOverview)  
  
-   [Implantando uma floresta com o Gerenciador do servidor](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)  
  
-   [Implantando uma floresta com o Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_PSForest)  
  
## <a name="BKMK_SimplifiedAdmin"></a>Administração simplificada dos serviços de domínio do Active Directory  
O Windows Server 2012 introduz a próxima geração de Administração Simplificada dos Serviços de Domínio Active Directory, e essa é a mais nova e radical previsão de domínio desde o Windows 2000 Server. Administração simplificada do AD DS traz lições aprendidas de 12 anos de Active Directory e faz uma experiência de administrativa mais intuitiva com maior suporte, mais flexível para arquitetos e administradores. Isso significa criar novas versões de tecnologias existentes, bem como estender as funcionalidades dos componentes liberados no Windows Server 2008 R2.  
  
### <a name="what-is-ad-ds-simplified-administration"></a>O que é Administração Simplificada do AD DS?  
A Administração Simplificada do AD DS é uma reformulação da imagem de implantação do domínio. Alguns desses recursos incluem:  
  
-   A implantação de função do AD DS agora é parte da nova arquitetura do Gerenciador do Servidor e permite instalação remota.  
  
-   O mecanismo de implantação e configuração do AD DS agora é o Windows PowerShell, mesmo quando usar uma configuração gráfica.  
  
-   A promoção agora inclui verificação de pré-requisitos que valida a floresta e a prontidão do domínio para o novo controlador de domínio, diminuindo a oportunidade de falha nas promoções.  
  
-   O nível funcional da floresta do Windows Server 2012 não implementa novos recursos e o nível funcional de domínio é necessário somente para um subconjunto de novos recursos de Kerberos, aliviando os administradores das necessidades frequentes para um ambiente de controlador de domínio mais homogêneo.  
  
### <a name="purpose-and-benefits"></a>Objetivos e benefícios  
Essas mudanças podem parecer mais complexas, não mais simples. Na reformulação do processo de implantação do AD DS, no entanto, houve oportunidade de conciliar muitas etapas e práticas recomendadas com menos etapas e ações mais fáceis. Isso significa, por exemplo, que a configuração gráfica de um novo controlador de domínio de réplica agora apresenta 8 caixas de diálogo, em vez das 12 anteriores. A criação de uma nova floresta do Active Directory requer um *único* comando do Windows PowerShell com somente *um* argumento: o nome do domínio.  
  
Por que existe essa ênfase no Windows PowerShell em Windows Server 2012? Na medida em que a computação distribuída evolui, o Windows PowerShell permite um único mecanismo de configuração e manutenção de ambas as interfaces, gráfica e de linha de comandos. Ele permite sistema de script completo para qualquer componente com a mesma nacionalidade de primeira classe para um profissional de TI cuja API concede aos desenvolvedores. Na medida em que a computação baseada em nuvem torna-se onipresente, o Windows PowerShell também finalmente traz a capacidade de administrar remotamente um servidor, onde um computador sem qualquer interface gráfica tem as mesmas capacidades de gerenciamento de um computador com monitor e mouse.  
  
Um administrador veterano do AD DS deve considerar seu conhecimento anterior altamente relevante. Um administrador iniciante descobrirá uma curva de aprendizagem muito menor.  
  
## <a name="BKMK_TechOverview"></a>Visão geral técnica  
  
### <a name="what-you-should-know-before-you-begin"></a>O que você deve saber antes de iniciar  
Este tópico presume familiaridade com versões anteriores dos Serviços de Domínio Active Directory e não fornece detalhes conceituais sobre seu objetivo e funcionalidade. Para saber mais sobre o AD DS, veja as páginas do Portal TechNet no link abaixo:  
  
-   [Serviços de domínio do Active Directory para Windows Server 2008 R2](https://technet.microsoft.com/library/dd378801(WS.10).aspx)  
  
-   [Serviços de domínio do Active Directory para Windows Server 2008](https://technet.microsoft.com/library/dd378891(WS.10).aspx)  
  
-   [Referência técnica do Windows Server](https://technet.microsoft.com/library/cc739127(WS.10).aspx)  
  
### <a name="functional-descriptions"></a>Descrições funcionais  
  
#### <a name="ad-ds-role-installation"></a>Instalação da função AD DS  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_SelectServerRoles.gif)  
  
A instalação dos Serviços de Domínio Active Directory utiliza o Gerenciador do Servidor e o Windows PowerShell, assim como todas as outras funções e recursos do servidor no Windows Server 2012. O programa Dcpromo.exe não fornece mais opções de configuração da GUI.  
  
Você usa um assistente gráfico no Gerenciador do Servidor ou no módulo ServerManager do Windows PowerShell nas duas instalações, local e remota. Executando várias instâncias desses assistentes ou cmdlets e destinando vários servidores, você pode implantar o AD DS para vários controladores de domínios simultaneamente, tudo de um único console. Embora esses novos recursos não sejam retroativamente compatíveis com o Windows Server 2008 R2 ou sistemas operacionais anteriores, você ainda pode usar o aplicativo Dism.exe introduzido no Windows Server 2008 R2 para instalação da função local por uma linha de comandos clássica.  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSAddWindowsFeature.png)  
  
#### <a name="ad-ds-role-configuration"></a>Configuração da função do AD DS  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
Configuração de serviços de domínio Active ativa "anteriormente conhecida como DCPROMO" agora é uma uma operação discreta de instalação da função. Após instalar a função do AD DS, um administrador configura o servidor como um controlador de domínio usando um assistente separado dentro do Gerenciador do Servidor ou usando o módulo ADDSDeployment do Windows PowerShell.  
  
A configuração de função do AD DS desenvolvida em 12 anos de experiência de campo, agora configura controladores de domínio com base nas práticas recomendadas mais recentes da Microsoft. Por exemplo, Sistema de Nome de Domínio e Catálogos Globais são instalados por padrão em cada controlador de domínio.  
  
O Assistente de configuração do Gerenciador do servidor AD DS mescla muitas caixas de diálogo individuais em alguns poucos prompts e não mais oculta configurações em um modo "Avançado". Todo o processo de promoção é uma caixa de diálogo expandida durante a instalação. O assistente e o módulo ADDSDeployment do Windows PowerShell mostram a você mudanças notáveis e questões de segurança, com links para mais informações.  
  
O Dcpromo.exe permanece no Windows Server 2012 somente para instalações não monitoradas por linha de comandos e não executa mais o assistente de instalação gráfico. Ele é altamente recomendável que você descontinuar o uso do Dcpromo.exe para instalações autônomas e substituí-lo com o módulo ADDSDeployment, como o executável agora preterido não será incluído na próxima versão do Windows.  
  
Esses novos recursos não são mais compatíveis com versões anteriores dos sistemas operacionais Windows Server 2008 R2 ou mais antigos.  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSForest.png)  
  
> [!IMPORTANT]  
> Dcpromo.exe não contém mais um assistente gráfico e não instala mais binários de funções ou recursos. Ao tentar executar o Dcpromo.exe do shell do Explorer o seguinte é retornado:  
>   
> "O Assistente de instalação dos serviços de domínio Active Directory está realocado no Gerenciador do servidor. Para obter mais informações, consulte https://go.microsoft.com/fwlink/?LinkId=220921. "  
>   
> A tentativa de executar o Dcpromo.exe /unattend ainda instala os binários, como nos sistemas operacionais anteriores, mas avisa:  
>   
> "O dcpromo operação autônoma é substituída pelo módulo ADDSDeployment do Windows PowerShell. Para obter mais informações, consulte https://go.microsoft.com/fwlink/?LinkId=220924. "  
>   
> O Windows Server 2012 substituiu o dcpromo.exe e ele não será mais incluído em versões futuras do Windows, nem receberá novos aprimoramentos nesse sistema operacional. Os administradores devem descontinuar seu uso e mudar para os módulos do Windows PowerShell compatíveis se desejarem criar controladores de domínio usando a linha de comando.  
  
#### <a name="prerequisite-checking"></a>Verificação de pré-requisito  
A configuração do controlador de domínio também implementa uma fase de verificação de pré-requisitos que avalia a floresta e o domínio antes de continuar com a promoção do controlador de domínio. Isso inclui a disponibilidade da função FSMO, privilégios de usuário, compatibilidade de esquema estendido e outros requisitos. Esse novo design elimina problemas em que a promoção do controlador de domínio começa e depois trava no meio do caminho com um erro fatal de configuração. Isso diminui as oportunidades de metadados de controlador de domínio órfãos na floresta ou um servidor que se considera incorretamente que é um controlador de domínio.  
  
## <a name="BKMK_SMForest"></a>Implantando uma floresta com o Gerenciador do servidor  
Esta seção explica como instalar o primeiro controlador de domínio em um domínio raiz de floresta, usando o Gerenciador do Servidor em um computador gráfico do Windows Server 2012.  
  
### <a name="server-manager-ad-ds-role-installation-process"></a>Processo de instalação da função do Server Manager AD DS  
O diagrama abaixo ilustra o processo de instalação da função dos Serviços de Domínio Active Directory, começando com a execução do ServerManager.exe e finalizando bem antes da promoção do controlador de domínio.  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment.png)  
  
#### <a name="server-pool-and-add-roles"></a>Pool do Servidores e Adicionar Funções  
Qualquer computador com Windows Server 2012 acessível de um computador que executa o Gerenciador do Servidor é elegível ao pooling. Depois de reunidos, você pode selecionar os servidores para instalação remota do AD DS ou qualquer outra opção de configuração do Gerenciador do Servidor.  
  
Para adicionar servidores, escolha uma das seguintes opções:  
  
-   Clique em **Adicionar Outros Servidores para Gerenciar** no painel de boas-vindas  
  
-   Clique no menu **Gerenciar** e selecione **Adicionar Servidores**  
  
-   Clique com o botão direito do mouse em **Todos os Servidores** e escolha **Adicionar Servidores**  
  
Isso abra a caixa de diálogo Adicionar Servidores:  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddServers.png)  
  
Isso lhe oferece três maneiras de adicionar servidores ao pool para uso ou agrupamento:  
  
-   A pesquisa do Active Directory (utiliza LDAP, requer que os computadores pertençam a um domínio, permite a filtragem do sistema operacional e dá suporte a caracteres curinga)  
  
-   Pesquisa DNS (usa alias DNS ou endereço IP via transmissão ARP ou NetBIOS ou pesquisa WINS, não permite filtragem de sistema operacional, nem dá suporte a coringas)  
  
-   Importação (usa uma lista de arquivos de texto de servidores separados por CR/LF)  
  
Clique em **Localizar Agora** para retornar uma lista de servidores desse mesmo domínio Active Directory ao qual o computador se integrou, clique em um ou mais nomes de servidor da lista de servidores. Clique na seta para a direita para adicionar os servidores à lista **Selecionados**. Use a caixa de diálogo **Adicionar Servidores** para adicionar servidores selecionados aos grupos de função do painel. Ou clique em **Gerenciar** e em **Criar Grupo de Servidores**, ou clique em **Criar Grupo de Servidores** no painel **Bem-vindo ao Gerenciador do Servidor** para criar grupos de servidores personalizados.  
  
> [!NOTE]  
> O procedimento Adicionar Servidores não confirma se um servidor está online ou é acessível. Entretanto, todos os servidores inacessíveis serão sinalizados no modo de exibição Capacidade de Gerenciamento do Gerenciador do Servidor, na próxima atualização.  
  
Você pode instalar funções remotamente em qualquer computador do Windows Server 2012 adicionado ao pool, conforme mostrado:  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/tADDS_SMI_TR_AddRolesFeatures.png)  
  
Não pode gerenciar integralmente servidores que executam sistemas operacionais anteriores ao Windows Server 2012. A seleção **Adicionar Funções e Recursos** está executando o módulo ServerManager do Windows PowerShell **Install-WindowsFeature**.  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddADDSToAnotherServer.png)  
  
Você também pode usar o Painel do Gerenciador do Servidor em um controlador de domínio existente para selecionar a instalação do AD DS do servidor remoto com a função já pré-selecionada, clicando com o botão direito do mouse e selecionando o bloco do painel AD DS e selecionando **Adicionar AD DS a Outro Servidor**. O **Install-WindowsFeature AD-Domain-Services** está sendo invocado.  
  
O computador em que você está executando o Gerenciador do Servidor entra em pool automaticamente. Para instalar a função AD DS aqui, simplesmente clique no menu **Gerenciar** e clique em **Adicionar Funções e Recursos**.  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ManageAddRoles.png)  
  
#### <a name="installation-type"></a>Tipo de instalação  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectInstallationType.png)  
  
A caixa de diálogo **Tipo de Instalação** fornece uma opção que não dá suporte para Serviços de Domínio Active Directory: a **Instalação baseada em cenário de Serviços de Área de Trabalho Remota**. Essa opção permite apenas Serviço de Área de Trabalho Remota em uma carga de trabalho distribuída em vários servidores. Se você selecioná-la, o, AD DS não poderá instalar.  
  
Sempre deixe a seleção padrão marcada quando instalar o AD DS: **Instalação baseada em função ou recurso**.  
  
#### <a name="server-selection"></a>Seleção de servidor  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectDestinationServer.png)  
  
O diálogo **Seleção do Servidor** permite que você escolha um dos servidores previamente adicionados ao pool, desde que ele esteja acessível. O servidor local que executa o Gerenciador do Servidor está disponível automaticamente.  
  
Além disso, você pode selecionar arquivos VHD do Hyper-V offline com o sistema operacional Windows Server 2012, e o Gerenciador do Servidor adiciona a função a eles diretamente por meio de instalação de componentes. Isso permite a você provisionar servidores virtuais com os componentes necessários antes de configurá-los melhor.  
  
#### <a name="server-roles-and-features"></a>Funções e recursos do servidor  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectServerRoles.png)  
  
Selecione a função **Serviços de Domínio Active Directory** se pretende promover um controlador de domínio. Todos os recursos de administração do Active Directory e serviços necessários são instalados automaticamente, mesmo se foram parte de outra função ou se não aparecerem selecionados na interface do Gerenciador do Servidor.  
  
O Gerenciador do Servidor também apresenta uma caixa de diálogo informativa que mostra quais recursos de gerenciamento essa função instala implicitamente; isso se equivale ao argumento **-IncludeManagementTools**.  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddFeaturesDialog.gif)  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectFeatures.png)  
  
**Recursos** adicionais podem ser incluídos aqui, conforme desejados.  
  
#### <a name="active-directory-domain-services"></a>Active Directory Domain Services  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSIntro.png)  
  
A caixa de diálogo **Serviços de Domínio Active Directory** fornece informações limitadas sobre requisitos e práticas recomendadas. Isso serve principalmente como uma confirmação de que você escolheu a função AD DS "se esta tela não aparecer, você não selecionou AD DS.  
  
#### <a name="confirmation"></a>Confirmação  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Confirmation.png)  
  
A caixa de diálogo **Confirmação** é o ponto de verificação final antes da instalação da função iniciar. Ela apresenta uma opção para reiniciar o computador como necessária após a instalação da função, mas a instalação do AD DS não requer uma reinicialização.  
  
Clicando em **Instalar**, você confirma que está pronto para começar a instalação da função. Você não pode cancelar a instalação de uma função depois de iniciá-la.  
  
#### <a name="results"></a>Resultados  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Results.png)  
  
A caixa de diálogo **Resultados** mostra o progresso atual da instalação e o status da instalação atual. A instalação da função continua, independentemente de ser fechado o Gerenciador do Servidor.  
  
Verificar os resultados da instalação ainda é uma melhor prática. Se você fechar a caixa de diálogo **Resultados** antes de concluir a instalação, poderá verificar os resultados usando o sinalizador de notificação do Gerenciador do Servidor. O Gerenciador do Servidor também mostra uma mensagem de aviso para quaisquer servidores que tenham instalado a função AD DS, mas que ainda não foram configurados como controladores de domínio.  
  
**Notificações da tarefa**  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskNotofications.png)  
  
**Detalhes do AD DS**  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSDetails.png)  
  
**Detalhes da tarefa**  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskDetails.png)  
  
#### <a name="promote-to-domain-controller"></a>Promover o controlador de domínio  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Promote.png)  
  
No final da instalação da função AD DS, você pode continuar com a configuração usando o link **Promover este servidor a um controlador de domínio** . Isso é necessário para tornar o servidor um controlador de domínio, mas não é necessário para executar o assistente de configuração imediatamente. Por exemplo, você pode desejar somente provisionar servidores com binários do AD DS antes de enviá-los para outras filiais para configuração posterior. Adicionar a função AD DS antes da remessa, permite que você ganhe tempo quando eles chegarem em seu destino. Você também pode seguir as melhores práticas de não manter um controlador de domínio offline durante dias ou semanas. Finalmente, isso permite que você atualize componentes antes da promoção do controlador de domínio, evitando pelo menos uma reinicialização posterior.  
  
Selecionar esse link posteriormente invoca os cmdlets ADDSDeployment: **install-addsforest**, **install-addsdomain**ou **install-addsdomaincontroller**.  
  
### <a name="uninstallingdisabling"></a>Desinstalando/Desabilitando  
Você remove a função AD DS como qualquer outra função, independentemente se você promoveu o servidor para um controlador de domínio. Entretanto, ao remover a função AD DS é necessário reiniciar para concluir.  
  
A remoção da função dos Serviços de Domínio Active Directory é diferente da instalação, na qual requer o rebaixamento do controlador de domínio para ser concluída. Isso é necessário para evitar que um controlador de domínio tenha seus binários de função desinstalados sem a limpeza apropriada dos metadados na floresta. Para obter mais informações, consulte [rebaixar controladores de domínio e domínios de &#40;nível 200&#41;](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
> [!WARNING]  
> Remover as funções AD DS com Dism.exe ou o módulo DISM do Windows PowerShell após a promoção para um Controlador de Domínio não é possível e impedirá que o servidor seja reinicializado normalmente.  
>   
> Diferente do Gerenciador do Servidor ou do módulo de Implantação do AD DS para Windows PowerShell, o DISM é um sistema de instalação nativo que não possui conhecimento inerente de AD DS ou de sua configuração. Não use Dism.exe ou o módulo do DISM do Windows PowerShell para desinstalar a função AD DS, a menos que o servidor não seja mais um controlador de domínio.  
  
### <a name="create-an-ad-ds-forest-root-domain-with-server-manager"></a>Criar um domínio raiz de floresta do AD DS com o Gerenciador do Servidor  
O diagrama a seguir ilustra o processo de configuração dos Serviços de Domínio Active Directory, no caso em que você instalou previamente a função AD DS e iniciou o **Assistente de Configuração dos Serviços de Domínio Active Directory** usando o Gerenciador do Servidor.  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_forestdeploy2.png)  
  
#### <a name="deployment-configuration"></a>Configuração de Implantação  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddNewForest.png)  
  
O Gerenciador do Servidor começa toda a promoção do controlador de domínio com a página **Configuração de Implantação** . As demais opções e campos exigidos mudam nessa página e nas páginas subsequentes, dependendo da operação de implantação selecionada.  
  
Para criar uma nova floresta do Active Directory, clique em **Adicionar uma nova floresta**. Você deve fornecer um nome de domínio raiz válido; o nome não pode conter apenas uma palavra (por exemplo, o nome deve ter *contoso.com* ou similar e não apenas *contoso*) e deve usar requisitos de nomenclatura de domínio DNS permitidos.  
  
Para obter mais informações sobre nomes de domínio válidos, veja o artigo KB [Convenções de nomenclatura no Active Directory para computadores, domínios, sites e OUs](https://support.microsoft.com/kb/909264).  
  
> [!WARNING]  
> Não cria novas florestas do Active Directory com o mesmo nome que um nome DNS externo. Por exemplo, se a URL de DNS de Internet for http://contoso.com, você deve escolher um nome diferente para sua floresta interna evitar futuros problemas de compatibilidade. Esse nome deve ser exclusivo e de uso improvável no tráfego da Web. Por exemplo: corp.contoso.com.  
  
Uma nova floresta não precisa de novas credenciais para a conta de Administrador do domínio. O processo de promoção do controlador de domínio utiliza as credenciais da conta do Administrador interna do primeiro controlador de domínio usado para criar a raiz da floresta. Não há maneira (por padrão) de desabilitar ou bloquear a conta de Administrador interna e essa pode ser a única entrada em uma floresta se as outras contas de domínio administrativas ficarem inutilizáveis. É fundamental saber a senha antes de implantar uma nova floresta.  
  
**DomainName** requer um nome DNS de domínio totalmente qualificado válido.  
  
#### <a name="domain-controller-options"></a>Opções de controlador de domínio  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DCOptions_Forest.gif)  
  
As **Opções de Controlador de Domínio** permitem que você configure o **nível funcional da floresta** e **nível funcional do domínio** para o novo domínio raiz da floresta. Por padrão, essas configurações são o Windows Server 2012 em um domínio de raiz da nova floresta. O nível funcional de floresta do Windows Server 2012 não fornece nenhuma nova funcionalidade sobre o nível funcional de floresta do Windows Server 2008 R2. O nível funcional de domínio do Windows Server 2012 é necessário somente para implementar as novas configurações de Kerberos "sempre fornecer declarações" e "recusar solicitações de autenticação." Um uso primordial dos níveis no Windows Server 2012 é restringir a participação no domínio para controladores de domínio que atendem aos requisitos do sistema operacional mínimos permitidos. Em outras palavras, você pode especificar o Windows Server 2012 domínio funcional nível somente controladores de domínio que executam o Windows Server 2012 podem hospedar o domínio.  Windows Server 2012 implementa um novo sinalizador do controlador de domínio chamado **DS_WIN8_REQUIRED** na **DSGetDcName** função de NetLogon que localiza exclusivamente os controladores de domínio do Windows Server 2012. Isso permite a você a flexibilidade de uma floresta mais homogênea ou mais heterogênea em termos de quais sistemas operacionais têm permissão de executar em controladores de domínio.  
  
Para obter mais informações sobre a Localização do controlador de domínio, reveja [Funções de serviço de diretório](https://msdn.microsoft.com/library/ms675900(VS.85).aspx).  
  
O único recurso do controlador de domínio configurável é a opção do servidor DNS. A Microsoft recomenda que todos os controladores de domínio forneçam serviços DNS de alta disponibilidade em ambientes distribuídos, é por isso que essa opção é selecionada por padrão ao instalar um controlador de domínio em qualquer modo ou domínio. As opções de Catálogo Global e controlador de domínio somente leitura estão indisponíveis quando se cria um novo domínio raiz de floresta; o primeiro controlador de domínio deve ser um GC, e não pode ser RODC (Controlador de Domínio Somente Leitura).  
  
A **Senha do Modo de Restauração dos Serviços de Diretório** deve atender à política de senha aplicada ao servidor, que por padrão não requer uma senha forte; somente uma que não esteja em branco. Escolha sempre uma senha forte e complexa.  
  
#### <a name="dns-options-and-dns-delegation-credentials"></a>Opções de DNS e credenciais de delegação de DNS  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestDNSOptions.png)  
  
A página **Opções de DNS** permite que você configure a delegação DNS e forneça credenciais administrativas de DNS alternativas.  
  
Não é possível configurar opções DNS ou delegação no Assistente de Configuração de Serviços de Domínio Active Directory quando instalar um novo Domínio Raiz de Floresta do Active Directory em que você selecionou **Servidor DNS** na página **Opções de Controlador de Domínio**. A opção **Criar delegação DNS** está disponível quando criar uma nova zona DNS de raiz de floresta em uma infraestrutura de servidor DNS existente. Essa opção habilita você a fornecer credenciais administrativas DNS alternativas que possuem direitos para atualizar a zona DNS.  
  
Para saber se você precisa criar uma delegação de DNS, consulte [Noções básicas de delegação de zona](https://technet.microsoft.com/library/cc771640.aspx).  
  
#### <a name="additional-options"></a>Opções adicionais  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestAdditionalOptions.png)  
  
A página **Opções Adicionais** mostra o nome NetBIOS do domínio e permite que você o substitua. Por padrão, o nome de domínio NetBIOS corresponde ao rótulo mais à esquerda do nome de domínio totalmente qualificado fornecido na página **Configuração de Implantação** . Por exemplo, se você forneceu o nome de domínio totalmente qualificado corp.contoso.com, o nome de domínio padrão do NetBIOS é CORP.  
  
Se o nome tiver 15 caracteres ou menos e não entrar em conflito com outro nome NetBIOS, ele ficará inalterado. Se ocorrer conflito com outro nome NetBIOS, um número será acrescentado ao nome. Se o nome tiver mais de 15 caracteres, o assistente fornecerá uma sugestão truncada exclusiva. Em ambos os casos, o assistente primeiro valida o nome que ainda não está em uso por meio de uma consulta WINS e difusão de NetBIOS.  
  
Para obter mais informações sobre nomes de domínio válidos, veja o artigo KB [Convenções de nomenclatura no Active Directory para computadores, domínios, sites e OUs](https://support.microsoft.com/kb/909264).  
  
#### <a name="paths"></a>Caminhos  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPaths.png)  
  
A página **Rotas** permite substituir os locais de pasta padrão do banco de dados AD DS, os logs de transação de banco de dados e o compartilhamento SYSVOL. Os locais padrão estão sempre em subdiretórios do %systemroot% (ou seja, C:\Windows).  
  
#### <a name="review-options-and-view-script"></a>Examinar opções e exibir script  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestReviewOptions.png)  
  
A página **Examinar Opções** permite que você valide suas configurações e verifique se elas cumprem os requisitos, antes de iniciar a instalação. Esta não é a última oportunidade de interromper a instalação ao usar o Gerenciador do Servidor. Essa página simplesmente permite que você confirme suas configurações antes de continuar a configuração.  
  
A página **Opções de revisão** do Gerenciador do Servidor também oferece um botão **Exibir Script** para criar um arquivo de texto Unicode contendo a configuração atual de ADDSDeployment como um script simples do Windows PowerShell. Isso permite que você use a interface gráfica do Gerenciador do Servidor como um estúdio de implantação do Windows PowerShell. Use o Assistente de Configuração dos Serviços de Domínio do Active Directory para configurar opções, exportar a configuração e então cancelar o assistente. Esse processo cria um exemplo válido e sintaticamente correto para modificações adicionais ou uso direto. Por exemplo:   
  
```powershell 
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSForest `  
-CreateDNSDelegation `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainMode "Win2012" `  
-DomainName "corp.contoso.com" `  
-DomainNetBIOSName "CORP" `  
-ForestMode "Win2012" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-NoRebootOnCompletion:$false `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> O Gerenciador do Servidor geralmente preenche todos os argumentos com valores quando promove e não depende de padrões (já que eles podem ser alterados entre versões futuras do Windows ou service packs). Uma exceção a isso é o argumento **-safemodeadministratorpassword** (que é deliberadamente omitido do script). Para forçar um prompt de confirmação, omita o valor ao executar o cmdlet interativamente.  
  
#### <a name="prerequisites-check"></a>Verificação de pré-requisitos  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPrereqCheck.png)  
  
A **Verificação de Pré-requisitos** é um novo recurso na configuração do domínio AD DS. Essa nova fase confirma que a configuração do servidor é capaz de dar suporte a uma nova floresta AD DS.  
  
Ao instalar um novo domínio raiz da floresta, o Assistente de Configuração dos Serviços de Domínio Active Directory do Gerenciador do Servidor invoca uma série de testes modulares. Esses testes o alertam com opções de reparo sugeridas. Você pode executar os testes quantas vezes forem necessárias. O processo do controlador de domínio não pode continuar até que todos os testes de pré-requisitos sejam feitos.  
  
A **Verificação de Pré-requisitos** também dá superfície a informações relevantes, como alterações de segurança que afetam os sistemas operacionais mais antigos.  
  
Para mais informações sobre as verificações de pré-requisitos, consulte [Prerequisite Checking](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
#### <a name="installation"></a>Instalação  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestInstallation.png)  
  
Quando a página **Instalação** é exibida, a configuração do controlador de domínio começa e não pode ser interrompida ou cancelada. Operações detalhadas são exibidas nesta página e gravadas em logs:  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
> [!NOTE]  
> É possível executar vários assistentes de instalação de função e configuração do AD DS no mesmo console do Gerenciador do Servidor simultaneamente.  
  
#### <a name="results"></a>Resultados  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
A página **Resultados** mostra o sucesso ou o fracasso da promoção e qualquer informação administrativa importante. O controlador de domínio reiniciará automaticamente após 10 segundos.  
  
## <a name="BKMK_PSForest"></a>Implantando uma floresta com o Windows PowerShell  
Esta seção explica como instalar o primeiro controlador de domínio em um domínio raiz de floresta, usando o Windows PowerShell em um computador Core Windows Server 2012.  
  
### <a name="windows-powershell-ad-ds-role-installation-process"></a>Processo de instalação da função AD DS do Windows PowerShell  
Implementando alguns cmdlets de implantação diretos do ServerManager em seus processos de implantação, você obtém ainda a visão de administração simplificada AD DS.  
  
A figura a seguir ilustra o processo de instalação da função dos Serviços de Domínio Active Directory, começando com a execução do **PowerShell.exe** e finalizando bem antes da promoção do controlador de domínio.  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment_powershell.png)  
  
|||  
|-|-|  
|Cmdlet ServerManager|Argumentos (os argumentos em **Negrito** são necessários. Os argumentos em*Itálico* podem ser especificados usando o Windows PowerShell ou o Assistente de Configuração do AD DS.)|  
|Install-WindowsFeature/Add-WindowsFeature|***-Name***<br /><br />*-Restart*<br /><br />*-IncludeAllSubFeature*<br /><br />*-IncludeManagementTools*<br /><br />-Source<br /><br />*-ComputerName*<br /><br />-Credential<br /><br />-LogPath<br /><br />*-Vhd*<br /><br />*-ConfigurationFilePath*|  
  
> [!NOTE]  
> Embora não necessário, o argumento **-IncludeManagementTools** é altamente recomendado ao instalar binários de função AD DS  
  
O módulo ServerManager expõe a instalação da função, status e partes de remoção do novo módulo DISM para Windows PowerShell. Esse sistema em camadas simplifica a maior parte das tarefas e reduz a necessidade de uso direto do poderoso (mas perigoso quando usado de forma incorreta) módulo DISM.  
  
Use **Get-Command** para expor os aliases e cmdlets em ServerManager.  
  
```powershell  
Get-Command -module ServerManager  
```  
  
Por exemplo:  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetCommand.png)  
  
Para adicionar a função de Serviços de Domínio Active Directory, simplesmente execute **Install-WindowsFeature** com o nome de função AD DS como um argumento. Como o Gerenciador do Servidor, todos os serviços necessários estão implícitos na instalação da função AD DS automaticamente.  
  
```powershell  
Install-WindowsFeature -name AD-Domain-Services  
```  
  
Se você deseja que as ferramentas de gerenciamento do AD DS sejam instaladas - e isso é muito recomendado - forneça o argumento **-IncludeManagementTools** :  
  
```powershell  
Install-WindowsFeature -name AD-Domain-Services -IncludeManagementTools  
```  
  
Por exemplo:  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallWinFeature.png)  
  
Para obter uma lista de todos os recursos e funções com seu status de instalação, use **Get-WindowsFeature** sem argumentos. Especifique o argumento **-ComputerName** para o status de instalação de um servidor remoto.  
  
```powershell  
Get-WindowsFeature  
```  
  
Como **Get-WindowsFeature** não possui um mecanismo de filtragem, você deve usar **Where-Object** com um pipeline para encontrar recursos específicos. O pipeline é um canal usado entre vários cmdlets para passar dados e o cmdlet Where-Object atua como um filtro. A variável **$_** embutida atua como uma passagem do objeto atual pelo pipeline com qualquer propriedade que ela possa conter.  
  
```powershell  
Get-WindowsFeature | where-object <options>  
```  
  
Por exemplo, para localizar todos os recursos que contêm "Active Dir" em sua propriedade **Nome de Exibição** , use:  
  
```powershell  
Get-WindowsFeature | where displayname -like "*active dir*"  
```  
  
Mais exemplos estão ilustrados abaixo:  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetWindowsFeature.png)  
  
Para obter mais informações sobre mais operações do Windows PowerShell com pipelines e Where-Object, consulte [Piping and the Pipeline in Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).  
  
Observe também que o Windows PowerShell 3.0 simplificou significativamente os argumentos de linha de comando necessários nessa operação de pipeline. O Windows PowerShell 2.0 teria exigido:  
  
```powershell  
Get-WindowsFeature | where {$_.displayname - like "*active dir*"}  
```  
  
Usando o pipeline do Windows PowerShell, você pode criar resultados legíveis. Por exemplo:  
  
```powershell  
Install-WindowsFeature | Format-List  
Install-WindowsFeature | select-object | Format-List  
  
```  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDS.png)  
  
Observe como usar o cmdlet **Select-Object** com o argumento **-expandproperty** para retornar dados interessantes:  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSWithTools.png)  
  
> [!NOTE]  
> O argumento **Select-Object -expandproperty** diminui ligeiramente o desempenho geral da instalação.  
  
### <a name="BKMK_PS"></a>Criar um domínio de raiz de floresta do AD DS com o Windows PowerShell  
Para instalar uma nova floresta do Active Directory utilizando o módulo ADDSDeployment, use o seguinte cmdlet:  
  
```powershell  
Install-addsforest  
```  
  
O cmdlet **Install-AddsForest** só tem duas fases (verificação e instalação de pré-requisito). As duas figuras abaixo mostram a fase de instalação com os argumentos mínimos exigidos de **-domainname**.  
  
|||  
|-|-|  
|Cmdlet ADDSDeployment|Argumentos (os argumentos em **Negrito** são necessários. Os argumentos em*Itálico* podem ser especificados usando o Windows PowerShell ou o Assistente de Configuração do AD DS.)|  
|install-addsforest|-Confirm<br /><br />*-CreateDNSDelegation*<br /><br />*-DatabasePath*<br /><br />*-DomainMode*<br /><br />***-DomainName***<br /><br />***-DomainNetBIOSName***<br /><br />*-DNSDelegationCredential*<br /><br />*-ForestMode*<br /><br />-Force<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-NoDnsOnNetwork<br /><br />-NoRebootOnCompletion<br /><br />*-SafeModeAdministratorPassword*<br /><br />-SkipAutoConfigureDNS<br /><br />-SkipPreChecks<br /><br />*-SYSVOLPath*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> O argumento **-DomainNetBIOSName** será exigido se você quiser alterar o nome de 15 caracteres gerado automaticamente com base no prefixo de nome de domínio DNS ou se o nome exceder 15 caracteres.  
  
O cmdlet ADDSDeployment de **Configuração de Implantação** e os argumentos equivalentes do Gerenciador do Servidor são:  
  
```powershell  
Install-ADDSForest  
-DomainName <string>  
```  
  
Os argumentos do cmdlet ADDSDeployment das Opções de Controlador de Domínio do Gerenciador do Servidor equivalentes são:  
  
```powershell  
-ForestMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-InstallDNS <{$false | $true}>  
-SafeModeAdministratorPassword <secure string>  
  
```  
  
Os argumentos **Install-ADDSForest** seguirão os mesmos padrões do Gerenciador do Servidor, se não forem especificados.  
  
A operação do argumento **SafeModeAdministratorPassword** é especial:  
  
-   Se *nenhum argumento for especificado* , o cmdlet solicitará que você insira e confirme uma senha mascarada. Este é o uso preferencial ao executar o cmdlet interativamente.  
  
    Por exemplo, para criar uma nova floresta denominada corp.contoso.com e ser solicitado a digitar e confirmar uma senha mascarada:  
  
    ```powershell  
    Install-ADDSForest "DomainName corp.contoso.com  
    ```  
  
-   Se especificado *com um valor*, esse valor deverá ser uma cadeia de caracteres segura. Este não é o uso preferencial ao executar o cmdlet interativamente.  
  
Por exemplo, você pode solicitar manualmente uma senha usando o cmdlet **Read-Host** para solicitar ao usuário uma cadeia de caracteres segura:  
  
```powershell  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
```  
  
> [!WARNING]  
> Como a opção anterior não confirma a senha, seja extremamente cuidadoso: a senha não fica visível.  
  
Você também pode fornecer uma cadeia de caracteres segura como uma variável de texto não criptografado convertida, embora seja altamente recomendável não fazer isso.  
  
```powershell  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
```  
  
Finalmente, você pode armazenar a senha ofuscada em um arquivo e depois reutilizá-la mais tarde, sem a senha com texto não criptografado aparecendo. Por exemplo:  
  
```powershell  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> O fornecimento ou o armazenamento de uma senha com texto não criptografado ou ofuscado não é recomendável. Qualquer pessoa que executar esse comando em um script ou que estiver por perto tomará conhecimento da senha DSRM desse controlador de domínio. Qualquer pessoa com acesso ao arquivo pode reverter essa senha ofuscada. Com esse conhecimento, ela pode fazer logon em um DC iniciado em DSRM e, eventualmente, representar o próprio controlador de domínio, elevando seus privilégios ao nível mais alto em uma floresta Active Directory. Um conjunto adicional de etapas, usando **System.Security.Cryptography** para criptografar os dados do arquivo de texto é aconselhável, mas fora do escopo. A melhor prática é evitar totalmente o armazenamento de senha.  
  
O cmdlet ADDSDeployment oferece uma opção adicional para pular a configuração automática das configurações de cliente DNS, encaminhadores e dicas de raiz. Não é possível pular essa opção de configuração quando usar o Gerenciador do Servidor. Esse argumento aplica-se somente se você instalou a função do Servidor DNS antes de configurar o controlador de domínio:  
  
```powershell  
-SkipAutoConfigureDNS  
```  
  
A operação **DomainNetBIOSName** também é especial:  
  
-   Se o argumento **DomainNetBIOSName** não for especificado com um nome de domínio NetBIOS e o nome de domínio com prefixo de rótulo único no argumento **DomainName** for de15 caracteres ou menos, a promoção continuará com um nome gerado automaticamente.  
  
-   Se o argumento **DomainNetBIOSName** não for especificado com um nome de domínio NetBIOS e o nome de domínio de prefixo de rótulo único no argumento **DomainName** for de 16 caracteres ou mais, a promoção falhará.  
  
-   Se o argumento **DomainNetBIOSName** for especificado com um nome de domínio do NetBIOS de 15 caracteres ou menos, a promoção continuará com o nome especificado.  
  
-   Se o argumento **DomainNetBIOSName** for especificado com um nome de domínio do NetBIOS de 16 caracteres ou mais, a promoção falhará.  
  
O argumento de cmdlet ADDSDeployment das Opções Adicionais do Gerenciador do Servidor equivalente é:  
  
```powershell  
-domainnetbiosname <string>  
```  
  
Os argumentos de cmdlet ADDSDeployment de **Caminhos** do Gerenciador do Servidor equivalentes são:  
  
```powershell  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
  
```  
  
Use o argumento **Whatif** opcional com o cmdlet **Install-ADDSForest** para examinar as informações da configuração. Isso permite que você veja os valores explícitos e implícitos de argumento de um cmdlet.  
  
Por exemplo:  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSPaths.png)  
  
Não é possível ignorar a **Verificação de Pré-requisitos** ao usar o Gerenciador do Servidor, mas você pode ignorar o processo ao usar o cmdlet de Implantação do AD DS com o seguinte argumento:  
  
```powershell  
-skipprechecks  
```  
  
> [!WARNING]  
> A Microsoft desencoraja ignorar a verificação de pré-requisito, pois isso pode levar a uma promoção parcial do controlador de domínio ou danificar a floresta AD DS.  
  
Observe que, assim como o Gerenciador do Servidor, o **Install-ADDSForest** lembra você de que a promoção reiniciará o servidor automaticamente.  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSReboot.png)  
  
![Instalar uma nova floresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallProgress.png)  
  
Para aceitar o prompt de reinicialização automática, use os argumentos **-force** ou **-confirm:$false** com qualquer cmdlet ADDSDeployment do Windows PowerShell. Para evitar que o servidor reinicie automaticamente no final da promoção, use o argumento **-norebootoncompletion** .  
  
> [!WARNING]  
> Não é recomendável substituir a reinicialização. O controlador de domínio deve reiniciar para funcionar corretamente.  
  
## <a name="see-also"></a>Consulte também  
[Serviços de domínio do Active Directory (Portal TechNet)](https://technet.microsoft.com/library/cc770946(WS.10).aspx)  
[Serviços de domínio do Active Directory para Windows Server 2008 R2](https://technet.microsoft.com/library/dd378801(WS.10).aspx)  
[Serviços de domínio do Active Directory para Windows Server 2008](https://technet.microsoft.com/library/dd378891(WS.10).aspx)  
[Referência técnica do Windows Server (Windows Server 2003)](https://technet.microsoft.com/library/cc739127(WS.10).aspx)  
[Centro Administrativo do Active Directory: Guia de Introdução (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd560651(WS.10).aspx)  
[Administração do Active Directory com o Windows PowerShell (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd378937(WS.10).aspx)  
[Pergunte à equipe de serviços de diretório (Blog de suporte técnico oficial comercial da Microsoft)](http://blogs.technet.com/b/askds)  
  

