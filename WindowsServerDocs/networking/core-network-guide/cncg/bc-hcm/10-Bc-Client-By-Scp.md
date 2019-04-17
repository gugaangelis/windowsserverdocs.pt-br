---
title: Configurar a descoberta de Cache hospedado automática de cliente pelo ponto de Conexão de serviço
description: Este guia fornece instruções sobre como implantar BranchCache em modo de cache hospedado em computadores que executam o Windows Server 2016 e o Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: ea1c34fd-5a33-4228-9437-9bb3d44230eb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b12fa6f9e11c8816d74c9013dd80b3fa38d0a478
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
#  <a name="configure-client-automatic-hosted-cache-discovery-by-service-connection-point"></a>Configurar a descoberta de Cache hospedado automática de cliente pelo ponto de Conexão de serviço

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Com esse procedimento, você pode usar política de grupo para habilitar e configurar o modo de cache BranchCache hospedado em computadores ingressados em domain\ que estão executando os seguintes sistemas operacionais Windows compatíveis com BranchCache\.

- Windows 10 Enterprise
- Windows 10 Education
- Windows 8.1 Enterprise
- Windows 8 Enterprise

> [!NOTE]  
> Para configurar computadores ingressados em domínio que executam o Windows Server 2008 R2 ou Windows 7, consulte o Windows Server 2008 R2 [guia de implantação do BranchCache](https://technet.microsoft.com/library/ee649232.aspx).

A associação ao grupo **Admins. do domínio**, ou equivalente é o requisito mínimo para executar este procedimento.

### <a name="to-use-group-policy-to-configure-clients-for-hosted-cache-mode"></a>Para usar política de grupo para configurar clientes para o modo de cache hospedado

1. Em um computador no qual a função de servidor de serviços de domínio do Active Directory é instalada, abra o Gerenciador do servidor, selecione o servidor Local, clique em **ferramentas**e clique em **Group Policy Management**. O console de gerenciamento de política de grupo é aberto.

2. No console de gerenciamento de política de grupo, expanda o caminho a seguir: **floresta:** *corp.contoso.com*, **domínios**, *corp.contoso.com*, **objetos de política de grupo**, onde *corp.contoso.com* é o nome do domínio onde as contas de computador cliente BranchCache que você deseja configurar estão localizadas.

3. Clique em Right\ **objetos de política de grupo**e clique em **nova**. O **novo GPO** caixa de diálogo é aberta. Em **nome**, digite um nome para a nova política de grupo \(GPO\) de objeto. Por exemplo, se você quiser nomear o objeto BranchCache os computadores cliente, digite **os computadores cliente BranchCache**. Clique em **Okey**.

4. No console de gerenciamento de política de grupo, certifique-se de que **objetos de política de grupo** é selecionado e os detalhes painel right\-clique o GPO que você acabou criado. Por exemplo, se você nomeou computadores cliente BranchCache GPO, clique em right\ **os computadores cliente BranchCache**. Clique em **editar**. O console do Editor de gerenciamento de política de grupo é aberto.

5. No console do Editor de gerenciamento de política de grupo, expanda o caminho a seguir: **configuração do computador**, **políticas**, **modelos administrativos: política definições \(ADMX files\) recuperado do computador local**, **rede**, **BranchCache**.

6. Clique em **BranchCache**e, em seguida, no painel de detalhes, clique em double\ **ativar BranchCache**. O **ativar BranchCache** caixa de diálogo é aberta.
  
7.  No **ativar BranchCache** caixa de diálogo, clique em **Enabled**e clique em **Okey**.

8. No console do Editor de gerenciamento de política de grupo, certifique-se de que **BranchCache** ainda estiver selecionado e, em seguida, o painel de detalhes double\-clique **habilitar hospedado Cache descoberta automática pelo serviço de Conexão ponto**. Abre a caixa de diálogo de configuração de política.

9. No **habilitar hospedado Cache descoberta automática pelo serviço de Conexão ponto** caixa de diálogo, clique em **Enabled**e clique em **Okey**.

10. Para permitir que os computadores cliente baixar e armazenar em cache conteúdo de BranchCache baseados em servidor \ conteúdos servidores de arquivos: no console do Editor de gerenciamento de política de grupo, certifique-se de que **BranchCache** ainda estiver selecionado e, em seguida, o painel de detalhes double\-clique **BranchCache para arquivos de rede**. O **configurar BranchCache para arquivos de rede** caixa de diálogo é aberta. 
11. No **configurar BranchCache para arquivos de rede** caixa de diálogo, clique em **Enabled**. Em **opções**, digite um valor numérico, em milissegundos, para o tempo de latência máxima redondo rede e, em seguida, clique em **Okey**.
  
    > [!NOTE]
    > Por padrão, os computadores cliente cache conteúdo dos servidores de arquivos se a latência da rede de ida e volta for maior que 80 milissegundos.
  
12. Para configurar a quantidade de espaço em disco rígido alocada em cada computador cliente para o cache BranchCache: no console do Editor de gerenciamento de política de grupo, certifique-se de que **BranchCache** ainda estiver selecionado e, em seguida, o painel de detalhes double\-clique **definir o percentual de espaço em disco usado para o cache do computador cliente**. O **definir o percentual de espaço em disco usado para o cache do computador cliente** caixa de diálogo é aberta. Clique em **Enabled**e, em seguida, em **opções** digitar um valor numérico que representa o percentual de espaço em disco usado em cada computador cliente para o cache BranchCache. Clique em **Okey**.

13. Para especificar a idade de padrão, em dias, para os quais segmentos são válidos no cache de dados BranchCache em computadores cliente: no console do Editor de gerenciamento de política de grupo, certifique-se de que **BranchCache** ainda estiver selecionado e, em seguida, o painel de detalhes double\-clique **definir idade para segmentos em cache dados**. O **definir idade para segmentos em cache dados** caixa de diálogo é aberta. Clique em **Enabled**e, em seguida, no painel de detalhes, digite o número de dias que preferir. Clique em **Okey**.

14. Defina configurações de política de BranchCache adicionais para os computadores cliente conforme apropriado para a implantação.

15. Atualizar a política de grupo em computadores cliente filiais executando o comando **gpupdate /force**, ou pela reinicialização os computadores cliente.

A implantação de modo de Cache hospedado do BranchCache agora é concluída.

Para obter informações adicionais sobre as tecnologias neste guia, consulte [recursos adicionais](11-Bc-Hcm-additional-resources.md).