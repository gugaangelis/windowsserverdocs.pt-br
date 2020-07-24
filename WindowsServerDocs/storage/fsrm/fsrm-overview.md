---
title: Visão Geral do FSRM (Gerenciador de Recursos de Servidor de Arquivos)
ms.prod: windows-server
ms.author: jgerend
manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 5/14/2018
description: O FSRM (Gerenciador de recursos de servidor de arquivos) é uma ferramenta que permite gerenciar e classificar dados em um servidor de arquivos do Windows Server.
ms.openlocfilehash: 58b410e51dae3ea102bb1a15f5bb60f00ab702fa
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86964558"
---
# <a name="file-server-resource-manager-fsrm-overview"></a>Visão Geral do FSRM (Gerenciador de Recursos de Servidor de Arquivos)

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server (canal semestral),

O Gerenciador de Recursos do Servidor de Arquivos (FSRM) é um serviço de função no Windows Server que permite que você gerencie e classifique dados armazenados nos servidores de arquivo. Você pode usar o Gerenciador de recursos de servidor de arquivos para classificar automaticamente os arquivos, executar tarefas com base nessas classificações, definir cotas em pastas e criar relatórios monitorando o uso do armazenamento.

É um pequeno ponto, mas também [adicionamos a capacidade de desabilitar os diários de alterações](#whats-new) no Windows Server, versão 1803.

## <a name="features"></a>Recursos

O Gerenciador de Recursos de Servidor de Arquivos inclui os seguintes recursos:

-   O [Gerenciamento de cotas](quota-management.md) permite limitar o espaço permitido para um volume ou uma pasta, e eles podem ser aplicados automaticamente a novas pastas criadas em um volume. Você também pode definir modelos de cota que podem ser aplicados a novos volumes ou pastas.
-   A [infraestrutura de classificação de arquivos](classification-management.md) fornece informações sobre seus dados automatizando os processos de classificação para que você possa gerenciar seus dados com mais eficiência. Você pode classificar arquivos e aplicar políticas com base nesta classificação. As políticas de exemplo incluem controle de acesso dinâmico para restringir o acesso a arquivos, criptografia de arquivos e expiração de arquivos. Os arquivos podem ser classificados automaticamente por meio de regras de classificação de arquivo ou manualmente, modificando as propriedades de um arquivo ou uma pasta selecionada.
-   [As tarefas de gerenciamento de arquivos](file-management-tasks.md) permitem que você aplique uma política condicional ou uma ação a arquivos com base em sua classificação. As condições de uma tarefa de gerenciamento de arquivos incluem o local do arquivo, as propriedades de classificação, a data em que o arquivo foi criado, a data da última modificação do arquivo ou a última vez que o arquivo foi acessado. As ações que uma tarefa de gerenciamento de arquivos pode tomar incluem a capacidade de expirar arquivos, criptografar arquivos ou executar um comando personalizado.
-   O [Gerenciamento de triagens de arquivos](file-screening-management.md) ajuda a controlar os tipos de arquivos que o usuário pode armazenar em um servidor de arquivos. Você pode limitar a extensão que pode ser armazenada em seus arquivos compartilhados. Por exemplo, você pode criar uma tela de arquivos que não permite que arquivos com extensão MP3 sejam armazenados em pastas pessoais compartilhadas em um servidor de arquivos.
-   Os [relatórios de armazenamento](storage-reports-management.md) ajudam a identificar tendências no uso do disco e como seus dados são classificados. Você também pode acompanhar um grupo selecionado de usuários para tentar salvar arquivos não autorizados.

Os recursos incluídos no Gerenciador de recursos de servidor de arquivos podem ser configurados e gerenciados usando o aplicativo do Gerenciador de recursos do servidor de arquivos ou usando o Windows PowerShell.

> [!IMPORTANT]
>  O Gerenciador de Recursos de Servidor de Arquivos suporta somente volumes formatados com o sistema de arquivos NTFS. O Sistema de Arquivos Resiliente não conta com suporte.

## <a name="practical-applications"></a>Aplicações práticas
 Algumas aplicações práticas para o Gerenciador de Recursos de Servidor de Arquivos incluem:

-   Usar a Infraestrutura de Classificação de Arquivos com o cenário de Controle de Acesso Dinâmico para criar uma política que garanta acesso a arquivos e pastas com base na forma como os arquivos são classificados no servidor de arquivos.

-   Criar uma regra de classificação de arquivos que marque qualquer arquivo que contenha pelo menos 10 números do Seguro Social com informações de identificação pessoal.

-   Expirar qualquer arquivo que não tenha sido modificado nos últimos 10 anos.

-   Crie uma cota de 200 megabytes para o diretório base de cada usuário e notifique-os quando eles estiverem usando 180 megabytes.

-   Não permitir que arquivos de música sejam armazenados em pastas pessoais compartilhadas.

-   Agendar um relatório que será executado todo domingo à meia-noite, gerando uma lista que inclui os arquivos acessados mais recentemente dos dois dias anteriores. Isto pode ajudar a determinar a atividade de armazenamento do fim de semana e planejar o tempo de inatividade do seu servidor adequadamente.

## <a name="whats-new---prevent-fsrm-from-creating-change-journals"></a><a name="whats-new"></a>O que há de novo – impedir que o FSRM crie diários de alterações

A partir do Windows Server, versão 1803, agora você pode impedir que o serviço do Gerenciador de recursos do servidor de arquivos crie um diário de alterações (também conhecido como um diário de USN) em volumes quando o serviço for iniciado. Isso pode conservar um pouco de espaço em cada volume, mas desabilitará a classificação de arquivos em tempo real.

Para novos recursos mais antigos, consulte [novidades no Gerenciador de recursos do servidor de arquivos](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383587(v=ws.11)).

Para impedir que o Gerenciador de recursos do servidor de arquivos crie um diário de alterações em alguns ou todos os volumes quando o serviço for iniciado, use as seguintes etapas:

1. Pare o serviço SRMSVC. Por exemplo, abra uma sessão do PowerShell como administrador e digite `Stop-Service SrmSvc` .
2. Exclua o diário de USN dos volumes nos quais você deseja conservar espaço usando o comando fsutil:

      ```
      fsutil usn deletejournal /d <VolumeName>
      ```
    Por exemplo: `fsutil usn deletejournal /d c:`

3. Abra o editor do registro, por exemplo, digitando `regedit` a mesma sessão do PowerShell.
4. Navegue até a seguinte chave: **HKEY_LOCAL_MACHINE \system\currentcontrolset\services\srmsvc\settings**
5. Para, opcionalmente, ignorar a criação do diário de alterações para todo o servidor (pule esta etapa se quiser desabilitá-la somente em volumes específicos):
    1. Clique com o botão direito do mouse na chave de **configurações** e selecione **novo**  >  **valor DWORD (32 bits)**.
    1. Nomeie o valor `SkipUSNCreationForSystem` .
    1. Defina o valor como **1** (em hexadecimal).
6. Para, opcionalmente, ignorar a criação do diário de alterações para volumes específicos:
    1. Obtenha os caminhos de volume que você deseja ignorar usando o `fsutil volume list` comando ou o seguinte comando do PowerShell:
        ```PowerShell
        Get-Volume | Format-Table DriveLetter,FileSystemLabel,Path
        ```
       Aqui está um exemplo de saída:

       ```
        DriveLetter FileSystemLabel Path
        ----------- --------------- ----
                    System Reserved \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        C                           \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
       ```
    2. De volta ao editor do registro, clique com o botão direito do mouse na chave **HKEY_LOCAL_MACHINE \system\currentcontrolset\services\srmsvc\settings** e selecione **novo**  >  **valor de cadeia de caracteres múltipla**.
    3. Nomeie o valor `SkipUSNCreationForVolumes` .
    4. Insira o caminho de cada volume no qual você ignora a criação de um diário de alterações, colocando cada caminho em uma linha separada. Por exemplo:

        ```
        \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
        ```

        > [!NOTE]
        > O editor do registro pode informar que removeu cadeias de caracteres vazias, exibindo este aviso de que você pode desconsiderar com segurança: *dados do tipo REG_MULTI_SZ não podem conter cadeias de caracteres vazias. O editor do registro removerá todas as cadeias de caracteres vazias encontradas.*

7. Inicie o serviço SRMSVC. Por exemplo, em uma sessão do PowerShell, insira `Start-Service SrmSvc` .



## <a name="additional-references"></a>Referências adicionais

- [Controle de Acesso Dinâmico](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn408191(v=ws.11))
