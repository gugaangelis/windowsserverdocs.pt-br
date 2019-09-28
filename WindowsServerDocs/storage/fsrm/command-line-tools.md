---
title: Ferramentas de linha de comando do Gerenciador de Recursos de Servidor de Arquivos
description: Este artigo descreve as ferramentas de linha de comando do Windows Server 2016
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 78c054c5b0c3de19d1f3acd825335eab2f140541
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394303"
---
# <a name="file-server-resource-manager-command-line-tools"></a>Ferramentas de linha de comando do Gerenciador de Recursos de Servidor de Arquivos

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Gerenciador de recursos do servidor de arquivos instala o cmdlets do PowerShell [FileServerResourceManager](https://technet.microsoft.com/itpro/powershell/windows/fileserverresourcemanager/fileserverresourcemanager), bem como as ferramentas de linha de comando a seguir:

-   **Dirquota.exe**. Use para criar e gerenciar cotas, cotas de aplicação automática e modelos de cotas.
-   **Filescrn.exe**. Use para criar e gerenciar triagens, modelos de triagem de arquivo, exceções de triagem de arquivo e grupos de arquivos.
-   **Storrept.exe**. Use para configurar os parâmetros de relatório e gerar relatórios de armazenamento sob demanda. Também use para criar tarefas de relatório que você pode programar usando **schtasks.exe**.

Você pode usar essas ferramentas para gerenciar recursos de armazenamento em um computador local ou em um computador remoto. Para obter mais informações sobre essas ferramentas de linha de comando, consulte as referências a seguir:

-   **Dirquota**: <https://go.microsoft.com/fwlink/?LinkId=92741>
-   **Filescrn**: <https://go.microsoft.com/fwlink/?LinkId=92742>
-   **Storrept**: <https://go.microsoft.com/fwlink/?LinkId=92743>


> [!Note]
> Para ver a sintaxe de comando e os parâmetros disponíveis para um comando, execute o comando com o parâmetro <strong>/?</strong> .


## <a name="remote-management-using-the-command-line-tools"></a>Gerenciamento remoto usando as ferramentas de linha de comando

Cada ferramenta tem várias opções para executar as ações semelhantes àquelas disponíveis no snap-in do MMC do Gerenciador de Recursos de Servidor de Arquivos. Para que um comando realize uma ação em um computador remoto em vez de um computador local, use o parâmetro **/remote**:*ComputerName*.

Por exemplo, **Dirquota.exe** inclui um parâmetro **exportar modelo** para gravar configurações de modelo de cota em um arquivo XML e um parâmetro **importar modelo** para importar as configurações de modelo do arquivo XML. Adicionar o parâmetro **/remote**:*ComputerName* ao comando **Importar modelo Dirquota.exe** importará os modelos do arquivo XML no computador local ao computador remoto.

> [!Note]
> Quando você executa as ferramentas de linha de comando com o parâmetro **/remote**:<em>ComputerName</em> para executar uma exportação de modelo (ou importar) em um computador remoto, os modelos são gravados (ou copiados de) um arquivo XML no computador local.

<br />

## <a name="additional-considerations"></a>Considerações adicionais 

Para gerenciar recursos remotos com as ferramentas de linha de comando:

-   Você deve estar conectado ao computador local com uma conta de domínio que é um membro do grupo **Administradores** no computador remoto.
-   Você deve executar as ferramentas de linha de comando de uma janela de Prompt de comando elevada. Para abrir a janela do Prompt de Comando com privilégios elevados, clique em **Iniciar**, aponte para **Todos os Programas**, clique em **Acessórios**, clique com o botão direito do mouse em **Prompt de Comando** e clique em **Executar como administrador**.
-   O computador remoto deve estar executando o Windows Server e Gerenciador de recursos do servidor de arquivos deve estar instalado.
-   A exceção **Gerenciamento do Gerenciador de Recursos de Servidor de Arquivos remoto** no computador remoto deve ser habilitada. Habilite essa exceção usando o Firewall do Windows no Painel de Controle.


## <a name="see-also"></a>Consulte também

-   [Gerenciar recursos de armazenamento remoto](managing-remote-storage-resources.md)