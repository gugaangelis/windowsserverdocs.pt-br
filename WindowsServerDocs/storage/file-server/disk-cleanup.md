---
title: Usando a limpeza de disco no Windows Server
description: Saiba como usar opções de linha de comando para configurar a ferramenta de limpeza de disco (cleanmgr. exe) para limpar automaticamente determinados arquivos.
ms.prod: windows-server-threshold
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: 4bf32520dc6fa2be36d44fbd66a7efc885a8f5d7
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867404"
---
# <a name="using-disk-cleanup-on-windows-server"></a>Usando a limpeza de disco no Windows Server

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

A ferramenta limpeza de disco limpa os arquivos desnecessários em um ambiente do Windows Server. Essa ferramenta está disponível por padrão no Windows Server 2019 e no Windows Server 2016, mas talvez seja necessário executar algumas etapas manuais para habilitá-lo em versões anteriores do Windows Server.

Para iniciar a ferramenta de limpeza de disco, execute o comando cleanmgr. exe ou selecione **Iniciar**, selecione **Ferramentas administrativas do Windows**e, em seguida, selecione **limpeza de disco**.

Você também pode executar a limpeza de disco usando o [comando cleanmgr do Windows](../../administration/windows-commands/cleanmgr.md) e usar opções de linha de comando para especificar que a limpeza de disco limpará determinados arquivos.

## <a name="enable-disk-cleanup-on-an-earlier-version-of-windows-server-by-installing-the-desktop-experience"></a>Habilitar a limpeza de disco em uma versão anterior do Windows Server instalando a experiência desktop

Siga estas etapas para usar o assistente para adicionar funções e recursos para instalar a experiência desktop em um servidor que executa o Windows Server 2012 R2 ou anterior, que também instala a limpeza de disco.

1. Se o Gerenciador do Servidor já estiver aberto, vá para a etapa seguinte. Se o Gerenciador do Servidor ainda não estiver aberto, abra-o de uma das maneiras a seguir.

   - Na área de trabalho do Windows, inicie o Gerenciador do Servidor clicando em **Gerenciador do Servidor** na barra de tarefas do Windows.

   - Vá para **Iniciar** e selecione o bloco Gerenciador do servidor.

1. No menu **gerenciar** , selecione Adicionar **funções e recursos**.

1. Na página **antes de começar** , verifique se o servidor de destino e o ambiente de rede estão preparados para o recurso que você deseja instalar. Selecione **Avançar**.

1. Na página **Selecionar tipo de instalação** , selecione **instalação baseada em função ou recurso** para instalar todos os recursos de peças em um único servidor. Selecione **Avançar**.

1. Na página **Selecionar servidor de destino**, escolha um servidor no pool de servidores ou um VHD offline. Selecione **Avançar**.

1. Na página **selecionar funções de servidor** , selecione **Avançar**.

1. Na página **selecionar recursos** , selecione **interface do usuário e infraestrutura**e, em seguida, selecione **experiência desktop**.

1. Em **Adicionar recursos que são necessários para a experiência desktop?** , selecione **Adicionar recursos**.

1. Prossiga com a instalação e reinicialize o sistema.

1. Verifique se o botão opção de **limpeza de disco** aparece na caixa de diálogo Propriedades.

   ![Caixa de diálogo Propriedades do disco com a opção limpeza de disco](media/diskpropswcleanup.png)

## <a name="manually-add-disk-cleanup-to-an-earlier-version-of-windows-server"></a>Adicionar manualmente a limpeza de disco a uma versão anterior do Windows Server

A ferramenta de limpeza de disco (cleanmgr. exe) não está presente no Windows Server 2012 R2 ou anterior, a menos que você tenha o recurso experiência desktop instalado.

Para usar o cleanmgr. exe, instale a experiência desktop conforme descrito anteriormente ou copie dois arquivos que já estão presentes no servidor, cleanmgr. exe e cleanmgr. exe. mui. Use a tabela a seguir para localizar os arquivos do seu sistema operacional.

| Sistema operacional  | Arquitetura  | Localização do arquivo  |
| ----------------- | -------------- | --------------- |
| Windows Server 2008 R2 | 64 bits | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr_31bf3856ad364e35_6.1.7600.16385_none_c9392808773cd7da\cleanmgr.exe 
| Windows Server 2008 R2 | 64 bits | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr.resources_31bf3856ad364e35_6.1.7600.16385_en-us_b9cb6194b257cc63\cleanmgr.exe.mui |

Localize cleanmgr. exe e mova o arquivo para **%systemroot%\system32**.

Localize cleanmgr. exe. mui e mova os arquivos para **%systemroot%\System32\en-US**.

Agora você pode iniciar a ferramenta de limpeza de disco executando o cleanmgr. exe no prompt de comando ou clicando em **Iniciar** e digitando **cleanmgr** na barra de pesquisa.

Para que o botão limpeza de disco apareça na caixa de diálogo de propriedades de um disco, você também precisará instalar o recurso experiência desktop.

## <a name="additional-references"></a>Referências adicionais

[Liberar espaço em disco no Windows 10](https://support.microsoft.com/en-us/help/12425/windows-10-free-up-drive-space)

[cleanmgr](../../administration/windows-commands/cleanmgr.md)
