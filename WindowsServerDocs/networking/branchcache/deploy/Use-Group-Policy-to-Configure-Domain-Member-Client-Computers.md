---
title: Usar Política de Grupo para configurar computadores cliente membro do domínio
description: Este tópico faz parte do guia de implantação do BranchCache para o Windows Server 2016, que demonstra como implantar o BranchCache em modos de cache distribuídos e hospedados para otimizar o uso de largura de banda WAN em filiais
manager: dougkim
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 911c1538-f79d-42e9-ba38-f4618f87b008
ms.author: lizross
author: eross-msft
ms.date: 06/02/2018
ms.openlocfilehash: c6ca1ff8fabb559628afd2dd1abafc56a908909a
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319192"
---
# <a name="use-group-policy-to-configure-domain-member-client-computers"></a>Usar Política de Grupo para configurar computadores cliente membro do domínio

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Nesta seção, você cria um objeto Política de Grupo para todos os computadores em sua organização, configura computadores cliente membro do domínio com o modo de cache distribuído ou o modo de cache hospedado e configura o Firewall do Windows com segurança avançada para permitir o BranchCache tráfico.  
  
Esta seção contém os procedimentos a seguir.  
  
1.  [Para criar um objeto de Política de Grupo e configurar modos de BranchCache](#bkmk_gp)  
  
2.  [Para configurar o Firewall do Windows com regras de tráfego de entrada de segurança avançada](#bkmk_inbound)  
  
3.  [Para configurar o Firewall do Windows com regras de tráfego de saída de segurança avançada](#bkmk_outbound)  
  
> [!TIP]  
> No procedimento a seguir, você é instruído a criar um objeto Política de Grupo na política de domínio padrão, no entanto, você pode criar o objeto em uma UO (unidade organizacional) ou em outro contêiner que seja apropriado para sua implantação.  
  
Você deve ser membro de **Admins**. do domínio ou equivalente a executar esses procedimentos.  
  
## <a name="to-create-a-group-policy-object-and-configure-branchcache-modes"></a><a name="bkmk_gp"></a>Para criar um objeto de Política de Grupo e configurar modos de BranchCache  
  
1.  Em um computador no qual a função de servidor Active Directory Domain Services está instalada, em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, clique em **Gerenciamento de política de grupo**. O console de gerenciamento do Política de Grupo é aberto.  
  
2.  No console de gerenciamento do Política de Grupo, expanda o seguinte caminho: **floresta:** *example.com*, **domínios**, *example.com*, **política de grupo objetos**, em que *example.com* é o nome do domínio em que as contas de computador cliente do BranchCache que você deseja configurar estão localizadas.  
  
3.  Clique com o botão direito do mouse em **Objetos de Diretiva de Grupo** e clique em **Novo**. A caixa de diálogo **Novo GPO** é aberta. Em **nome**, digite um nome para o novo objeto de política de grupo (GPO). Por exemplo, se desejar denominar o objeto como Computadores Cliente BranchCache, digite **Computadores Cliente BranchCache**. Clique em **OK**.  
  
4.  No Console de Gerenciamento de Diretiva de Grupo, verifique se **Objetos de Diretiva de Grupo** está selecionado e, no painel de detalhes, clique com o botão direito do mouse no GPO que você acabou de criar. Por exemplo, se você denominou seu GPO como Computadores Cliente BranchCache, clique com o botão direito do mouse em **Computadores Cliente BranchCache**. Clique em **Editar**. O console do Editor de Gerenciamento de Política de Grupo é aberto.  
  
5.  No console do Editor de Gerenciamento de Política de Grupo, expanda o seguinte caminho: **configuração do computador**, **políticas** **modelos administrativos: definições de política (arquivos ADMX) recuperadas do computador local**, **rede**, **BranchCache**.  
  
6.  Clique em **BranchCache** e, no painel de detalhes, clique duas vezes em **Ativar o BranchCache**. A caixa de diálogo configuração de política é aberta.  
  
7.  Na caixa de diálogo **Ativar o BranchCache**, clique em **Habilitado** e em **OK**.  
  
8.  Para habilitar o modo de cache distribuído do BranchCache, no painel de detalhes, clique duas vezes em **definir o modo de cache distribuído do BranchCache**. A caixa de diálogo configuração de política é aberta.  
  
9. Na caixa de diálogo **Definir o modo Cache Distribuído do BranchCache**, clique em **Habilitado** e em **OK**.  
  
10. Se você tiver uma ou mais filiais em que você está implantando o BranchCache no modo de cache hospedado e tiver implantado servidores de cache hospedados nesses escritórios, clique duas vezes em **habilitar descoberta automática de cache hospedado pelo ponto de conexão de serviço**. A caixa de diálogo configuração de política é aberta.  
  
11. Na caixa de diálogo **habilitar descoberta automática de cache hospedado por ponto de conexão de serviço** , clique em **habilitado**e em **OK**.  
  
    > [!NOTE]  
    > Quando você habilita as configurações de política **definir o modo de cache distribuído do BranchCache** e **habilitar descoberta automática de cache hospedado por serviço** , os computadores cliente operam no modo de cache distribuído do BranchCache, a menos que encontrem um servidor de cache hospedado na filial, em que ponto eles operam no modo de cache hospedado.  
  
12. Use os procedimentos a seguir para definir as configurações de firewall em computadores cliente usando Política de Grupo.  
  
## <a name="to-configure-windows-firewall-with-advanced-security-inbound-traffic-rules"></a><a name="bkmk_inbound"></a>Para configurar o Firewall do Windows com regras de tráfego de entrada de segurança avançada  
  
1.  No console de gerenciamento do Política de Grupo, expanda o seguinte caminho: **floresta:** *example.com*, **domínios**, *example.com*, **política de grupo objetos**, em que *example.com* é o nome do domínio em que as contas de computador cliente do BranchCache que você deseja configurar estão localizadas.  
  
2.  No Console de Gerenciamento de Diretiva de Grupo, verifique se **Objetos de Diretiva de Grupo** está selecionado e, no painel de detalhes, clique no GPO dos computadores cliente BranchCache criados anteriormente. Por exemplo, se você denominou seu GPO como Computadores Cliente BranchCache, clique com o botão direito do mouse em **Computadores Cliente BranchCache**. Clique em **Editar**. O console do Editor de Gerenciamento de Política de Grupo é aberto.  
  
3.  No console do Editor de Gerenciamento de Política de Grupo, expanda o seguinte caminho **: configuração do computador**, **políticas**, **configurações do Windows**, configurações de **segurança**, **Firewall do Windows com segurança avançada**, **Firewall do Windows com segurança avançada-LDAP**, **regras de entrada**.  
  
4.  Clique com o botão direito do mouse em **Regras de Entrada** e clique em **Nova Regra**. O Assistente para Nova Regra de Entrada é aberto.  
  
5.  Em **tipo de regra**, clique em **predefinido**, expanda a lista de opções e clique em **BranchCache-Recuperação de conteúdo (usa http)** . Clique em **Avançar**.  
  
6.  Em **Regras Predefinidas**, clique em **Avançar**.  
  
7.  Em **Ação**, verifique se **Permitir a conexão** está selecionado e clique em **Concluir**.  
  
    > [!IMPORTANT]  
    > Selecione **Permitir a conexão** para que o cliente BranchCache possa receber o tráfego nesta porta.  
  
8.  Para criar uma exceção do firewall do WS-Discovery, clique novamente com o botão direito do mouse em **Regras de Entrada** e clique em **Nova Regra**. O Assistente para Nova Regra de Entrada é aberto.  
  
9. Em **tipo de regra**, clique em **predefinido**, expanda a lista de opções e clique em **BranchCache – descoberta de mesmo nível (usa WSD)** . Clique em **Avançar**.  
  
10. Em **Regras Predefinidas**, clique em **Avançar**.  
  
11. Em **Ação**, verifique se **Permitir a conexão** está selecionado e clique em **Concluir**.  
  
    > [!IMPORTANT]  
    > Selecione **Permitir a conexão** para que o cliente BranchCache possa receber o tráfego nesta porta.  
  
## <a name="to-configure-windows-firewall-with-advanced-security-outbound-traffic-rules"></a><a name="bkmk_outbound"></a>Para configurar o Firewall do Windows com regras de tráfego de saída de segurança avançada  
  
1.  No console do Editor de Gerenciamento de Diretiva de Grupo, clique com o botão direito do mouse em **Regras de Saída** e clique em **Nova Regra**. O Assistente para Nova Regra de Saída é aberto.  
  
2.  Em **tipo de regra**, clique em **predefinido**, expanda a lista de opções e clique em **BranchCache-Recuperação de conteúdo (usa http)** . Clique em **Avançar**.  
  
3.  Em **Regras Predefinidas**, clique em **Avançar**.  
  
4.  Em **Ação**, verifique se **Permitir a conexão** está selecionado e clique em **Concluir**.  
  
    > [!IMPORTANT]  
    > Selecione **Permitir a conexão** para que o cliente BranchCache possa enviar o tráfego nesta porta.  
  
5.  Para criar uma exceção do firewall do WS-Discovery, clique novamente com o botão direito do mouse em **Regras de Saída** e clique em **Nova Regra**. O Assistente para Nova Regra de Saída é aberto.  
  
6.  Em **tipo de regra**, clique em **predefinido**, expanda a lista de opções e clique em **BranchCache – descoberta de mesmo nível (usa WSD)** . Clique em **Avançar**.  
  
7.  Em **Regras Predefinidas**, clique em **Avançar**.  
  
8.  Em **Ação**, verifique se **Permitir a conexão** está selecionado e clique em **Concluir**.  
  
    > [!IMPORTANT]  
    > Selecione **Permitir a conexão** para que o cliente BranchCache possa enviar o tráfego nesta porta.  
  


