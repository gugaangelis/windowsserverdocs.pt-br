---
title: "Mover dados e configurações do Windows SBS 2003 para a migração do servidor de destino para o Windows Server Essentials"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67087ccb-d820-4642-8ca2-7d2d38714014
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 74375d65845a7a5e9c2d6752bdee49993b8cc791
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="move-windows-sbs-2003-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Mover dados e configurações do Windows SBS 2003 para a migração do servidor de destino para o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Mova as configurações e dados ao servidor de destino, da seguinte maneira:

1.  [Copiar dados ao servidor de destino](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
2.  [Importar contas de usuário do Active Directory para Windows Server Essentials Dashboard (opcional)](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_ImportADaccounts)  
  
3.  [Remover o antigo scripts de logon (opcionais)](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_RemoveScripts)  
  
4.  [Remover herdados Active Directory Group Policy Objects (opcional)](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_RemoveLegacyADGPO)  
  
5.  [Configurar a rede](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Configure)  
  
6.  [Mapear computadores permitidos para contas de usuário](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
 
##  <a name="BKMK_CopyData"></a>Copiar dados ao servidor de destino  
 Antes de copiar dados do servidor de origem para o servidor de destino, execute as seguintes tarefas:  
  
-   Examine a lista de pastas compartilhadas no servidor de origem, incluindo as permissões para cada pasta. Criar ou personalizar as pastas no servidor de destino para corresponder a estrutura de pastas que você está migrando do servidor de origem.  
  
-   Analise o tamanho de cada pasta e verifique se o servidor de destino tem espaço de armazenamento suficiente.  
  
-   Verifique as pastas compartilhadas no servidor de origem Read-only para todos os usuários não escrita pode carregar colocar na unidade enquanto você estiver copiando arquivos para o servidor de destino.  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Para copiar dados do servidor de origem para o servidor de destino  
  
1.  Faça logon no servidor de destino como um administrador de domínio.  
  
2.  Clique em **iniciar**, tipo **cmd** na caixa de pesquisa e pressione ENTER.  
  
3.  No prompt de comando, digite o seguinte comando e pressione ENTER:  
  
    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`  
  
     Onde:
     - \ < SourceServerName\ > é o nome do servidor de origem
     - \ < SharedSourceFolderName\ > é o nome da pasta compartilhada no servidor de origem
     - \ < DestinationServerName\ > é o nome do servidor de destino,
     - \ < SharedDestinationFolderName\ > é a pasta compartilhada no servidor de destino para o qual os dados serão copiados.  
 
4.  Repita a etapa anterior para cada pasta compartilhada que você está migrando do servidor de origem.  
  
##  <a name="BKMK_ImportADaccounts"></a>Importar contas de usuário do Active Directory para Windows Server Essentials Dashboard (opcional)  
 Por padrão, todas as contas de usuário criadas no servidor de origem são migradas automaticamente para o painel no Windows Server Essentials. No entanto, a migração automática de uma conta de usuário no Active Directory falhará se algumas propriedades não atender aos requisitos de migração. Você pode usar o seguinte cmdlet do Windows PowerShell para importar os usuários do Active Directory.  
  
#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>Para importar uma conta de usuário do Active Directory para o Windows Server Essentials Dashboard  
  
1.  Faça logon no servidor de destino como um administrador de domínio.  
  
2.  Abra o Windows PowerShell como administrador.  
  
3.  Execute o seguinte cmdlet, onde `[AD username]`é o nome da conta de usuário do Active Directory que você deseja importar:  
  
     `Import-WssUser SamAccountName [AD username]`  
  
##  <a name="BKMK_RemoveScripts"></a>Remover o antigo scripts de logon (opcionais)  
 Windows SBS 2003 usa scripts de logon para tarefas como instalar o software e personalizar áreas de trabalho.  Windows Server Essentials substitui os scripts de logon do Windows SBS 2003 com uma combinação de scripts de logon e objetos de política de grupo.  
  
> [!NOTE]
>  Se você tiver modificado os scripts de logon do Windows SBS 2003, você deve renomear os scripts para preservar suas personalizações.  
  
> [!NOTE]
>  Scripts de logon do Windows SBS 2003 se aplicam somente às contas de usuário que foram adicionadas usando o **o Assistente para adicionar novos usuários**.  
  
#### <a name="to-remove-the-windows-sbs-2003-logon-scripts"></a>Para remover os scripts de logon do Windows SBS 2003  
  
1.  Clique em **iniciar**, aponte para **ferramentas administrativas**e clique em **usuários e Active Directory computadores**.  
  
2.  Em **usuários e Active Directory computadores**, expanda sua rede e, em seguida, clique em **usuários**.  
  
3.  Clique com botão direito a um nome de usuário, clique em **propriedades**e, em seguida, clique no **perfil** guia.  
  
4.  Excluir o conteúdo do **script de Logon** caixa de texto e clique em **Okey**.  
  
5.  Repita as etapas 3 e 4 para cada usuário.  
  
##  <a name="BKMK_RemoveLegacyADGPO"></a>Remover herdados Active Directory Group Policy Objects (opcional)  
 Os objetos de política de grupo (GPOs) são atualizados para o Windows Server Essentials. Eles são um subconjunto dos GPOs Windows SBS 2003. Para o Windows Server Essentials, um número filtros de GPOs do Windows SBS 2003 e Windows Management Instrumentation (WMI) deve ser excluído manualmente para evitar conflitos com os filtros do Windows Server Essentials GPOs e WMI.  
  
> [!NOTE]
>  Se modificado original Windows SBS 2003 Group Policy Objects, você deve salvar cópias em um local diferente e, em seguida, exclua-os do Windows SBS 2003.  
  
#### <a name="to-remove-old-group-policy-objects-from-windows-sbs-2003"></a>Para remover objetos de diretiva de grupo antigos do Windows SBS 2003  
  
1.  Fazer logon no servidor de origem com uma conta de administrador.  
  
2.  Clique em **iniciar**e clique em **gerenciamento de servidor**.  
  
3.  No painel de navegação, clique em **gerenciamento avançado de**, clique em **Group Policy Management**e clique em **floresta:***< YourDomainName\ >*.  
  
4.  Clique em **domínios**, clique em *< YourDomainName\ >*e clique em **objetos de política de grupo**.  
  
5.  Clique com botão direito **política de auditoria do servidor de negócios pequeno**, clique em **excluir**e clique em **Okey**.  
  
6.  Repita a etapa 5 para excluir os GPOs a seguir que se aplicam à sua rede:  
  
    -   Computador do cliente Small Business Server  
  
    -   Política de senha de domínio de servidor de pequenas empresas  
  
         Recomendamos que você configure a política de senha no Windows Server Essentials para impor senhas fortes. Para configurar a política de senha, use o painel, que grava a configuração de política de domínio padrão. A configuração de política de senha não é gravada Small Business Server domínio senha objeto de diretiva, como era no Windows SBS 2003.  
  
    -   Firewall de Conexão de Internet de servidor de pequenas empresas  
  
    -   Política de bloqueio de servidor de pequenas empresas  
  
    -   Diretiva de assistência remota do Small Business Server  
  
    -   Firewall do Windows Small Business Server  
  
    -   Diretiva de computador de cliente Small Business Server Update Services  
  
         Esse GPO estará presente se você estiver migrando do Windows SBS 2003 R2.  
  
    -   Diretiva Small Business Server Update Services comuns configurações  
  
         Esse GPO estará presente se você estiver migrando do Windows SBS 2003 R2.  
  
    -   Diretiva de computador de servidor Small Business Server Update Services  
  
         Esse GPO estará presente se você estiver migrando do Windows SBS 2003 R2.  
  
7.  Confirme que todos os GPOs são excluídos.  
  
#### <a name="to-remove-wmi-filters-from-windows-sbs-2003"></a>Para remover filtros WMI do Windows SBS 2003  
  
1.  Fazer logon no servidor de origem com uma conta de administrador.  
  
2.  Clique em **iniciar**e clique em **gerenciamento de servidor**.  
  
3.  No painel de navegação, clique em **gerenciamento avançado de**, clique em **Group Policy Management**e clique em **floresta:***< YourNetworkDomainName\ >*  
  
4.  Clique em **domínios**, clique em *< YourNetworkDomainName\ >*e clique em **filtros WMI**.  
  
5.  Clique com botão direito **PostSP2**, clique em **excluir**e clique em **Sim**.  
  
6.  Clique com botão direito **PreSP2**, clique em **excluir**e clique em **Sim**.  
  
7.  Confirme que esses três filtros WMI são excluídos.  
  
##  <a name="BKMK_Configure"></a>Configurar a rede  
  
#### <a name="to-configure-the-network"></a>Para configurar a rede  
  
1.  No servidor de destino, abra o painel.  
  
2.  No painel **Home** página, clique em **instalação**, clique em **configurar o acesso em qualquer lugar**e, em seguida, escolha o **clique para configurar o acesso em qualquer lugar** opção.  
  
3.  Siga as instruções no **configurar o acesso em qualquer lugar** Assistente para configurar seu roteador e nome de domínio.  
  
 Se o roteador não der suporte a estrutura UPnP, ou se a estrutura UPnP está desabilitada, um ícone de aviso amarelo pode aparecer ao lado do nome do roteador. Certifique-se de que as seguintes portas estejam abertas e que eles são direcionados para o endereço IP do servidor de destino:  
  
-   Porta 80: O tráfego HTTP Web  
  
-   Porta 443: O tráfego da Web HTTPS  
  
> [!NOTE]
>  Se você tiver configurado um servidor do Exchange no local em um segundo servidor, você deve garantir porta 25 (SMTP) também é aberta e que ele é redirecionado para o endereço IP do servidor do Exchange no local.  
  
##  <a name="BKMK_MapPermittedComputers"></a>Mapear computadores permitidos para contas de usuário  
 No Windows SBS 2003, se um usuário se conecta ao acesso via Web remoto, todos os computadores na rede são exibidos. Isso pode incluir computadores que o usuário não tem permissão para acessar. No Windows Server Essentials, um usuário deve ser explicitamente atribuído a um computador para que ele seja exibido no acesso via Web remoto. Cada conta de usuário migrada do Windows SBS 2003 deve ser mapeada para um ou mais computadores.  
  
#### <a name="to-map-user-accounts-to-computers"></a>Mapear as contas de usuário em computadores  
  
1.  No servidor de destino, abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **usuários**.  
  
3.  Na lista de contas de usuário, clique com botão direito uma conta de usuário e, em seguida, clique em **exibir as propriedades da conta**.  
  
4.  Clique no **acesso em qualquer local** guia e, em seguida, clique em **permitir acesso remoto via Web e acesso a aplicativos web services.**.  
  
5.  Clique em **pastas compartilhadas**, clique em **computadores**, clique em **links da home page**e clique em **aplicar**.  
  
6.  Clique no **acesso ao computador** guia e clique no nome do computador ao qual você deseja permitir o acesso.  
  
7.  Repita as etapas 3, 4, 5 e 6 para cada conta de usuário.  
  
> [!NOTE]
>  Você não precisa alterar a configuração do computador cliente. Ele é configurado automaticamente.  
  
> [!NOTE]
>  Depois de concluir a migração, se você encontrar um problema quando você cria a primeira nova conta de usuário no servidor de destino, remova a conta de usuário que você adicionou e, em seguida, crie-o novamente.
