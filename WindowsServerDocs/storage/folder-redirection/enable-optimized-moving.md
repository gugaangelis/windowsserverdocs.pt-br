---
title: Habilitar movimentações otimizadas de pastas redirecionadas
description: Como executar uma movimentação otimizada de pastas redirecionadas para um novo compartilhamento de arquivo.
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 895bf41a5a42bd4578e99ba12205b56da18b9ba2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87942186"
---
# <a name="enable-optimized-moves-of-redirected-folders"></a>Habilitar movimentações otimizadas de pastas redirecionadas

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (Canal Semestral)

Este tópico descreve como executar uma movimentação otimizada de pastas redirecionadas (Redirecionamento de Pastas) para um novo compartilhamento de arquivo. Se você habilitar essa configuração de política, quando um administrador mover o compartilhamento de arquivo que hospeda pastas redirecionadas e atualizar o caminho de destino elas na Política de Grupo, o conteúdo do cache será simplesmente renomeado no cache de Arquivos Offline local sem nenhum atraso ou perda de dados potencial para o usuário.

Anteriormente, os administradores podiam alterar o caminho de destino das pastas redirecionadas em Política de Grupo e deixar os computadores cliente copiarem os arquivos na próxima entrada do usuário afetado, causando um atraso na entrada. Ou os administradores podiam mover o compartilhamento de arquivo e atualizar o caminho de destino das pastas redirecionadas na Política de Grupo. No entanto, as alterações feitas localmente nos computadores cliente entre o início da movimentação e a primeira sincronização após a movimentação seriam perdidas.

## <a name="prerequisites"></a>Pré-requisitos

A movimentação otimizada tem os seguintes requisitos:

- O Redirecionamento de Pastas deve estar configurado. Para obter mais informações, confira [Implantar o Redirecionamento de Pastas com Arquivos Offline](deploy-folder-redirection.md).
- Os computadores cliente devem executar o Windows 10, o Windows 8.1, o Windows 8, o Windows Server 2019, o Windows Server 2016, o Windows Server 2012 R2, o Windows Server 2012 ou o Windows Server (Canal Semestral).

## <a name="step-1-enable-optimized-move-in-group-policy"></a>Etapa 1: Habilitar a movimentação otimizada na Política de Grupo

Para otimizar a realocação dos dados do Redirecionamento de Pastas, use a Política de Grupo para habilitar a configuração de política **Habilitar a movimentação otimizada de conteúdo no cache de Arquivos Offline na alteração do caminho do servidor do Redirecionamento de Pastas** para o GPO (Objeto de Política de Grupo) adequado. Definir essa configuração de política como **Desabilitada** ou **Não configurada** fará o cliente copiar todo o conteúdo do Redirecionamento de Pastas para a nova localização e excluir o conteúdo da localização antiga se o caminho for alterado.

Veja como habilitar a movimentação otimizada de pastas redirecionadas:

1. Em Gerenciamento de Política de Grupo, clique com o botão direito do mouse no GPO que você criou para as configurações do Redirecionamento de Pastas (por exemplo, **Configurações de Redirecionamento de Pastas e Configurações de Perfis de Usuários Móveis**) e, em seguida, selecione **Editar**.
2. Em **Configuração do Usuário**, navegue até **Políticas**, **Modelos Administrativos**, **Sistema** e **Redirecionamento de Pastas**.
3. Clique com o botão direito do mouse em **Habilitar a movimentação otimizada de conteúdo no cache de Arquivos Offline na alteração do caminho do servidor do Redirecionamento de Pastas** e, em seguida, selecione **Editar**.
4. Selecione **Habilitado** e, em seguida, selecione **OK**.

## <a name="step-2-relocate-the-file-share-for-redirected-folders"></a>Etapa 2: Relocar o compartilhamento de arquivo para pastas redirecionadas

Ao mover o compartilhamento de arquivo que contém as pastas redirecionadas dos usuários, é importante tomar precauções para verificar se as pastas foram devidamente relocadas.

>[!IMPORTANT]
>Se os arquivos dos usuários estiverem em uso ou se o estado completo do arquivo não for preservado na movimentação, os usuários poderão enfrentar baixo desempenho enquanto os arquivos são copiados pela rede, conflitos de sincronização gerados por Arquivos Offline ou até mesmo perda de dados.

1. Notifique os usuários antecipadamente de que o servidor que hospeda as pastas redirecionadas deles será alterado e recomende que executem as seguintes ações:

      - Sincronizar o conteúdo do cache de Arquivos Offline deles e resolver conflitos.
      - Abrir um prompt de comandos com privilégios elevados, inserir **GpUpdate /Target:User /Force**, sair e entrar novamente para verificar se as configurações mais recentes da Política de Grupo foram aplicadas ao computador cliente

        >[!NOTE]
        >Por padrão, os computadores cliente atualizam a Política de Grupo a cada 90 minutos. Portanto, se você permitir que os computadores cliente tenham tempo suficiente para receber a política atualizada, não será necessário solicitar que os usuários usem **GpUpdate**.
2. Remova o compartilhamento de arquivo do servidor para que nenhum arquivo nele esteja em uso. Para fazer isso no Gerenciador do Servidor, na página **Compartilhamentos** dos Serviços de Arquivo e Armazenamento, clique com o botão direito do mouse no compartilhamento de arquivo apropriado e selecione **Remover**.

    Os usuários trabalharão offline usando os Arquivos Offline até que a movimentação seja concluída e que eles recebam as configurações de Redirecionamento de Pastas atualizadas da Política de Grupo.

3. Usando uma conta com privilégios de backup, mova o conteúdo do compartilhamento de arquivo para a nova localização usando um método que preserva os carimbos de data/hora do arquivo, como um utilitário de backup e restauração. Para usar o comando **Robocopy**, abra um prompt de comandos com privilégios elevados e digite o seguinte comando, em que ```<Source>``` é a localização atual do compartilhamento de arquivo e ```<Destination>``` é a nova localização:

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >O comando **Xcopy** não preserva todo o estado do arquivo.
4. Edite as configurações da política de Redirecionamento de Pastas, atualizando a localização da pasta de destino para cada pasta redirecionada que você deseja relocar. Para obter mais informações, confira a Etapa 4 de [Implantar o Redirecionamento de Pastas com Arquivos Offline](deploy-folder-redirection.md).
5. Notifique os usuários de que o servidor que hospeda as pastas redirecionadas deles foi alterado e que eles devem usar o comando **GpUpdate /Target:User /Force** e, em seguida, saia e entre novamente para obter a configuração atualizada e retomar a sincronização de arquivo adequada.

    Os usuários devem fazer logon em todos os computadores pelo menos uma vez para verificar se os dados foram realocados corretamente em cada cache de Arquivos Offline.

## <a name="more-information"></a>Mais informações

* [Implantar Redirecionamento de Pastas com Arquivos Offline](deploy-folder-redirection.md)
* [Implantar perfis de usuário móveis](deploy-roaming-user-profiles.md)
* [Visão geral de Redirecionamento de Pastas, Arquivos Offline e Perfis de Usuários Móveis](folder-redirection-rup-overview.md)