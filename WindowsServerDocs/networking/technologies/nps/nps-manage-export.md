---
title: Exportar uma configuração do NPS para importação em outro servidor
description: Você pode usar este tópico para saber como exportar uma configuração de servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d268dc57-78f8-47ba-9a7a-a607e8b9225c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a5f28c317f1d58fd1889fb55d345463dc8a62999
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816417"
---
# <a name="export-an-nps-configuration-for-import-on-another-server"></a>Exportar uma configuração do NPS para importação em outro servidor

Aplica-se a: Windows Server 2016

Você pode exportar toda a configuração do NPS — incluindo clientes RADIUS e servidores, diretiva de rede, diretiva de solicitação de conexão, registro e configuração de log — de um NPS para importação em outro NPS. 

Use uma das ferramentas a seguir para exportar a configuração do NPS:

- No Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012, você pode usar Netsh ou você pode usar o Windows PowerShell.
- No Windows Server 2008 R2 e Windows Server 2008, use Netsh.

>[!IMPORTANT]
>Não use este procedimento se o banco de dados NPS de origem tem um número de versão maior que o número de versão do banco de dados NPS de destino. Você pode exibir o número de versão do banco de dados na exibição do NPS a **netsh nps Mostrar config** comando.

Como configurações de NPS não são criptografadas no arquivo XML exportado, enviá-la em uma rede pode representar um risco de segurança, portanto, tome precauções ao mover o arquivo XML do servidor de origem para os servidores de destino. Por exemplo, adicione o arquivo para um arquivo de armazenamento protegido de senha criptografado, antes de mover o arquivo. Além disso, armazene o arquivo em um local seguro para impedir que usuários mal-intencionados acessem-lo.

>[!NOTE]
>Se o log do SQL Server está configurado na fonte de NPS, as configurações de log do SQL Server não são exportadas para o arquivo XML. Depois de importar o arquivo em outro NPS, configure manualmente o registro em log do SQL Server.

## <a name="export-and-import-the-nps-configuration-by-using-windows-powershell"></a>Exportar e importar a configuração do NPS usando o Windows PowerShell

Para Windows Server 2012 e versões posteriores do sistema operacional, você pode exportar a configuração do NPS usando o Windows PowerShell.

A sintaxe de comando para exportar a configuração do NPS é da seguinte maneira. 

    Export-NpsConfiguration -Path <filename>

A tabela a seguir lista os parâmetros para o **NpsConfiguration exportação** cmdlet do Windows PowerShell. Parâmetros em negrito são obrigatórios.

|Parâmetro|Descrição|
|---------|-----------|
|Caminho|Especifica o nome e local do arquivo XML ao qual você deseja exportar a configuração do NPS.|

**Credenciais administrativas**

Para concluir este procedimento, você deve ser um membro do grupo Administradores.

### <a name="export-example"></a>Exemplo de exportação 

No exemplo a seguir, a configuração do NPS é exportada para um arquivo XML localizado na unidade local. Para executar esse comando, execute o Windows PowerShell como administrador na fonte de NPS, digite o seguinte comando e pressione Enter.

`Export-NpsConfiguration –Path c:\config.xml` 

Para obter mais informações, consulte [NpsConfiguration exportação](https://technet.microsoft.com/library/jj872749.aspx).

Depois de exportar a configuração do NPS, copie o arquivo XML para o servidor de destino.

A sintaxe de comando para importar a configuração do NPS no servidor de destino é da seguinte maneira.

    Import-NpsConfiguration [-Path] <String> [ <CommonParameters>]

### <a name="import-example"></a>Exemplo de importação

O comando a seguir importa as configurações do arquivo denominado C:\Npsconfig.xml ao NPS. Para executar esse comando, execute o Windows PowerShell como administrador no NPS de destino, digite o seguinte comando e pressione Enter.

    PS C:\> Import-NpsConfiguration -Path "C:\Npsconfig.xml"

Para obter mais informações, consulte [importação NpsConfiguration](https://technet.microsoft.com/library/jj872750.aspx).

## <a name="export-and-import-the-nps-configuration-by-using-netsh"></a>Exportar e importar a configuração do NPS usando Netsh

Você pode usar o Shell de rede \(Netsh\) para exportar a configuração NPS usando o **exportação netsh nps** comando.

Quando o **importação de netsh nps** comando é executado, o NPS é atualizado automaticamente com as configurações atualizadas. Não é necessário parar o NPS no computador de destino para executar o **importação de netsh nps** de comando, no entanto se o console do NPS ou o snap-in MMC do NPS está aberto durante a importação de configuração, as alterações na configuração de servidor não são visíveis até Atualize a exibição. 

>[!NOTE]
>Quando você usa o **exportação netsh nps** de comando, você deve fornecer o parâmetro de comando **exportPSK** com o valor **Sim**. Esse parâmetro e valor claramente que você entenda que você está exportando a configuração do NPS, e que o arquivo XML exportado contém sem criptografia de segredos compartilhados para clientes RADIUS e membros de grupos de servidores RADIUS remotos.

**Credenciais administrativas**

Para concluir este procedimento, você deve ser um membro do grupo Administradores.

### <a name="to-copy-an-nps-configuration-to-another-nps-using-netsh-commands"></a>Para copiar uma configuração do NPS para outro NPS usando comandos Netsh

1. Na fonte de NPS, abra **Prompt de comando**, digite **netsh**, e pressione Enter.

2. No **netsh** , digite **nps**, e pressione Enter. 

3. No **netsh nps** , digite **exportar filename =**"*path\file.xml*" **exportPSK = YES**, onde *caminho* é o local da pasta onde você deseja salvar o arquivo de configuração do NPS, e *arquivo* é o nome do arquivo XML que você deseja salvar. Pressione Enter. 

Isso armazena as definições de configuração \(incluindo as configurações do registro\) em um arquivo XML. O caminho pode ser relativo ou absoluto, ou pode ser uma convenção de nomenclatura Universal \(UNC\) caminho. Depois de pressionar Enter, será exibida uma mensagem que indica se a exportação para o arquivo foi bem-sucedida.

4. Copie o arquivo que você criou para o NPS de destino.

5. Em um prompt de comando no NPS de destino, digite **netsh nps importar filename =**"*path\file.xml*", e pressione Enter. Será exibida uma mensagem que indica se a importação do arquivo XML foi bem-sucedida.

Para obter mais informações sobre netsh, consulte [Network Shell (Netsh)](../netsh/netsh.md).

