---
title: Habilitar movimentos otimizados de pastas redirecionadas
description: Como realizar uma mudança otimizada de pastas redirecionadas para um novo compartilhamento de arquivos.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7bdf30a4f721568add4e7902245da2a803b72db1
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475865"
---
# <a name="enable-optimized-moves-of-redirected-folders"></a>Habilitar movimentos otimizados de pastas redirecionadas

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canal semestral)

Este tópico descreve como executar uma mudança otimizada de pastas redirecionadas (redirecionamento de pasta) para um novo compartilhamento de arquivos. Se você habilitar essa configuração de política, quando um administrador move o compartilhamento de arquivos que hospedam as pastas redirecionadas e atualiza o caminho de destino das pastas redirecionadas na diretiva de grupo, o conteúdo em cache simplesmente renomeado no cache de arquivos Offline local sem atrasos ou perda de dados para o usuário.

Anteriormente, os administradores poderiam alterar o caminho de destino das pastas redirecionadas na diretiva de grupo e permitem que os computadores cliente copiar os arquivos no próximo logon do usuário afetado, causando uma atraso de entrada. Como alternativa, os administradores podem mover o compartilhamento de arquivos e atualizar o caminho de destino das pastas redirecionadas na diretiva de grupo. No entanto, todas as alterações feitas localmente nos computadores cliente entre o início da movimentação e a primeira sincronização após a movimentação seriam perdidas.

## <a name="prerequisites"></a>Pré-requisitos

Mover otimizado tem os seguintes requisitos:

- Redirecionamento de pasta deve ser configurado. Para obter mais informações, consulte [implantar o redirecionamento de pasta com arquivos Offline](deploy-folder-redirection.md).
- Computadores cliente devem executar o Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server (canal semestral).

## <a name="step-1-enable-optimized-move-in-group-policy"></a>Etapa 1: Habilitar otimizada mudança na política de grupo

Para otimizar a realocação de dados de redirecionamento de pasta, use a diretiva de grupo para habilitar o **Enable otimizado mover de conteúdo no cache de arquivos off-line na alteração de caminho de servidor de redirecionamento de pasta** configuração de política para a política de grupo apropriada GPO (objeto). Definir essa configuração de política como **desabilitados** ou **não configurado** faz com que o cliente copiar todo o conteúdo de redirecionamento de pasta para o novo local e, em seguida, exclua o conteúdo do local antigo se a alterações de caminho do servidor.

Aqui está como permitem mover otimizado de pastas redirecionadas:

1. Em gerenciamento de diretiva de grupo, clique com botão direito no GPO criado por você para as configurações de redirecionamento de pasta (por exemplo, **redirecionamento de pastas e configurações de perfis de usuário móvel**) e, em seguida, selecione **editar**.
2. Sob **configuração do usuário**, navegue até **diretivas**, em seguida, **modelos administrativos**, em seguida, **sistema**, em seguida,  **Redirecionamento de pasta**.
3. Clique com botão direito **Enable otimizado mover de conteúdo no cache de arquivos off-line na alteração de caminho de servidor de redirecionamento de pasta**e, em seguida, selecione **editar**.
4. Selecione **Enabled**e, em seguida, selecione **Okey**.

## <a name="step-2-relocate-the-file-share-for-redirected-folders"></a>Etapa 2: Realocar o compartilhamento de arquivos para pastas redirecionadas

Ao mover o compartilhamento de arquivo que contém usuários pastas redirecionadas, é importante tomar precauções para garantir que as pastas são realocadas corretamente.

>[!IMPORTANT]
>Se os arquivos dos usuários estão em usar, ou se o estado de arquivo completo não é preservado na mudança, os usuários podem enfrentar um desempenho ruim, pois os arquivos são copiados pela rede, conflitos de sincronização, gerados por arquivos Offline, ou até mesmo a perda de dados.

1. Notificar os usuários com antecedência que o servidor que hospeda suas pastas redirecionadas alterar e recomendamos que eles executam as seguintes ações:

      - Sincronizar o conteúdo de seu cache de arquivos Offline e resolva os conflitos.
      - Abra um prompt de comando com privilégios elevados, digite **GpUpdate /Target:User /Force**e, em seguida, saia e entre novamente para garantir que as configurações de diretiva de grupo mais recentes sejam aplicadas ao computador cliente

        >[!NOTE]
        >Por padrão, os computadores cliente atualizam política de grupo a cada 90 minutos, portanto, se você permitir tempo suficiente para o cliente computadores recebam a política atualizada, você precisa solicitar que os usuários usem **GpUpdate**.
2. Remova o compartilhamento de arquivos do servidor para garantir que nenhum arquivo no compartilhamento de arquivos está em uso. Para fazer isso no Gerenciador do servidor, nos **compartilhamentos** página de arquivo e serviços de armazenamento, o compartilhamento de arquivos apropriada com o botão direito e selecione **remover**.

    Os usuários trabalharão offline usando arquivos Offline até que a migração for concluída e eles recebem as configurações de redirecionamento de pasta atualizadas da diretiva de grupo.

3. Usando uma conta com privilégios de backup, mover o conteúdo do compartilhamento de arquivo para o novo local usando um método que preserva os carimbos de hora de arquivo, como um backup e restauração do utilitário. Para usar o **Robocopy** de comando, abra um prompt de comando com privilégios elevados e, em seguida, digite o comando a seguir, onde ```<Source>``` é o local atual do compartilhamento de arquivo, e ```<Destination>``` é o novo local:

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >O **Xcopy** comando não preserva todos os estados de arquivo.
4. Edite as configurações de política de redirecionamento de pasta, atualizando o local da pasta de destino para cada pasta redirecionada que você deseja transferir. Para obter mais informações, consulte a etapa 4 do [implantar o redirecionamento de pasta com arquivos Offline](deploy-folder-redirection.md).
5. Notificar os usuários que o servidor que hospeda suas pastas redirecionadas foi alterado, e que eles devem usar o **GpUpdate /Target:User /Force** de comando e, em seguida, saia e entre novamente para obter a configuração atualizada e retomar adequadas de arquivo sincronização.

    Os usuários devem entrar todas as máquinas pelo menos uma vez para garantir que os dados corretamente realocados em cada cache de arquivos Offline.

## <a name="more-information"></a>Mais informações

* [Implantar o redirecionamento de pasta com arquivos Offline](deploy-folder-redirection.md)
* [Implantar perfis de usuário móveis](deploy-roaming-user-profiles.md)
* [Visão geral do redirecionamento de pasta, arquivos Offline e perfis de usuário móvel](folder-redirection-rup-overview.md)