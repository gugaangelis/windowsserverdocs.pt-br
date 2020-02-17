---
title: Como usar a Limpeza de Disco no Windows Server
description: Saiba como usar opções de linha de comando para configurar a ferramenta de Limpeza de Disco (cleanmgr.exe) para limpar automaticamente determinados arquivos.
ms.prod: windows-server
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: bb93ec15fd138ee65797c9d27413552c3a1759a6
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949678"
---
# <a name="using-disk-cleanup-on-windows-server"></a>Como usar a Limpeza de Disco no Windows Server

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

A ferramenta Limpeza de Disco limpa os arquivos desnecessários em um ambiente do Windows Server. Essa ferramenta está disponível por padrão no Windows Server 2019 e no Windows Server 2016, mas talvez seja necessário executar algumas etapas manuais para habilitá-la em versões anteriores do Windows Server.

Para iniciar a ferramenta de limpeza de disco, execute o comando cleanmgr.exe ou selecione **Iniciar**, selecione **Ferramentas Administrativas do Windows** e, em seguida, selecione **Limpeza de Disco**.

Você também pode executar a Limpeza de Disco usando o [comando cleanmgr do Windows](../../administration/windows-commands/cleanmgr.md) e as opções de linha de comando para especificar que a Limpeza de Disco limpa determinados arquivos.

## <a name="enable-disk-cleanup-on-an-earlier-version-of-windows-server-by-installing-the-desktop-experience"></a>Habilitar a Limpeza de Disco em uma versão anterior do Windows Server instalando a Experiência Desktop

Siga estas etapas para usar o assistente para Adicionar Funções e Recursos para instalar a Experiência Desktop em um servidor que executa o Windows Server 2012 R2 ou anterior, que também instala a Limpeza de Disco.

1. Se o Gerenciador do Servidor já estiver aberto, vá para a etapa seguinte. Se o Gerenciador do Servidor ainda não estiver aberto, abra-o de uma das maneiras a seguir.

   - Na área de trabalho do Windows, inicie o Gerenciador do Servidor clicando em **Gerenciador do Servidor** na barra de tarefas do Windows.

   - Acesse **Iniciar** e selecione o bloco Gerenciador do Servidor.

1. No menu **Gerenciar**, selecione **Adicionar Funções e Recursos**.

1. Na página **Antes de começar**, verifique se o servidor de destino e o ambiente de rede estão preparados para o recurso que deseja instalar. Selecione **Avançar**.

1. Na página **Selecionar tipo de instalação**, selecione **Instalação baseada em função ou recurso** para instalar todos os recursos da peça em um servidor. Selecione **Avançar**.

1. Na página **Selecionar servidor de destino** , escolha um servidor no pool de servidores ou um VHD offline. Selecione **Avançar**.

1. Na página **Selecionar funções do servidor**, selecione **Avançar**.

1. Na página **Selecionar recursos**, selecione **Interface do Usuário e Infraestrutura** e, em seguida, selecione **Experiência Desktop**.

1. Em **Adicionar recursos que são necessários para a Experiência Desktop?** , selecione **Adicionar Recursos**.

1. Prossiga com a instalação e então reinicialize o sistema.

1. Verifique se o botão de opção **Limpeza de Disco** aparece na caixa de diálogo Propriedades.

   ![Caixa de diálogo Propriedades do Disco com a opção Limpeza de Disco](media/diskpropswcleanup.png)

## <a name="manually-add-disk-cleanup-to-an-earlier-version-of-windows-server"></a>Adicionar manualmente a Limpeza de Disco a uma versão anterior do Windows Server

A ferramenta de Limpeza de Disco (cleanmgr.exe) não estará presente no Windows Server 2012 R2 ou anterior, a menos que você tenha o recurso Experiência Desktop instalado.

Para usar o cleanmgr.exe, instale a Experiência Desktop conforme descrito anteriormente ou copie dois arquivos que já estão presentes no servidor, cleanmgr.exe e cleanmgr.exe.mui. Use a tabela a seguir para localizar os arquivos do sistema operacional.

| Sistema operacional  | Arquitetura  | Localização do arquivo  |
| ----------------- | -------------- | --------------- |
| Windows Server 2008 R2 | 64 bits | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr_31bf3856ad364e35_6.1.7600.16385_none_c9392808773cd7da\cleanmgr.exe 
| Windows Server 2008 R2 | 64 bits | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr.resources_31bf3856ad364e35_6.1.7600.16385_en-us_b9cb6194b257cc63\cleanmgr.exe.mui |

Localize cleanmgr.exe e mova o arquivo para **%systemroot%\System32**.

Localize cleanmgr.exe.mui e mova os arquivos para **%systemroot%\System32\en-US**.

Agora você pode iniciar a ferramenta de Limpeza de Disco executando o cleanmgr.exe do prompt de comando ou clicando em **Iniciar** e digitando **cleanmgr** na barra de pesquisa.

Para que o botão Limpeza de Disco apareça na caixa de diálogo de Propriedades de um disco, você também precisará instalar o recurso Experiência Desktop.

## <a name="additional-references"></a>Referências adicionais

[Liberar espaço em disco no Windows 10](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space)

[cleanmgr](../../administration/windows-commands/cleanmgr.md)
