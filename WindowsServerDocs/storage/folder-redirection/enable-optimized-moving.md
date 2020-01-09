---
title: Habilitar movimentações otimizadas de pastas redirecionadas
description: Como executar uma movimentação otimizada de pastas redirecionadas para um novo compartilhamento de arquivos.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: edf714bc0d6b39dbe7c5e800e953d7820fe9abc5
ms.sourcegitcommit: bfe9c5f7141f4f2343a4edf432856f07db1410aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75352605"
---
# <a name="enable-optimized-moves-of-redirected-folders"></a>Habilitar movimentações otimizadas de pastas redirecionadas

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canal semestral)

Este tópico descreve como executar uma movimentação otimizada de pastas redirecionadas (redirecionamento de pasta) para um novo compartilhamento de arquivos. Se você habilitar essa configuração de política, quando um administrador mover o compartilhamento de arquivos que hospeda pastas redirecionadas e atualizar o caminho de destino das pastas redirecionadas no Política de Grupo, o conteúdo armazenado em cache será simplesmente renomeado no cache de Arquivos Offline local sem atrasos ou potencial perda de dados para o usuário.

Anteriormente, os administradores podiam alterar o caminho de destino das pastas redirecionadas em Política de Grupo e deixar que os computadores cliente copiem os arquivos na próxima entrada do usuário afetado, causando um logon atrasado. Como alternativa, os administradores podem mover o compartilhamento de arquivos e atualizar o caminho de destino das pastas redirecionadas no Política de Grupo. No entanto, todas as alterações feitas localmente nos computadores cliente entre o início da movimentação e a primeira sincronização após a movimentação seriam perdidas.

## <a name="prerequisites"></a>Pré-requisitos

A movimentação otimizada tem os seguintes requisitos:

- O redirecionamento de pasta deve ser configurado. Para obter mais informações, consulte [implantar redirecionamento de pasta com arquivos offline](deploy-folder-redirection.md).
- Os computadores cliente devem executar o Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server (canal semestral).

## <a name="step-1-enable-optimized-move-in-group-policy"></a>Etapa 1: habilitar movimentação otimizada no Política de Grupo

Para otimizar a realocação de dados de redirecionamento de pasta, use Política de Grupo para habilitar a opção **habilitar movimentação otimizada de conteúdo no cache arquivos offline no caminho do servidor de redirecionamento de pasta alterar** política para o objeto de política de grupo apropriado (GPO). Definir essa configuração de política como **desabilitada** ou **não definida** faz com que o cliente Copie todo o conteúdo de redirecionamento de pasta para o novo local e, em seguida, exclua o conteúdo do local antigo se o caminho do servidor for alterado.

Veja como habilitar a movimentação otimizada de pastas redirecionadas:

1. Em gerenciamento de Política de Grupo, clique com o botão direito do mouse no GPO criado para as configurações de redirecionamento de pasta (por exemplo, **configurações de redirecionamento de pasta e perfis de usuário de roaming**) e, em seguida, selecione **Editar**.
2. Em **configuração do usuário**, navegue até **políticas**, **modelos administrativos**, **sistema**e **redirecionamento de pasta**.
3. Clique com o botão direito do mouse em **habilitar movimentação otimizada de conteúdo em arquivos offline cache no caminho do servidor de redirecionamento de pasta alterar**e selecione **Editar**.
4. Selecione **habilitado**e, em seguida, selecione **OK**.

## <a name="step-2-relocate-the-file-share-for-redirected-folders"></a>Etapa 2: realocar o compartilhamento de arquivos para pastas redirecionadas

Ao mover o compartilhamento de arquivos que contém pastas redirecionadas dos usuários, é importante tomar precauções para garantir que as pastas sejam realocadas corretamente.

>[!IMPORTANT]
>Se os arquivos dos usuários estiverem em uso ou se o estado completo do arquivo não for preservado na movimentação, os usuários poderão experimentar um desempenho ruim à medida que os arquivos forem copiados pela rede, os conflitos de sincronização gerados por Arquivos Offline ou até mesmo perda de dados.

1. Notifique os usuários antecipadamente que o servidor que hospeda suas pastas redirecionadas será alterado e recomende que eles executem as seguintes ações:

      - Sincronize o conteúdo de seu cache Arquivos Offline e resolva quaisquer conflitos.
      - Abra um prompt de comando com privilégios elevados, digite **gpupdate/target: user/Force**e saia e entre novamente para garantir que as últimas configurações de política de grupo sejam aplicadas ao computador cliente

        >[!NOTE]
        >Por padrão, os computadores cliente são atualizados Política de Grupo a cada 90 minutos, portanto, se você permitir tempo suficiente para que os computadores cliente recebam a política atualizada, não será necessário solicitar que os usuários usem o **gpupdate**.
2. Remova o compartilhamento de arquivos do servidor para garantir que nenhum arquivo no compartilhamento de arquivos esteja em uso. Para fazer isso no Gerenciador do Servidor, na página **compartilhamentos** de serviços de arquivo e armazenamento, clique com o botão direito do mouse no compartilhamento de arquivos apropriado e selecione **remover**.

    Os usuários trabalharão offline usando Arquivos Offline até que a movimentação seja concluída e recebam as configurações de redirecionamento de pasta atualizadas de Política de Grupo.

3. Usando uma conta com privilégios de backup, mova o conteúdo do compartilhamento de arquivos para o novo local usando um método que preserva os carimbos de data/hora do arquivo, como um utilitário de backup e restauração. Para usar o comando **Robocopy** , abra um prompt de comando com privilégios elevados e digite o seguinte comando, em que ```<Source>``` é o local atual do compartilhamento de arquivos e ```<Destination>``` é o novo local:

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >O comando **xcopy** não preserva todo o estado do arquivo.
4. Edite as configurações de política de redirecionamento de pasta, atualizando o local da pasta de destino para cada pasta redirecionada que você deseja realocar. Para obter mais informações, consulte a etapa 4 de [implantar redirecionamento de pasta com arquivos offline](deploy-folder-redirection.md).
5. Notifique os usuários que o servidor que hospeda suas pastas redirecionadas foi alterado e que eles devem usar o comando **gpupdate/target: user/Force** e, em seguida, sair e entrar novamente para obter a configuração atualizada e retomar a sincronização de arquivo adequada.

    Os usuários devem fazer logon em todos os computadores pelo menos uma vez para garantir que os dados sejam realocados corretamente em cada cache de Arquivos Offline.

## <a name="more-information"></a>Mais informações

* [Implantar redirecionamento de pasta com Arquivos Offline](deploy-folder-redirection.md)
* [Implantar perfis de usuário móveis](deploy-roaming-user-profiles.md)
* [Visão geral dos perfis de usuário de redirecionamento de pasta, Arquivos Offline e roaming](folder-redirection-rup-overview.md)