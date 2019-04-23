---
title: Configurar descoberta automática de cache hospedado cliente por ponto de conexão de serviço
description: Este guia fornece instruções sobre como implantar o BranchCache no modo de cache hospedado em computadores que executam o Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: ea1c34fd-5a33-4228-9437-9bb3d44230eb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bd77fc76a999517cb8372aec8dfad25b4dd5be3b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829717"
---
#  <a name="configure-client-automatic-hosted-cache-discovery-by-service-connection-point"></a>Configurar descoberta automática de cache hospedado cliente por ponto de conexão de serviço

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Com esse procedimento, você pode usar a diretiva de grupo para habilitar e configurar o modo de cache hospedado do BranchCache no domínio\-em computadores que estão executando o seguinte BranchCache ingressados\-compatível com sistemas de operacionais do Windows.

- Windows 10 Enterprise
- Windows 10 Education
- Windows 8.1 Enterprise
- Windows 8 Enterprise

> [!NOTE]  
> Para configurar computadores ingressados no domínio que executam o Windows Server 2008 R2 ou Windows 7, consulte o Windows Server 2008 R2 [guia de implantação do BranchCache](https://technet.microsoft.com/library/ee649232.aspx).

A associação a **Adminis. do Domínio** ou equivalente é o requisito mínimo para a execução deste procedimento.

### <a name="to-use-group-policy-to-configure-clients-for-hosted-cache-mode"></a>Para usar a diretiva de grupo para configurar clientes para o modo de cache hospedado

1. Em um computador no qual a função de servidor do Active Directory Domain Services está instalada, abra o Gerenciador do servidor, selecione o servidor Local, clique em **ferramentas**e, em seguida, clique em **Group Policy Management**. Abre o console de gerenciamento de diretiva de grupo.

2. No Console de Gerenciamento de Diretiva de Grupo, expanda o seguinte caminho: **Floresta:** *corp.contoso.com*, **domínios**, *corp.contoso.com*, **objetos de diretiva de grupo**, onde *corp.contoso.com* é o nome do domínio onde se encontram as contas de computador cliente BranchCache que você deseja configurar.

3. À direita\-clique em **objetos de diretiva de grupo**e, em seguida, clique em **New**. A caixa de diálogo **Novo GPO** é aberta. Na **nome**, digite um nome para o novo objeto de diretiva de grupo \(GPO\). Por exemplo, se desejar denominar o objeto como Computadores Cliente BranchCache, digite **Computadores Cliente BranchCache**. Clique em **OK**.

4. No console de gerenciamento de diretiva de grupo, verifique **Group Policy Objects** esteja selecionado e na parte direita do painel de detalhes\-clique no GPO que você acabou de criar. Por exemplo, se você nomeou seus computadores de cliente do BranchCache de GPO, com o botão direito\-clique em **os computadores cliente BranchCache**. Clique em **Editar**. O console do Editor de gerenciamento de diretiva de grupo é aberto.

5. No console do Editor de gerenciamento de diretiva de grupo, expanda o seguinte caminho: **Configuração do computador**, **diretivas**, **modelos administrativos: Definições de política \(arquivos ADMX\) recuperadas do computador local**, **rede**, **BranchCache**.

6. Clique em **BranchCache**e, em seguida, no painel de detalhes, clique duas vezes\-clique em **ativar o BranchCache**. A caixa de diálogo **Ativar o BranchCache** é aberta.
  
7.  Na caixa de diálogo **Ativar o BranchCache**, clique em **Habilitado** e em **OK**.

8. No console do Editor de gerenciamento de diretiva de grupo, certifique-se de que **BranchCache** é ainda selecionado e, em seguida, os duplos de painel de detalhes\-clique **habilitar Hosted Cache a descoberta automática por Conexão de serviço Ponto**. Abre a caixa de diálogo de configuração da política.

9. No **habilitar Hosted Cache a descoberta automática pelo ponto de Conexão de serviço** caixa de diálogo, clique em **Enabled**e, em seguida, clique em **Okey**.

10. Para permitir que computadores cliente download e o conteúdo do cache do servidor de arquivos BranchCache\-com base em servidores de conteúdo: No console do Editor de gerenciamento de diretiva de grupo, certifique-se de que **BranchCache** é ainda selecionado e, em seguida, os duplos de painel de detalhes\-clique em **BranchCache para arquivos de rede**. A caixa de diálogo **Configurar o BranchCache para arquivos de rede** é aberta. 
11. Na caixa de diálogo **Configurar o BranchCache para arquivos de rede**, clique em **Habilitado**. Em **Opções**, digite um valor numérico, em milissegundos, para o tempo máximo de latência de rede completa e clique em **OK**.
  
    > [!NOTE]
    > Por padrão, computadores cliente armazenam em cache conteúdo de servidores de arquivos se a latência de ida e volta de rede tiver mais de 80 milissegundos.
  
12. Para configurar a quantidade de espaço em disco rígido alocada em cada computador cliente para o cache do BranchCache: No console do Editor de gerenciamento de diretiva de grupo, certifique-se de que **BranchCache** é ainda selecionado e, em seguida, os duplos de painel de detalhes\-clique **definir a porcentagem do espaço em disco usada pelo cache do computador cliente**. A caixa de diálogo **Definir a porcentagem do espaço em disco usada pelo cache do computador cliente** é aberta. Clique em **Habilitado** e, em **Opções**, digite um valor numérico que represente a porcentagem de espaço no disco rígido usado em cada computador cliente para o cache do BranchCache. Clique em **OK**.

13. Para especificar a duração padrão, em dias, para os quais segmentos são válidos no cache de dados do BranchCache em computadores cliente: No console do Editor de gerenciamento de diretiva de grupo, certifique-se de que **BranchCache** é ainda selecionado e, em seguida, os duplos de painel de detalhes\-clique em **definir o tempo decorrido dos segmentos no cache de dados**. O **definir o tempo decorrido dos segmentos no cache de dados** caixa de diálogo é aberta. Clique em **Enabled**e, em seguida, no painel de detalhes, digite o número de dias que você preferir. Clique em **OK**.

14. Defina configurações de diretiva adicionais do BranchCache para computadores cliente conforme apropriado para sua implantação.

15. Atualizar diretiva de grupo em computadores cliente filiais executando o comando **gpupdate /force**, ou reinicializando os computadores cliente.

Sua implantação do modo de Cache hospedado do BranchCache está concluída.

Para obter informações adicionais sobre as tecnologias neste guia, consulte [recursos adicionais](11-Bc-Hcm-additional-resources.md).