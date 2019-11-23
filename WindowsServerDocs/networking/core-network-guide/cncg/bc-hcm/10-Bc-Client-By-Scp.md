---
title: Configurar descoberta automática de cache hospedado cliente por ponto de conexão de serviço
description: Este guia fornece instruções sobre como implantar o BranchCache no modo de cache hospedado em computadores que executam o Windows Server 2016 e o Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: ea1c34fd-5a33-4228-9437-9bb3d44230eb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 60ccc8b80537da0d0b689f6c508c75ef15a339c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406401"
---
#  <a name="configure-client-automatic-hosted-cache-discovery-by-service-connection-point"></a>Configurar descoberta automática de cache hospedado cliente por ponto de conexão de serviço

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Com este procedimento, você pode usar Política de Grupo para habilitar e configurar o modo de cache hospedado do BranchCache no domínio\-computadores ingressados que estão executando os seguintes sistemas operacionais Windows compatíveis com o\-BranchCache.

- Windows 10 Enterprise
- Windows 10 Education
- Windows 8.1 Enterprise
- Windows 8 Enterprise

> [!NOTE]  
> Para configurar computadores ingressados no domínio que executam o Windows Server 2008 R2 ou o Windows 7, consulte o [Guia de implantação do BranchCache](https://technet.microsoft.com/library/ee649232.aspx)do windows Server 2008 R2.

A associação a **Adminis. do Domínio** ou equivalente é o requisito mínimo para a execução deste procedimento.

### <a name="to-use-group-policy-to-configure-clients-for-hosted-cache-mode"></a>Para usar Política de Grupo para configurar clientes para o modo de cache hospedado

1. Em um computador no qual a função de servidor do Active Directory Domain Services está instalada, abra Gerenciador do Servidor, selecione o servidor local, clique em **ferramentas**e, em seguida, clique em **Gerenciamento de política de grupo**. O console de gerenciamento do Política de Grupo é aberto.

2. No console de gerenciamento do Política de Grupo, expanda o seguinte caminho: **floresta:** *Corp.contoso.com*, **domínios**, *corp.contoso.com*, **política de grupo objetos**, em que *Corp.contoso.com* é o nome do domínio em que as contas de computador cliente do BranchCache que você deseja configurar estão localizadas.

3. À direita\-clique em **política de grupo objetos**e, em seguida, clique em **novo**. A caixa de diálogo **Novo GPO** é aberta. Em **nome**, digite um nome para o novo objeto de política de grupo \(GPO\). Por exemplo, se desejar denominar o objeto como Computadores Cliente BranchCache, digite **Computadores Cliente BranchCache**. Clique em **OK**.

4. No console de gerenciamento do Política de Grupo, verifique se **política de grupo objetos** está selecionado e no painel de detalhes à direita\-clique no GPO que você acabou de criar. Por exemplo, se você tiver nomeado os computadores cliente do GPO BranchCache, clique com o botão direito\-do mouse em **computadores cliente BranchCache**. Clique em **Editar**. O console do Editor de Gerenciamento de Política de Grupo é aberto.

5. No console do Editor de Gerenciamento de Política de Grupo, expanda o seguinte caminho: **configuração do computador**, **políticas** **Modelos Administrativos: definições de política \(arquivos ADMX\) recuperados do computador local**, **rede**, **BranchCache**.

6. Clique em **BranchCache**e, no painel de detalhes, clique duas vezes\-em **Ativar BranchCache**. A caixa de diálogo **Ativar o BranchCache** é aberta.
  
7.  Na caixa de diálogo **Ativar o BranchCache**, clique em **Habilitado** e em **OK**.

8. No console do Editor de Gerenciamento de Política de Grupo, verifique se o **BranchCache** ainda está selecionado e, no painel de detalhes, clique duas\-vezes em **habilitar descoberta automática de cache hospedado pelo ponto de conexão de serviço**. A caixa de diálogo configuração de política é aberta.

9. Na caixa de diálogo **habilitar descoberta automática de cache hospedado por ponto de conexão de serviço** , clique em **habilitado**e em **OK**.

10. Para permitir que os computadores cliente baixem e armazenem o conteúdo em cache do servidor de arquivos do BranchCache\-servidores de conteúdo baseados no console do Editor de Gerenciamento de Política de Grupo, verifique se o **BranchCache** ainda está selecionado e, em seguida, no painel de detalhes\-clique em **BranchCache para arquivos de rede**. A caixa de diálogo **Configurar o BranchCache para arquivos de rede** é aberta. 
11. Na caixa de diálogo **Configurar o BranchCache para arquivos de rede**, clique em **Habilitado**. Em **Opções**, digite um valor numérico, em milissegundos, para o tempo máximo de latência de rede completa e clique em **OK**.
  
    > [!NOTE]
    > Por padrão, os computadores cliente armazenam em cache o conteúdo de servidores de arquivos se a latência de rede de ida e volta for maior que 80 milissegundos.
  
12. Para configurar a quantidade de espaço em disco alocado em cada computador cliente para o cache do BranchCache: no console do Editor de Gerenciamento de Política de Grupo, verifique se o **BranchCache** ainda está selecionado e, no painel de detalhes,\-clique duas vezes em **definir percentual de espaço em disco usado para o cache do computador cliente**. A caixa de diálogo **Definir a porcentagem do espaço em disco usada pelo cache do computador cliente** é aberta. Clique em **Habilitado** e, em **Opções**, digite um valor numérico que represente a porcentagem de espaço no disco rígido usado em cada computador cliente para o cache do BranchCache. Clique em **OK**.

13. Para especificar a idade padrão, em dias, para quais segmentos são válidos no cache de dados do BranchCache em computadores cliente: no console do Editor de Gerenciamento de Política de Grupo, verifique se o **BranchCache** ainda está selecionado e, no painel de detalhes, clique duas vezes\-clique em **definir idade para segmentos no cache de dados**. A caixa de diálogo **definir idade para segmentos na cache de dados** é aberta. Clique em **habilitado**e, no painel de detalhes, digite o número de dias que você preferir. Clique em **OK**.

14. Defina configurações adicionais de política de BranchCache para computadores cliente, conforme apropriado para sua implantação.

15. Atualize Política de Grupo nos computadores cliente da filial executando o comando **gpupdate/force**ou reinicializando os computadores cliente.

A implantação do modo de cache hospedado do BranchCache agora está concluída.

Para obter informações adicionais sobre as tecnologias deste guia, consulte [recursos adicionais](11-Bc-Hcm-additional-resources.md).