---
title: Visão Geral do FSRM (Gerenciador de Recursos de Servidor de Arquivos)
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 5/14/2018
description: Gerenciador de recursos de servidor de arquivos (FSRM) é uma ferramenta que lhe permite gerenciar e classificar dados em um servidor de arquivos do Windows Server.
ms.openlocfilehash: 8488c7418ac03be53db7164678fad353bc7c637d
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476132"
---
# <a name="file-server-resource-manager-fsrm-overview"></a>Visão Geral do FSRM (Gerenciador de Recursos de Servidor de Arquivos)

> Aplica-se a: 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server (canal semestral), do Windows Server 

O Gerenciador de Recursos do Servidor de Arquivos (FSRM) é um serviço de função no Windows Server que permite que você gerencie e classifique dados armazenados nos servidores de arquivo. Você pode usar o Gerenciador de recursos de servidor de arquivos para classificar arquivos, executar tarefas com base nessas classificações, definir cotas nas pastas e criar relatórios para monitorar o uso de armazenamento.

É também um ponto de pequeno, mas podemos [adicionada a capacidade de desabilitar a alteração diários](#whats-new) no Windows Server, versão 1803.

## <a name="features"></a>Recursos

O Gerenciador de Recursos de Servidor de Arquivos inclui os seguintes recursos:

-   [Gerenciamento de cotas](quota-management.md) permite limitar o espaço permitido para um volume ou pasta e podem ser aplicadas automaticamente a novas pastas que são criadas em um volume. Você também pode definir modelos de cota que podem ser aplicados a novos volumes ou pastas.  
-   [Infraestrutura de classificação](classification-management.md) fornece ideias sobre seus dados, automatizando processos de classificação para que você possa gerenciar seus dados com mais eficiência. Você pode classificar arquivos e aplicar políticas com base nesta classificação. As políticas de exemplo incluem controle de acesso dinâmico para restringir o acesso a arquivos, criptografia de arquivos e expiração de arquivos. Os arquivos podem ser classificados automaticamente por meio de regras de classificação de arquivo ou manualmente, modificando as propriedades de um arquivo ou uma pasta selecionada.
-   [Tarefas de gerenciamento de arquivos](file-management-tasks.md) permite que você aplique uma ação ou política condicional a arquivos com base em sua classificação. As condições de uma tarefa de gerenciamento de arquivos incluem o local do arquivo, as propriedades de classificação, a data em que o arquivo foi criado, a data da última modificação do arquivo ou a última vez que o arquivo foi acessado. As ações que uma tarefa de gerenciamento de arquivos pode tomar incluem a capacidade de expirar arquivos, criptografar arquivos ou executar um comando personalizado.
-   [Gerenciamento de triagem de arquivo](file-screening-management.md) ajuda a controlar os tipos de arquivos que o usuário pode armazenar em um servidor de arquivos. Você pode limitar a extensão que pode ser armazenada em seus arquivos compartilhados. Por exemplo, você pode criar uma tela de arquivos que não permite que arquivos com extensão MP3 sejam armazenados em pastas pessoais compartilhadas em um servidor de arquivos.
-   [Relatórios de armazenamento](storage-reports-management.md) ajudá-lo a identificar tendências no uso do disco e como os dados são classificados. Você também pode acompanhar um grupo selecionado de usuários para tentar salvar arquivos não autorizados.  
  
Os recursos incluídos com o Gerenciador de recursos de servidor de arquivos podem ser configurados e gerenciados usando o aplicativo Gerenciador de recursos de servidor de arquivos ou por meio do Windows PowerShell.
  
> [!IMPORTANT]
>  O Gerenciador de Recursos de Servidor de Arquivos suporta somente volumes formatados com o sistema de arquivos NTFS. O Sistema de Arquivos Resiliente não conta com suporte.  
  
## <a name="practical-applications"></a>Aplicações práticas  
 Algumas aplicações práticas para o Gerenciador de Recursos de Servidor de Arquivos incluem:  
  
-   Usar a Infraestrutura de Classificação de Arquivos com o cenário de Controle de Acesso Dinâmico para criar uma política que garanta acesso a arquivos e pastas com base na forma como os arquivos são classificados no servidor de arquivos.  
  
-   Criar uma regra de classificação de arquivos que marque qualquer arquivo que contenha pelo menos 10 números do Seguro Social com informações de identificação pessoal.  
  
-   Expirar qualquer arquivo que não tenha sido modificado nos últimos 10 anos.  
  
-   Criar uma cota de 200 megabytes para cada diretório principal do usuário e notificá-lo quando estiver usando 180 megabytes.  
  
-   Não permitir que arquivos de música sejam armazenados em pastas pessoais compartilhadas.  
  
-   Agendar um relatório que será executado todo domingo à meia-noite, gerando uma lista que inclui os arquivos acessados mais recentemente dos dois dias anteriores. Isto pode ajudar a determinar a atividade de armazenamento do fim de semana e planejar o tempo de inatividade do seu servidor adequadamente.  

## <a name="whats-new"></a>O que há de novo – impedir que o FSRM criar diários de alteração

Começando com o Windows Server, versão 1803, agora você pode evitar o serviço do Gerenciador de recursos de servidor de arquivos desde a criação de um diário de alterações (também conhecido como um diário de USN) em volumes quando o serviço é iniciado. Isso pode economizar um pouco de espaço em cada volume, mas desabilitará a classificação de arquivos em tempo real.

Para novos recursos mais antigos, consulte [o que há de novo no Gerenciador de recursos de servidor de arquivos](https://technet.microsoft.com/library/dn383587.aspx).

Para impedir que o Gerenciador de recursos de servidor de arquivos criando um diário de alterações em alguns ou todos os volumes quando o serviço é iniciado, use as seguintes etapas: 

1. Interrompa o serviço SRMSVC. Por exemplo, abra uma sessão do PowerShell como administrador e digite `Stop-Service SrmSvc`.
2. Exclua o diário de USN para volumes que você deseja economizar espaço na usando o comando fsutil: 

      ```
      fsutil usn deletejournal /d <VolumeName>
      ```
    Por exemplo: `fsutil usn deletejournal /d c:`

3. Abra o Editor do registro, por exemplo, digitando `regedit` na mesma sessão do PowerShell.
4. Navegue até a seguinte chave: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SrmSvc\Settings**
5. Opcionalmente, para ignorar alterar criação de diário para o servidor inteiro (pule esta etapa se você quiser desabilitá-lo somente em volumes específicos):
    1. Clique com botão direito do **configurações** da chave e, em seguida, selecione **New** > **valor DWORD (32 bits)** . 
    1. O nome do valor `SkipUSNCreationForSystem`.
    1. Defina o valor como **1** (em hexadecimal).
6. Para opcionalmente ignorar a criação do diário de alteração para volumes específicos:
    1. Obter os caminhos de volume que você deseja ignorar usando o `fsutil volume list` comando ou o seguinte comando do PowerShell:
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
    2. Com o botão direito novamente no Editor do registro, o **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SrmSvc\Settings** da chave e, em seguida, selecione **New** > **várias cadeias de caracteres Valor**.
    3. O nome do valor `SkipUSNCreationForVolumes`.
    4. Insira o caminho de cada volume no qual você ignorar a criação de um diário de alterações, colocando cada caminho em uma linha separada. Por exemplo: 

        ```
        \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
        ```

        > [!NOTE] 
        > Editor do registro pode dizer que você remova as cadeias de caracteres vazias, exibir este aviso de que você poderá desconsiderar: *Dados de tipo REG_MULTI_SZ não podem conter cadeias de caracteres vazias. Editor do registro removerá todas as cadeias de caracteres vazias encontradas.*

7. Inicie o serviço SRMSVC. Por exemplo, em uma sessão do PowerShell, digite `Start-Service SrmSvc`.



## <a name="see-also"></a>Consulte também

- [Controle de acesso dinâmico](https://technet.microsoft.com/library/dn408191(v=ws.11).aspx) 