---
title: Usando a limpeza de disco no Windows Server
description: Saiba como usar as opções de linha de comando para configurar a ferramenta de limpeza de disco (Cleanmgr.exe) para limpar automaticamente determinados arquivos.
ms.prod: windows-server-threshold
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: fbec7cd2b8312f03998cfb27b739d0866d3a47c5
ms.sourcegitcommit: 545dcfc23a81943e129565d0ad188263092d85f6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67407662"
---
# <a name="using-disk-cleanup-on-windows-server"></a>Usando a limpeza de disco no Windows Server

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

A ferramenta de limpeza de disco limpa arquivos desnecessários em um ambiente do Windows Server. Essa ferramenta está disponível por padrão no Windows Server 2019 e Windows Server 2016, mas talvez você precise executar algumas etapas manuais para habilitá-lo em versões anteriores do Windows Server.

Para iniciar a ferramenta de limpeza de disco, execute o comando Cleanmgr.exe ou selecione **inicie**, selecione **ferramentas administrativas do Windows**e, em seguida, selecione **limpeza de disco**.

Você também pode executar a limpeza de disco usando o [cleanmgr comando Windows](../../administration/windows-commands/cleanmgr.md) e use opções de linha de comando para especificar que a limpeza de disco limpa determinados arquivos.

## <a name="enable-disk-cleanup-on-an-earlier-version-of-windows-server-by-installing-the-desktop-experience"></a>Habilitar a limpeza de disco em uma versão anterior do Windows Server, instalando a experiência de área de trabalho

Siga estas etapas para usar o assistente Adicionar funções e recursos para instalar a experiência de área de trabalho em um servidor executando o Windows Server 2012 R2 ou anterior, que também instala a limpeza de disco.

1. Se o Gerenciador do Servidor já estiver aberto, vá para a etapa seguinte. Se o Gerenciador do Servidor ainda não estiver aberto, abra-o de uma das maneiras a seguir.

   - Na área de trabalho do Windows, inicie o Gerenciador do Servidor clicando em **Gerenciador do Servidor** na barra de tarefas do Windows.

   - Vá para **iniciar** e selecione o bloco do Gerenciador do servidor.

1. Sobre o **Manage** menu, selecione Adicionar **funções e recursos**.

1. Sobre o **antes de começar** , verifique se o seu servidor de destino e o ambiente de rede estão preparados para o recurso que você deseja instalar. Selecione **Avançar**.

1. Sobre o **Selecionar tipo de instalação** , selecione **instalação baseada em função ou recurso** para instalar todos os recursos de partes em um único servidor. Selecione **Avançar**.

1. Na página **Selecionar servidor de destino**, escolha um servidor no pool de servidores ou um VHD offline. Selecione **Avançar**.

1. Sobre o **selecionar funções de servidor** , selecione **próxima**.

1. Sobre o **selecionar recursos** , selecione **Interface do usuário e a infraestrutura**e, em seguida, selecione **Experiência Desktop**.

1. Na **adicionar recursos que são necessários para a experiência Desktop?** , selecione **adicionar recursos**.

1. Prossiga com a instalação e, em seguida, reinicialize o sistema.

1. Verifique se que o **limpeza de disco** botão de opção é exibida na caixa de diálogo de propriedades.

   ![Caixa de diálogo de propriedades com a opção de limpeza de disco do disco](media/diskpropswcleanup.png)

## <a name="manually-add-disk-cleanup-to-an-earlier-version-of-windows-server"></a>Adicionar manualmente a limpeza de disco para uma versão anterior do Windows Server

A ferramenta de limpeza de disco (cleanmgr.exe) não estiver presente no Windows Server 2012 R2 ou anterior, a menos que você tenha o recurso Experiência Desktop instalado.

Para usar cleanmgr.exe, instalar a experiência de área de trabalho, conforme descrito anteriormente, ou copiar dois arquivos que já estão presentes no servidor, cleanmgr.exe e cleanmgr.exe.mui. Use a tabela a seguir para localizar os arquivos para seu sistema operacional.

| Sistema operacional  | Arquitetura  | Localização do arquivo  |
| ----------------- | -------------- | --------------- |
| Windows Server 2008 R2 | 64 bits | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr_31bf3856ad364e35_6.1.7600.16385_none_c9392808773cd7da\cleanmgr.exe 
| Windows Server 2008 R2 | 64 bits | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr.resources_31bf3856ad364e35_6.1.7600.16385_en-us_b9cb6194b257cc63\cleanmgr.exe.mui |

Localize cleanmgr.exe e mova o arquivo para **%systemroot%\System32**.

Localize cleanmgr.exe.mui e mova os arquivos a serem **%systemroot%\System32\en-US**.

Agora você pode iniciar a ferramenta de limpeza de disco executando Cleanmgr.exe de Prompt de comando ou clicando **inicie** e digitando **Cleanmgr** na barra de pesquisa.

Para fazer com que o botão de limpeza de disco são exibidos na caixa de diálogo de propriedades de um disco, você também precisará instalar o recurso Experiência Desktop.

## <a name="additional-references"></a>Referências adicionais

[Libere espaço em disco no Windows 10](https://support.microsoft.com/en-us/help/12425/windows-10-free-up-drive-space)

[cleanmgr](../../administration/windows-commands/cleanmgr.md)
