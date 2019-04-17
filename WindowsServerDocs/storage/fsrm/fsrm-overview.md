---
title: "Visão Geral do FSRM (Gerenciador de Recursos de Servidor de Arquivos)"
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 7/7/2017
description: "O Gerenciador de Recursos de Servidor de Arquivos (FSRM) é uma ferramenta que permite que você gerencie e classifique dados em um servidor de arquivo do Windows Server."
ms.openlocfilehash: ddfc0a0f4bede89a3c3a624d4f128717d0f1ac08
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="file-server-resource-manager-fsrm-overview"></a>Visão Geral do FSRM (Gerenciador de Recursos de Servidor de Arquivos)

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

O Gerenciador de Recursos do Servidor de Arquivos (FSRM) é um serviço de função no Windows Server que permite que você gerencie e classifique dados armazenados nos servidores de arquivo. O Gerenciador de Recursos de Servidor de Arquivos inclui os seguintes recursos:
  
-   **Infraestrutura de Classificação de Dados** A Infraestrutura de Classificação de Dados oferece um panorama dos dados automatizando os processos de classificação para você poder gerenciar os dados com mais eficácia. Você pode classificar arquivos e aplicar políticas com base nesta classificação. As políticas de exemplo incluem controle de acesso dinâmico para restringir o acesso a arquivos, criptografia de arquivos e expiração de arquivos. Os arquivos podem ser classificados automaticamente por meio de regras de classificação de arquivo ou manualmente, modificando as propriedades de um arquivo ou uma pasta selecionada.  
  
-   **Tarefas de Gerenciamento de Arquivos** Tarefas de Gerenciamento de Arquivos permitem que você aplique uma ação ou política condicional de arquivos com base em sua classificação. As condições de uma tarefa de gerenciamento de arquivos incluem o local do arquivo, as propriedades de classificação, a data em que o arquivo foi criado, a data da última modificação do arquivo ou a última vez que o arquivo foi acessado. As ações que uma tarefa de gerenciamento de arquivos pode tomar incluem a capacidade de expirar arquivos, criptografar arquivos ou executar um comando personalizado.  
  
-   **Gerenciamento de cota** Cotas permitem limitar o espaço permitido para um volume ou pasta e podem ser aplicadas automaticamente a novas pastas que são criadas em um volume. Você também pode definir modelos de cota que podem ser aplicados a novos volumes ou pastas.  
  
-   **Gerenciamento de triagem de arquivos** Triagens de arquivos o ajudam a controlar os tipos de arquivos que o usuário pode armazenar em um servidor de arquivos. Você pode limitar a extensão que pode ser armazenada em seus arquivos compartilhados. Por exemplo, você pode criar uma tela de arquivos que não permite que arquivos com extensão MP3 sejam armazenados em pastas pessoais compartilhadas em um servidor de arquivos.  
  
-   **Relatórios de armazenamento** Relatórios de armazenamento são usados ​​para ajudar a identificar tendências no uso do disco e como os dados são classificados. Você também pode acompanhar um grupo selecionado de usuários para tentar salvar arquivos não autorizados.  
  
 Os recursos inclusos no Gerenciador de Recursos de Servidor de Arquivos podem ser configurados e gerenciados usando o Gerenciador de Recursos de Servidor de Arquivos MMC (Microsoft Management Console) ou usando o Windows PowerShell.  
  
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

## <a name="see-also"></a>Veja também

- [O que é novo no Gerenciador de Recursos de Servidores de Arquivos](https://technet.microsoft.com/library/dn383587.aspx)
- [Controle de Acesso Dinâmico](https://technet.microsoft.com/library/dn408191(v=ws.11).aspx) 