---
title: Exportar uma configuração do servidor NPS para importar em outro servidor
description: Você pode usar este tópico para saber como exportar uma configuração do servidor de política de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d268dc57-78f8-47ba-9a7a-a607e8b9225c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 141a99e930672d8403315cb6804290d184ef3007
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="export-an-nps-server-configuration-for-import-on-another-server"></a>Exportar uma configuração do servidor NPS para importar em outro servidor

Aplica-se a: Windows Server 2016

Você pode exportar a configuração do NPS inteira — incluindo clientes RADIUS e servidores, política de rede, política de solicitação de conexão, registro e configuração de registro em log — de um servidor NPS para importar em outro servidor NPS. 

Use uma das seguintes ferramentas para exportar a configuração do NPS:

- No Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016, você pode usar Netsh ou você pode usar o Windows PowerShell.
- No Windows Server 2008 R2 e Windows Server 2008, use Netsh.

>[!IMPORTANT]
>Não use este procedimento se o banco de dados NPS de origem tem um número de versão maior que o número da versão do banco de dados NPS de destino. Você pode exibir o número da versão do banco de dados NPS da exibição do **netsh nps Mostrar config** comando.

Como configurações do servidor NPS não são criptografadas no arquivo XML exportado, enviando por uma rede pode representar um risco de segurança, então leve precauções ao mover o arquivo XML do servidor de origem para os servidores de destino. Por exemplo, adicione o arquivo para um arquivo criptografado, de arquivo-morto protegido de senha antes de passar o arquivo. Além disso, armazene o arquivo em um local seguro para impedir que usuários mal-intencionados acessem-lo.

>[!NOTE]
>Se o log do SQL Server é configurado no servidor NPS de origem, configurações de log do SQL Server não serão exportadas para o arquivo XML. Depois de importar o arquivo em outro servidor NPS, configure manualmente o log do SQL Server.

## <a name="export-and-import-the-nps-configuration-by-using-windows-powershell"></a>Exportar e importar a configuração do NPS usando o Windows PowerShell

Para Windows Server 2012 e versões posteriores do sistema operacional, você pode exportar a configuração do NPS usando o Windows PowerShell.

A sintaxe de comando para exportar a configuração do NPS é o seguinte. 

    Export-NpsConfiguration -Path <filename>

A tabela a seguir lista os parâmetros para o **exportar NpsConfiguration** cmdlet do Windows PowerShell. Parâmetros em negrito são necessários.

|Parâmetro|Descrição|
|---------|-----------|
|Caminho|Especifica o nome e local do arquivo XML para o qual você deseja exportar a configuração do servidor NPS.|

**Credenciais administrativas**

Para concluir este procedimento, você deve ser um membro do grupo Administradores.

### <a name="export-example"></a>Exemplo de exportação 

No exemplo a seguir, a configuração do NPS é exportada para um arquivo XML localizado na unidade local. Para executar esse comando, execute o Windows PowerShell como administrador no servidor NPS de origem, digite o seguinte comando e pressione Enter.

`Export-NpsConfiguration –Path c:\config.xml` 

Para obter mais informações, consulte [exportar NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx).

Depois que você tiver exportado a configuração do NPS, copie o arquivo XML para o servidor de destino.

A sintaxe de comando para importação de configuração do NPS no servidor de destino é o seguinte.

    Import-NpsConfiguration [-Path] <String> [ <CommonParameters>]

### <a name="import-example"></a>Exemplo de importação

O comando a seguir importa configurações do arquivo nomeado C:\Npsconfig.xml para NPS. Para executar esse comando, execute o Windows PowerShell como administrador no servidor NPS de destino, digite o seguinte comando e pressione Enter.

    PS C:\> Import-NpsConfiguration -Path "C:\Npsconfig.xml"

Para obter mais informações, consulte [importar NpsConfiguration](https://technet.microsoft.com/library/jj872750.aspx).

## <a name="export-and-import-the-nps-configuration-by-using-netsh"></a>Exportar e importar a configuração do NPS usando Netsh

Você pode usar o Shell de rede \(Netsh\) para exportar a configuração do servidor NPS usando o **netsh nps exportar** comando.

Quando o **netsh nps importar** comando é executado, o NPS sejam automaticamente atualizado com as configurações de configuração atualizada. Você não precisa parar NPS no computador de destino para executar o **netsh nps importar** de comando, porém se o console NPS ou o snap-in NPS MMC estiver aberta durante a importação de configuração, as alterações na configuração do servidor não estão visíveis até que você atualize o modo de exibição. 

>[!NOTE]
>Quando você usa o **netsh nps exportar** de comando, você precisa fornecer o parâmetro de comando **exportPSK** com o valor **Sim**. Este parâmetro e valor claramente que você entende que a exportação de configuração do servidor NPS, e que contém o arquivo XML exportado descriptografadas segredos compartilhados para clientes RADIUS e membros de grupos de servidores remotos RADIUS.

**Credenciais administrativas**

Para concluir este procedimento, você deve ser um membro do grupo Administradores.

### <a name="to-copy-an-nps-server-configuration-to-another-nps-server-using-netsh-commands"></a>Para copiar uma configuração do servidor NPS para outro servidor NPS usando comandos Netsh

1. No servidor NPS de origem, abra **Prompt de comando**, tipo **netsh**, e pressione Enter.

2. No **netsh** , digite **nps**, e pressione Enter. 

3. No **netsh nps** , digite **exportar filename =**"*path\file.xml*" **exportPSK = YES**, onde *caminho* é o local da pasta onde deseja salvar o servidor NPS arquivo de configuração, e *arquivo* é o nome do arquivo XML que você deseja salvar. Pressione Enter. 

Ele armazena configurações \(including registry settings\) em um arquivo XML. O caminho pode ser relativo ou absoluto ou pode ser um caminho de Universal Naming Convention \(UNC\). Depois que você pressiona Enter, será exibida uma mensagem indica se a exportação ao arquivo foi bem-sucedida.

4. Copie o arquivo que você criou para o servidor NPS de destino.

5. Em um prompt de comando no servidor NPS de destino, digite **netsh nps importar filename =**"*path\file.xml*", e pressione Enter. Será exibida uma mensagem indica se a importação do arquivo XML foi bem-sucedida.

Para obter mais informações sobre netsh, consulte [Shell de rede (Netsh)](../netsh/netsh.md).

