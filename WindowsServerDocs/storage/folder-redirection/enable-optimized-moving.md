---
title: Habilitar move otimizado de pastas redirecionadas
description: Como executar uma migração otimizada das pastas redirecionadas para um novo compartilhamento de arquivo.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 98fd5d50645ad454204dcf9dabf58e97c246ab1a
ms.sourcegitcommit: 505505b7ec99506e7ff50eddbdd6aa94a602abe6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2018
ms.locfileid: "3831376"
---
# Habilitar move otimizado de pastas redirecionadas

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Este tópico descreve como executar uma migração otimizada das pastas redirecionadas (redirecionamento de pasta) para um novo compartilhamento de arquivo. Se você habilitar essa configuração de política, quando um administrador move o compartilhamento de arquivos que hospeda pastas redirecionadas e atualiza o caminho de destino das pastas redirecionadas na política de grupo, o conteúdo em cache é simplesmente renomeado no cache local os arquivos Offline sem quaisquer atrasos ou perda de dados para o usuário.

Anteriormente, os administradores podem alterar o caminho de destino das pastas redirecionadas em política de grupo e permitir que os computadores cliente copiar os arquivos no próximo logon do usuário afetado, causando um sinal de atraso na. Como alternativa, os administradores podem mover o compartilhamento de arquivos e atualizar o caminho de destino das pastas redirecionadas na política de grupo. No entanto, as alterações feitas localmente nos computadores cliente entre o início da mudança e a primeira sincronização após a movimentação seriam perdidas.

## Pré-requisitos

Mover otimizado tem os seguintes requisitos:

- Redirecionamento de pasta deve ser configurado. Para obter mais informações, consulte [Implantar redirecionamento de pasta com arquivos Offline](deploy-folder-redirection.md).
- Os computadores cliente devem executar o Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.

## Etapa 1: Habilitar otimizado movimento na política de grupo

Para otimizar a realocação de dados de redirecionamento de pasta, use a política de grupo para habilitar a configuração de política **Ativar otimizado mover do conteúdo no cache de arquivos Offline na alteração de caminho de servidor de redirecionamento de pasta** para objeto de política de grupo (GPO) apropriado. Definir essa configuração de política como **desabilitado** ou **Não configurado** faz com que o cliente para copiar todo o conteúdo de redirecionamento de pasta para o novo local e, em seguida, exclua o conteúdo do local antigo se alterar o caminho do servidor.

Veja como habilitar a movimentação otimizado de pastas redirecionadas:

1. No gerenciamento de política de grupo, clique com botão direito no GPO que você criou para configurações de redirecionamento de pasta (por exemplo, **redirecionamento de pasta e configurações de perfis de usuário móvel**) e, em seguida, selecione **Editar**.
2. Em **Configuração do usuário**, navegue até **políticas**, em seguida, **Modelos administrativos**, em seguida, **sistema**e **Redirecionamento de pasta**.
3. Clique com botão direito **que habilitar otimizado mover do conteúdo no cache de arquivos Offline na alteração de caminho de servidor de redirecionamento de pasta**e, em seguida, selecione **Editar**.
4. Selecionar **ativado**e, em seguida, selecione **Okey**.

## Etapa 2: Realocar o compartilhamento de arquivos para pastas redirecionadas

Ao mover o compartilhamento de arquivo que contém dos usuários pastas redirecionadas, é importação tomar precauções para garantir que as pastas são realocadas corretamente.

>[!IMPORTANT]
>Se os arquivos dos usuários estão em usar ou se o estado completo do arquivo não é preservado na mudança, os usuários podem enfrentar um desempenho ruim como os arquivos são copiados pela rede, conflitos de sincronização geradas por arquivos Offline, ou até mesmo perda de dados.

1. Notifica os usuários com antecedência que o servidor que hospeda suas pastas redirecionadas alterar e recomendamos que eles executar as seguintes ações:

      - Sincronizar o conteúdo do cache seus arquivos Offline e resolver conflitos.
      - Abra um prompt de comando com privilégios elevados, digite **GpUpdate /Target:User /Force**e, em seguida, saia e entre novamente para garantir que as configurações de política de grupo mais recentes sejam aplicadas ao computador cliente

        >[!NOTE]
        >Por padrão, os computadores cliente atualizam a política de grupo a cada 90 minutos, portanto, se você permitir tempo suficiente para cliente computadores a receberem política atualizada, você não precisa solicitar que os usuários usem **GpUpdate**.
2. Remova o compartilhamento de arquivos do servidor para não garantir que nenhum arquivo no compartilhamento de arquivo em uso. Para fazer isso no Gerenciador do servidor, na página de **compartilhamentos** de arquivo e serviços de armazenamento, clique com botão direito no compartilhamento de arquivos apropriada e selecione **Remover**.

    Os usuários funcionará offline usando arquivos Offline até que a movimentação é concluída e recebem as configurações de redirecionamento de pasta atualizadas da política de grupo.

3. Usando uma conta com privilégios de backup, mover o conteúdo do compartilhamento de arquivo para o novo local usando um método que preserva carimbos de arquivo, como um backup e restauração do utilitário. Use o comando **Robocopy** , abra um prompt de comando com privilégios elevados e, em seguida, digite o seguinte comando, onde ```<Source>``` é o local atual do compartilhamento de arquivo, e ```<Destination>``` é o novo local:

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >O comando **Xcopy** não preserva todos os estados de arquivo.
4. Edite as configurações de política de redirecionamento de pasta, atualizando o local da pasta de destino para cada pasta redirecionada que você deseja realocar. Para obter mais informações, consulte a etapa 4 de [Implantar o redirecionamento de pasta com arquivos Offline](deploy-folder-redirection.md).
5. Notifica os usuários que o servidor que hospeda suas pastas redirecionadas foi alterado e que eles devem usar o comando **GpUpdate /Target:User /Force** e, em seguida, saia e entrar novamente para obter a configuração atualizada e continuar a sincronização de arquivo adequado.

    Os usuários devem fazer logon em todas as máquinas pelo menos uma vez garantir que os dados corretamente obtém realocados em cada cache de arquivos Offline.

## Mais informações

* [Implanta o redirecionamento de pasta com arquivos Offline](deploy-folder-redirection.md)
* [Implantar perfis de usuário móvel](deploy-roaming-user-profiles.md)
* [Visão geral de redirecionamento de pasta, arquivos Offline e perfis de usuário móvel](folder-redirection-rup-overview.md)