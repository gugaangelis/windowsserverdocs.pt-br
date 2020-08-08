---
title: Fazer backup de seus servidores Windows do centro de administração do Windows com o backup do Azure
description: Usar o centro de administração do Windows (projeto Honolulu) para fazer backup de servidores Windows com o backup do Azure
ms.topic: article
author: saurabhsensharma
ms.author: saurse
ms.date: 03/25/2019
ms.localizationpriority: low
ms.openlocfilehash: 796dfe509b1d24595dd3bc1aedd514789f4a378b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87949630"
---
# <a name="backup-your-windows-servers-from-windows-admin-center-with-azure-backup"></a>Fazer backup de seus servidores Windows do centro de administração do Windows com o backup do Azure

>Aplica-se a: Versão prévia do Windows Admin Center, Windows Admin Center

[Saiba mais sobre a integração do Azure com o Windows Admin Center.](../plan/azure-integration-options.md)

O centro de administração do Windows simplifica o processo de fazer backup de seus servidores Windows no Azure e de protegê-lo contra exclusões acidentais ou mal-intencionadas, corrupção e até mesmo ransomware. Para automatizar a instalação, você pode conectar o gateway do Windows Admin Center no Azure.

Use as informações a seguir para configurar o backup para o Windows Server e criar uma política de backup para fazer backup dos volumes do servidor e do estado do sistema do Windows no centro de administração do Windows.

## <a name="what-is-azure-backup-and-how-does-it-work-with-windows-admin-center"></a>O que é o backup do Azure e como ele funciona com o centro de administração do Windows?

O **Backup do Azure** é o serviço baseado no Azure que você pode usar para fazer backup (ou proteger) e restaurar os dados na nuvem da Microsoft. Ele substitui a solução de backup local ou externa existente por uma solução confiável, segura e econômica baseada em nuvem.
[Saiba mais sobre o backup do Azure](https://docs.microsoft.com/azure/backup/backup-overview).

O Backup do Azure oferece vários componentes que você pode baixar e implantar em um computador, servidor, ou na nuvem. O componente ou o agente que você implanta depende daquilo que deseja proteger. Todos os componentes de backup do Azure (não importa se você está protegendo dados locais ou no Azure) podem ser usados para fazer backup de dados em um cofre dos serviços de recuperação no Azure.

A integração do backup do Azure no centro de administração do Windows é ideal para fazer backup de volumes e de servidores locais ou físicos do Windows no estado do sistema Windows. Isso torna um mecanismo abrangente para backup de servidores de arquivos, controladores de domínio e servidores Web do IIS.

O centro de administração do Windows expõe a integração do backup do Azure por meio da ferramenta de **backup** nativo. A ferramenta de **backup** fornece experiências de instalação, gerenciamento e monitoramento para iniciar rapidamente o backup de seus servidores, executar operações comuns de backup e restauração e monitorar a integridade geral do backup de seus servidores Windows.

## <a name="prerequisites-and-planning"></a>Pré-requisitos e planejamento

- Uma conta do Azure com pelo menos uma assinatura ativa
- Os servidores Windows de destino que você deseja fazer backup devem ter acesso à Internet para o Azure
- [Conectar o gateway do centro de administração do Windows ao Azure](azure-integration.md)

Para iniciar o fluxo de trabalho para fazer backup do Windows Server, abra uma conexão de servidor, clique na ferramenta de **backup** e siga as etapas mencionadas abaixo.

## <a name="setup-azure-backup"></a>Configurar o backup do Azure
Ao clicar na ferramenta de **backup** para uma conexão de servidor na qual o backup do Azure ainda não esteja habilitado, você verá a tela **Bem-vindo ao backup do Azure** . Clique no botão **Configurar backup do Azure** . Isso abriria o assistente de configuração do backup do Azure. Siga as etapas listadas abaixo no Assistente para fazer backup do servidor.

Se o backup do Azure já estiver configurado, clicar na ferramenta de **backup** abrirá o **painel de backup**. Consulte a seção ([Gerenciamento e monitoramento](#management-and-monitoring)) para descobrir operações e tarefas que podem ser executadas no painel.

### <a name="step-1-login-to-microsoft-azure"></a>Etapa 1: fazer logon no Microsoft Azure
Entre em sua conta do Azure.

> [!NOTE]
> Se você conectou o gateway do centro de administração do Windows ao Azure, você deve fazer logon automaticamente no Azure. Você pode clicar em **sair** para entrar novamente como um usuário diferente.

### <a name="step-2-set-up-azure-backup"></a>Etapa 2: configurar o backup do Azure
Selecione as configurações apropriadas para o backup do Azure, conforme descrito abaixo

 - **ID da assinatura:** A assinatura do Azure que você deseja usar para fazer backup do Windows Server no Azure. Todos os ativos do Azure, como o grupo de recursos do Azure, o cofre dos serviços de recuperação serão criados na assinatura selecionada.
 - **Cofre:** O cofre dos serviços de recuperação onde os backups dos servidores serão armazenados. Você pode selecionar entre cofres existentes ou o centro de administração do Windows criará um novo cofre.
 - **Grupo de recursos:** O grupo de recursos do Azure é um contêiner para uma coleção de recursos. O cofre dos serviços de recuperação é criado ou está contido no grupo de recursos especificado. Você pode selecionar um dos grupos de recursos existentes ou o centro de administração do Windows criará um novo.
 - **Local:** A região do Azure em que o cofre dos serviços de recuperação será criado. É recomendável selecionar a região do Azure mais próxima do Windows Server.

### <a name="step-3-select-backup-items-and-schedule"></a>Etapa 3: selecionar itens e agenda de backup

- Selecione o que você deseja fazer backup do seu servidor. O centro de administração do Windows permite que você escolha entre uma combinação de **volumes** e o **estado do sistema do Windows** , oferecendo o tamanho estimado dos dados selecionados para backup.

> [!NOTE]
> O primeiro backup é um backup completo de todos os dados selecionados. No entanto, os backups subsequentes são incrementais por natureza e transferem apenas as alterações para os dados desde o backup anterior.

- Selecione entre vários **agendamentos de backup** predefinidos para o estado do sistema e/ou volumes.

### <a name="step-4-enter-encryption-passphrase"></a>Etapa 4: inserir senha de criptografia

- Insira uma **frase secreta de criptografia** de sua escolha (mínimo de 16 caracteres).  O **backup do Azure** protege seus dados de backup com uma frase secreta de criptografia configurada pelo usuário e gerenciada pelo usuário. A senha de criptografia é necessária para recuperar dados do backup do Azure.

> [!NOTE]
> A senha deve ser armazenada em um local externo seguro, como outro servidor ou o [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-portal). A Microsoft não armazenará a senha e não poderá recuperar ou redefinir a senha se ela for perdida ou esquecida.

- Examine todas as configurações e clique em **aplicar**

O centro de administração do Windows executará as seguintes operações

1. Criar um grupo de recursos do Azure se ele ainda não existir
2. Criar um cofre dos serviços de recuperação do Azure conforme especificado
3. Instalar e registrar o agente de Serviços de Recuperação do Microsoft Azure no cofre
4. Crie o agendamento de backup e retenção de acordo com as opções selecionadas e associe-as ao Windows Server.

## <a name="management-and-monitoring"></a>Gerenciamento e monitoramento

Depois de configurar com êxito o backup do Azure, você verá o **painel de backup** ao abrir a ferramenta de backup para uma conexão de servidor existente. Você pode executar as seguintes tarefas no **painel de backup**

- **Acesse o cofre no Azure:** Você pode clicar no link do **cofre dos serviços de recuperação** na guia **visão geral** do **painel de backup** para ser levado ao cofre no Azure para executar um [conjunto avançado de operações de gerenciamento](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server)
- **Executar um backup ad hoc:** Clique em **fazer backup agora** para fazer um backup ad hoc.
- **Monitorar trabalhos e configurar notificações de alerta:** Navegue até a guia **trabalhos** do painel para monitorar trabalhos em andamento ou passados e [configurar notificações de alerta](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server#configuring-notifications-for-alerts) para receber emails de quaisquer trabalhos com falha ou outros alertas relacionados ao backup.
- **Exibir pontos de recuperação e recuperar dados:** Clique na guia **pontos de recuperação** do painel para exibir os pontos de recuperação e clique em **recuperar dados** para obter as etapas para recuperar os dados do Azure.