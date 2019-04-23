---
title: Usar a política de grupo para configurar computadores clientes membros do domínio
description: Este tópico faz parte do BranchCache implantação guia para o Windows Server 2016, que demonstra como implantar o BranchCache nos modos de cache hospedado e distribuído para otimizar o uso de largura de banda WAN em filiais
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 911c1538-f79d-42e9-ba38-f4618f87b008
ms.author: pashort
author: shortpatti
ms.date: 06/02/2018
ms.openlocfilehash: 8e82d3e0ee7a84fbd6e2916d0f22472a8c117688
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855077"
---
# <a name="use-group-policy-to-configure-domain-member-client-computers"></a>Usar a política de grupo para configurar computadores clientes membros do domínio

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Nesta seção, você cria um objeto de diretiva de grupo para todos os computadores em sua organização, configurar computadores clientes membros do domínio com o modo de cache distribuído ou modo de cache hospedado e configurar o Firewall do Windows com segurança avançada para permitir que o BranchCache tráfego.  
  
Esta seção contém os procedimentos a seguir.  
  
1.  [Para criar um objeto de diretiva de grupo e configurar os modos do BranchCache](#bkmk_gp)  
  
2.  [Para configurar o Firewall do Windows com Advanced Security tráfego regras de entrada](#bkmk_inbound)  
  
3.  [Para configurar o Firewall do Windows com Advanced Security tráfego regras de saída](#bkmk_outbound)  
  
> [!TIP]  
> No procedimento a seguir, você é instruído para criar um objeto de diretiva de grupo na diretiva de domínio padrão, no entanto, você pode criar o objeto em uma unidade organizacional (UO) ou outro contêiner que é apropriado para sua implantação.  
  
Você deve ser um membro da **Admins. do domínio**, ou equivalente a executar esses procedimentos.  
  
## <a name="bkmk_gp"></a>Para criar um objeto de diretiva de grupo e configurar os modos do BranchCache  
  
1.  Em um computador no qual a função de servidor do Active Directory Domain Services está instalada, no Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **Group Policy Management**. Abre o console de gerenciamento de diretiva de grupo.  
  
2.  No Console de Gerenciamento de Diretiva de Grupo, expanda o seguinte caminho: **Floresta:** *exemplo.com*, **domínios**, *exemplo.com*, **objetos de diretiva de grupo**, onde  *exemplo.com* é o nome do domínio onde se encontram as contas de computador cliente BranchCache que você deseja configurar.  
  
3.  Clique com o botão direito do mouse em **Objetos de Diretiva de Grupo** e clique em **Novo**. A caixa de diálogo **Novo GPO** é aberta. Na **nome**, digite um nome para o novo grupo de política de GPO (objeto). Por exemplo, se desejar denominar o objeto como Computadores Cliente BranchCache, digite **Computadores Cliente BranchCache**. Clique em **OK**.  
  
4.  No Console de Gerenciamento de Diretiva de Grupo, verifique se **Objetos de Diretiva de Grupo** está selecionado e, no painel de detalhes, clique com o botão direito do mouse no GPO que você acabou de criar. Por exemplo, se você denominou seu GPO como Computadores Cliente BranchCache, clique com o botão direito do mouse em **Computadores Cliente BranchCache**. Clique em **Editar**. O console do Editor de gerenciamento de diretiva de grupo é aberto.  
  
5.  No console do Editor de gerenciamento de diretiva de grupo, expanda o seguinte caminho: **Configuração do computador**, **diretivas**, **modelos administrativos: Definições de diretiva (arquivos ADMX) recuperadas do computador local**, **Network**, **BranchCache**.  
  
6.  Clique em **BranchCache** e, no painel de detalhes, clique duas vezes em **Ativar o BranchCache**. Abre a caixa de diálogo de configuração da política.  
  
7.  Na caixa de diálogo **Ativar o BranchCache**, clique em **Habilitado** e em **OK**.  
  
8.  Para habilitar o modo de cache distribuído do BranchCache, no painel de detalhes, clique duas vezes em **modo de Cache distribuído BranchCache definir**. Abre a caixa de diálogo de configuração da política.  
  
9. Na caixa de diálogo **Definir o modo Cache Distribuído do BranchCache**, clique em **Habilitado** e em **OK**.  
  
10. Se você tiver um ou mais filiais em que você está implantando o BranchCache no modo de cache hospedado e você tiver implantado servidores de cache hospedado em escritórios, clique duas vezes **habilitar Hosted Cache a descoberta automática pelo ponto de Conexão de serviço**. Abre a caixa de diálogo de configuração da política.  
  
11. No **habilitar Hosted Cache a descoberta automática pelo ponto de Conexão de serviço** caixa de diálogo, clique em **Enabled**e, em seguida, clique em **Okey**.  
  
    > [!NOTE]  
    > Quando você habilita o ambos os **modo Definir Cache distribuído do BranchCache** e o **habilitar Hosted Cache a descoberta automática pelo ponto de Conexão de serviço** configurações de política, os computadores cliente operam no BranchCache modos de cache distribuído, a menos que ele encontrar um servidor de cache hospedado na filial, ponto em que eles operam em modo de cache hospedado.  
  
12. Use os procedimentos a seguir para definir configurações de firewall em computadores cliente usando a diretiva de grupo.  
  
## <a name="bkmk_inbound"></a>Para configurar o Firewall do Windows com Advanced Security tráfego regras de entrada  
  
1.  No Console de Gerenciamento de Diretiva de Grupo, expanda o seguinte caminho: **Floresta:** *exemplo.com*, **domínios**, *exemplo.com*, **objetos de diretiva de grupo**, onde  *exemplo.com* é o nome do domínio onde se encontram as contas de computador cliente BranchCache que você deseja configurar.  
  
2.  No Console de Gerenciamento de Diretiva de Grupo, verifique se **Objetos de Diretiva de Grupo** está selecionado e, no painel de detalhes, clique no GPO dos computadores cliente BranchCache criados anteriormente. Por exemplo, se você denominou seu GPO como Computadores Cliente BranchCache, clique com o botão direito do mouse em **Computadores Cliente BranchCache**. Clique em **Editar**. O console do Editor de gerenciamento de diretiva de grupo é aberto.  
  
3.  No console do Editor de gerenciamento de diretiva de grupo, expanda o seguinte caminho: **Configuração do computador**, **diretivas**, **configurações do Windows**, **as configurações de segurança**, **Firewall do Windows com segurança avançada**, **Firewall do Windows com segurança avançada - LDAP**, **regras de entrada**.  
  
4.  Clique com o botão direito do mouse em **Regras de Entrada** e clique em **Nova Regra**. O Assistente para Nova Regra de Entrada é aberto.  
  
5.  Na **tipo de regra**, clique em **predefinida**, expanda a lista de opções e, em seguida, clique em **BranchCache - recuperação de conteúdo (usa HTTP)**. Clique em **Avançar**.  
  
6.  Em **Regras Predefinidas**, clique em **Avançar**.  
  
7.  Em **Ação**, verifique se **Permitir a conexão** está selecionado e clique em **Concluir**.  
  
    > [!IMPORTANT]  
    > Selecione **Permitir a conexão** para que o cliente BranchCache possa receber o tráfego nesta porta.  
  
8.  Para criar uma exceção do firewall do WS-Discovery, clique novamente com o botão direito do mouse em **Regras de Entrada** e clique em **Nova Regra**. O Assistente para Nova Regra de Entrada é aberto.  
  
9. Na **tipo de regra**, clique em **predefinida**, expanda a lista de opções e, em seguida, clique em **BranchCache - descoberta de mesmo nível (usa WSD)**. Clique em **Avançar**.  
  
10. Em **Regras Predefinidas**, clique em **Avançar**.  
  
11. Em **Ação**, verifique se **Permitir a conexão** está selecionado e clique em **Concluir**.  
  
    > [!IMPORTANT]  
    > Selecione **Permitir a conexão** para que o cliente BranchCache possa receber o tráfego nesta porta.  
  
## <a name="bkmk_outbound"></a>Para configurar o Firewall do Windows com Advanced Security tráfego regras de saída  
  
1.  No console do Editor de Gerenciamento de Diretiva de Grupo, clique com o botão direito do mouse em **Regras de Saída** e clique em **Nova Regra**. O Assistente para Nova Regra de Saída é aberto.  
  
2.  Na **tipo de regra**, clique em **predefinida**, expanda a lista de opções e, em seguida, clique em **BranchCache - recuperação de conteúdo (usa HTTP)**. Clique em **Avançar**.  
  
3.  Em **Regras Predefinidas**, clique em **Avançar**.  
  
4.  Em **Ação**, verifique se **Permitir a conexão** está selecionado e clique em **Concluir**.  
  
    > [!IMPORTANT]  
    > Selecione **Permitir a conexão** para que o cliente BranchCache possa enviar o tráfego nesta porta.  
  
5.  Para criar uma exceção do firewall do WS-Discovery, clique novamente com o botão direito do mouse em **Regras de Saída** e clique em **Nova Regra**. O Assistente para Nova Regra de Saída é aberto.  
  
6.  Na **tipo de regra**, clique em **predefinida**, expanda a lista de opções e, em seguida, clique em **BranchCache - descoberta de mesmo nível (usa WSD)**. Clique em **Avançar**.  
  
7.  Em **Regras Predefinidas**, clique em **Avançar**.  
  
8.  Em **Ação**, verifique se **Permitir a conexão** está selecionado e clique em **Concluir**.  
  
    > [!IMPORTANT]  
    > Selecione **Permitir a conexão** para que o cliente BranchCache possa enviar o tráfego nesta porta.  
  


