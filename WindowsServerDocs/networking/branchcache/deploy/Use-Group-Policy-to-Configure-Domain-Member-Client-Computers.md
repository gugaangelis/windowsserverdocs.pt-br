---
title: Usar política de grupo para configurar computadores cliente membros do domínio
description: Este tópico faz parte do BranchCache implantação guia para Windows Server 2016, que demonstra como implantar BranchCache nos modos de cache hospedado e distribuídos para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 911c1538-f79d-42e9-ba38-f4618f87b008
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 96bf84df14eac8016a898e92eb96f90435c53c98
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="use-group-policy-to-configure-domain-member-client-computers"></a>Usar política de grupo para configurar computadores cliente membros do domínio

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar esses procedimentos para criar um objeto de política de grupo para todos os computadores em sua organização, para configurar computadores cliente membros do domínio com o modo de cache distribuído ou cache hospedado e configurar o Firewall do Windows com segurança avançada para permitir o tráfego BranchCache.  
  
Esta seção contém os procedimentos a seguir.  
  
1.  [Para criar um objeto de política de grupo e configurar modos BranchCache](#bkmk_gp)  
  
2.  [Configurar o Firewall do Windows com segurança entrada tráfego regras avançadas](#bkmk_inbound)  
  
3.  [Configurar o Firewall do Windows com Advanced Security tráfego regras de saída](#bkmk_outbound)  
  
> [!TIP]  
> No procedimento a seguir, você é instruído para criar um objeto de política de grupo na política de domínio padrão, mas você pode criar o objeto em uma unidade organizacional (UO) ou outro contêiner que é adequado para a implantação.  
  
Você deve ser um membro do **Admins. do domínio**, ou equivalente a executar esses procedimentos.  
  
## <a name="bkmk_gp"></a>Para criar um objeto de política de grupo e configurar modos BranchCache  
  
1.  Em um computador no qual a função de servidor de serviços de domínio do Active Directory é instalada, no Gerenciador do servidor, clique em **ferramentas**e clique em **Group Policy Management**. O console de gerenciamento de política de grupo é aberto.  
  
2.  No console de gerenciamento de política de grupo, expanda o caminho a seguir: **floresta:** *example.com*, **domínios**, *example.com*, **objetos de política de grupo**, onde *example.com* é o nome do domínio onde as contas de computador cliente BranchCache que você deseja configurar estão localizadas.  
  
3.  Clique com botão direito **objetos de política de grupo**e clique em **nova**. O **novo GPO** caixa de diálogo é aberta. Em **nome**, digite um nome para o objeto de política de grupo (GPO) novos. Por exemplo, se você quiser nomear o objeto BranchCache os computadores cliente, digite **os computadores cliente BranchCache**. Clique em **Okey**.  
  
4.  No console de gerenciamento de política de grupo, certifique-se de que **objetos de política de grupo** é selecionado e no painel de detalhes, clique com botão direito no GPO que você acabou de criar. Por exemplo, se você denominado computadores cliente BranchCache GPO, clique com botão direito **os computadores cliente BranchCache**. Clique em **editar**. O console do Editor de gerenciamento de política de grupo é aberto.  
  
5.  No console do Editor de gerenciamento de política de grupo, expanda o caminho a seguir: **configuração do computador**, **políticas**, **modelos administrativos: definições de política (arquivos ADMX) recuperadas do computador local**, **rede**, **BranchCache**.  
  
6.  Clique em **BranchCache**e, em seguida, no painel de detalhes, clique duas vezes em **ativar BranchCache**. Abre a caixa de diálogo de configuração de política.  
  
7.  No **ativar BranchCache** caixa de diálogo, clique em **Enabled**e clique em **Okey**.  
  
8.  Para habilitar o modo de cache distribuído BranchCache, no painel de detalhes, clique duas vezes em **modo Cache distribuído BranchCache definir**. Abre a caixa de diálogo de configuração de política.  
  
9. No **modo Definir BranchCache distribuído Cache** caixa de diálogo, clique em **Enabled**e clique em **Okey**.  
  
10. Se você tiver um ou mais filiais onde você estiver implantando BranchCache no modo de cache hospedado, e você implantou hospedado servidores de cache em escritórios, clique duas vezes em **habilitar hospedado Cache descoberta automática pelo serviço de Conexão ponto**. Abre a caixa de diálogo de configuração de política.  
  
11. No **habilitar hospedado Cache descoberta automática pelo serviço de Conexão ponto** caixa de diálogo, clique em **Enabled**e clique em **Okey**.  
  
    > [!NOTE]  
    > Quando você habilitar ambos os **modo Cache distribuído BranchCache definir** e o **habilitar hospedado Cache descoberta automática por ponto de Conexão de serviço** configurações de política, os computadores cliente operam no modo de cache distribuído BranchCache, a menos que ele encontrar um servidor de cache hospedado na filial, momento em que eles operam no modo de cache hospedado.  
  
12. Use os procedimentos a seguir para definir as configurações de firewall em computadores cliente usando a política de grupo.  
  
## <a name="bkmk_inbound"></a>Configurar o Firewall do Windows com segurança entrada tráfego regras avançadas  
  
1.  No console de gerenciamento de política de grupo, expanda o caminho a seguir: **floresta:** *example.com*, **domínios**, *example.com*, **objetos de política de grupo**, onde *example.com* é o nome do domínio onde as contas de computador cliente BranchCache que você deseja configurar estão localizadas.  
  
2.  No console de gerenciamento de política de grupo, certifique-se de que **objetos de política de grupo** é selecionado e no painel de detalhes, clique com botão direito computadores cliente BranchCache GPO que você criou anteriormente. Por exemplo, se você denominado computadores cliente BranchCache GPO, clique com botão direito **os computadores cliente BranchCache**. Clique em **editar**. O console do Editor de gerenciamento de política de grupo é aberto.  
  
3.  No console do Editor de gerenciamento de política de grupo, expanda o caminho a seguir: **configuração do computador**, **políticas**, **configurações do Windows**, **configurações de segurança**, **Firewall do Windows com segurança avançada**, **Firewall do Windows com segurança avançada - LDAP**, **regras de entrada**.  
  
4.  Clique com botão direito **regras de entrada**e clique em **nova regra**. O Assistente para nova regra de entrada é aberta.  
  
5.  Em **tipo de regra**, clique em **predefinida**, expanda a lista de opções e, em seguida, clique em **BranchCache - recuperação de conteúdo (usa HTTP)**. Clique em **próxima**.  
  
6.  Em **regras predefinidas**, clique em **próxima**.  
  
7.  Em **ação**, certifique-se de que **permitir a conexão** está selecionado e clique em **concluir**.  
  
    > [!IMPORTANT]  
    > Você deve selecionar **permitir a conexão** para o cliente BranchCache para ser capaz de receber tráfego nesta porta.  
  
8.  Para criar a exceção de firewall WS-Discovery, novamente com o botão direito **regras de entrada**e clique em **nova regra**. O Assistente para nova regra de entrada é aberta.  
  
9. Em **tipo de regra**, clique em **predefinida**, expanda a lista de opções e, em seguida, clique em **BranchCache - descoberta de par (WSD usa)**. Clique em **próxima**.  
  
10. Em **regras predefinidas**, clique em **próxima**.  
  
11. Em **ação**, certifique-se de que **permitir a conexão** está selecionado e clique em **concluir**.  
  
    > [!IMPORTANT]  
    > Você deve selecionar **permitir a conexão** para o cliente BranchCache para ser capaz de receber tráfego nesta porta.  
  
## <a name="bkmk_outbound"></a>Configurar o Firewall do Windows com Advanced Security tráfego regras de saída  
  
1.  No console do Editor de gerenciamento de política de grupo, clique com botão direito **regras de saída**e clique em **nova regra**. Abre a saída Assistente para nova regra.  
  
2.  Em **tipo de regra**, clique em **predefinida**, expanda a lista de opções e, em seguida, clique em **BranchCache - recuperação de conteúdo (usa HTTP)**. Clique em **próxima**.  
  
3.  Em **regras predefinidas**, clique em **próxima**.  
  
4.  Em **ação**, certifique-se de que **permitir a conexão** está selecionado e clique em **concluir**.  
  
    > [!IMPORTANT]  
    > Você deve selecionar **permitir a conexão** para o cliente BranchCache para poder enviar tráfego nesta porta.  
  
5.  Para criar a exceção de firewall WS-Discovery, novamente com o botão direito **regras de saída**e clique em **nova regra**. Abre a saída Assistente para nova regra.  
  
6.  Em **tipo de regra**, clique em **predefinida**, expanda a lista de opções e, em seguida, clique em **BranchCache - descoberta de par (WSD usa)**. Clique em **próxima**.  
  
7.  Em **regras predefinidas**, clique em **próxima**.  
  
8.  Em **ação**, certifique-se de que **permitir a conexão** está selecionado e clique em **concluir**.  
  
    > [!IMPORTANT]  
    > Você deve selecionar **permitir a conexão** para o cliente BranchCache para poder enviar tráfego nesta porta.  
  


