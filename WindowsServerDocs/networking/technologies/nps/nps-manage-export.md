---
title: Exportar uma configuração NPS para importação em outro servidor
description: Você pode usar este tópico para aprender a exportar uma configuração de servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d268dc57-78f8-47ba-9a7a-a607e8b9225c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cbebd0388ccd5dd2540a20f5d325d7f97c7e2bb3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405434"
---
# <a name="export-an-nps-configuration-for-import-on-another-server"></a>Exportar uma configuração NPS para importação em outro servidor

Aplica-se a: Windows Server 2016

Você pode exportar toda a configuração do NPS — incluindo clientes e servidores RADIUS, diretiva de rede, política de solicitação de conexão, registro e configuração de log — de um NPS para importação em outro NPS. 

Use uma das seguintes ferramentas para exportar a configuração do NPS:

- No Windows Server 2016, no Windows Server 2012 R2 e no Windows Server 2012, você pode usar o netsh ou pode usar o Windows PowerShell.
- No Windows Server 2008 R2 e no Windows Server 2008, use netsh.

> [!IMPORTANT]
> Não use este procedimento se o banco de dados NPS de origem tiver um número de versão superior ao número de versão do banco de dados NPS de destino. Você pode exibir o número de versão do banco de dados NPS na exibição do comando **netsh nps show config** .

Como as configurações de NPS não são criptografadas no arquivo XML exportado, enviá-las por uma rede pode representar um risco de segurança, portanto, tome cuidado ao mover o arquivo XML do servidor de origem para os servidores de destino. Por exemplo, adicione o arquivo a um arquivo criptografado protegido por senha antes de mover o arquivo. Além disso, armazene o arquivo em um local seguro para impedir que usuários mal-intencionados o acessem.

> [!NOTE]
> Se SQL Server log estiver configurado no NPS de origem, as configurações de log de SQL Server não serão exportadas para o arquivo XML. Depois de importar o arquivo em outro NPS, você deve configurar manualmente SQL Server log.

## <a name="export-and-import-the-nps-configuration-by-using-windows-powershell"></a>Exportar e importar a configuração do NPS usando o Windows PowerShell

Para o Windows Server 2012 e versões posteriores do sistema operacional, você pode exportar a configuração do NPS usando o Windows PowerShell.

A sintaxe de comando para exportar a configuração do NPS é a seguinte. 

    Export-NpsConfiguration -Path <filename>

A tabela a seguir lista os parâmetros para o cmdlet **Export-NpsConfiguration** no Windows PowerShell. Os parâmetros em negrito são obrigatórios.

|Parâmetro|Descrição|
|---------|-----------|
|Path|Especifica o nome e o local do arquivo XML para o qual você deseja exportar a configuração do NPS.|

**Credenciais administrativas**

Para concluir este procedimento, você deve ser um membro do grupo Administradores.

### <a name="export-example"></a>Exemplo de exportação 

No exemplo a seguir, a configuração do NPS é exportada para um arquivo XML localizado na unidade local. Para executar esse comando, execute o Windows PowerShell como administrador no NPS de origem, digite o comando a seguir e pressione Enter.

`Export-NpsConfiguration –Path c:\config.xml` 

Para obter mais informações, consulte [Export-NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx).

Depois de exportar a configuração do NPS, copie o arquivo XML para o servidor de destino.

A sintaxe de comando para importar a configuração do NPS no servidor de destino é a seguinte.

    Import-NpsConfiguration [-Path] <String> [ <CommonParameters>]

### <a name="import-example"></a>Exemplo de importação

O comando a seguir importa as configurações do arquivo chamado C:\Npsconfig.xml para o NPS. Para executar esse comando, execute o Windows PowerShell como administrador no NPS de destino, digite o comando a seguir e pressione Enter.

    PS C:\> Import-NpsConfiguration -Path "C:\Npsconfig.xml"

Para obter mais informações, consulte [Import-NpsConfiguration](https://technet.microsoft.com/library/jj872750.aspx).

## <a name="export-and-import-the-nps-configuration-by-using-netsh"></a>Exportar e importar a configuração do NPS usando Netsh

Você pode usar o Shell de rede \(Netsh @ no__t-1 para exportar a configuração do NPS usando o comando **netsh nps Export** .

Quando o comando **netsh nps Import** é executado, o NPS é atualizado automaticamente com as definições de configuração atualizadas. Você não precisa interromper o NPS no computador de destino para executar o comando **netsh nps Import** , no entanto, se o console do NPS ou o snap-in do MMC do NPS estiver aberto durante a importação da configuração, as alterações na configuração do servidor não estarão visíveis até que você atualize a exibição. 

> [!NOTE]
> Quando você usa o comando **netsh nps Export** , é necessário fornecer o parâmetro de comando **exportPSK** com o valor **Sim**. Esse parâmetro e valor explicitamente desejam que você entenda que está exportando a configuração NPS e que o arquivo XML exportado contém segredos compartilhados não criptografados para clientes RADIUS e membros de grupos de servidores remotos RADIUS.

**Credenciais administrativas**

Para concluir este procedimento, você deve ser um membro do grupo Administradores.

### <a name="to-copy-an-nps-configuration-to-another-nps-using-netsh-commands"></a>Para copiar uma configuração NPS para outro NPS usando comandos netsh

1. No NPS de origem, abra o **prompt de comando**, digite **netsh**e pressione Enter.

2. No prompt do **netsh** , digite **NPS**e pressione Enter. 

3. No prompt **netsh nps** , digite **Export filename =** "*path\file.xml*" **exportPSK = Yes**, em que *Path* é o local da pasta onde você deseja salvar o arquivo de configuração do NPS e *File* é o nome do arquivo XML que você deseja salvar. Pressione Enter. 

Isso armazena definições de configuração \(including configurações do registro @ no__t-1 em um arquivo XML. O caminho pode ser relativo ou absoluto, ou pode ser uma Convenção de nomenclatura universal \(UNC @ no__t-1 caminho. Depois de pressionar Enter, será exibida uma mensagem indicando se a exportação para o arquivo foi bem-sucedida.

4. Copie o arquivo que você criou para o NPS de destino.

5. Em um prompt de comando no NPS de destino, digite **netsh nps Import filename =** "*path\file.xml*" e pressione Enter. Uma mensagem é exibida indicando se a importação do arquivo XML foi bem-sucedida.

Para obter mais informações sobre o netsh, consulte o [Shell de rede (netsh)](../netsh/netsh.md).

