---
title: Analisar scwcmd
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0259271b-be5b-48d7-a51d-8b9b6786efb4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cce3428b281ede582ed781afbdee9dea495b52ae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873907"
---
# <a name="scwcmd-analyze"></a>Scwcmd: analyze

> Aplica-se a: Windows Server 2012 R2, Windows Server 2012

Determina se um computador está em conformidade com uma política. Os resultados são retornados em um arquivo. XML. Também aceita uma lista de nomes de computador como entrada. Para exibir os resultados em seu navegador, use **exibição scwcmd** e especifique **%windir%\security\msscw\TransformFiles\scwanalysis.xsl** como a transformação. xsl. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
scwcmd analyze [[[/m:<ComputerName> | /ou:<Ou>] /p:<Policy>] | /i:<ComputerList>] [/o:
<ResultDir>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>] [/l] [/e]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/m:\<ComputerName>|Especifica o nome NetBIOS, nome DNS ou endereço IP do computador que será analisado. Se o **/m** parâmetro for especificado, o **p** parâmetro também deve ser especificado.|
|/ou:\<OuName>|Especifica o nome de domínio totalmente qualificado (FQDN) de uma unidade organizacional (UO) no Active Directory Domain Services. Se o **/ou** parâmetro for especificado, o **p** parâmetro também deve ser especificado. Todos os computadores na UO serão analisados em relação a política determinada.|
|/p:\<política >|Especifica o nome de arquivo e caminho do arquivo de diretiva. XML a ser usado para executar a análise.|
|/i:\<ComputerList>|Especifica o nome de arquivo e caminho de um arquivo. XML que contém uma lista de computadores juntamente com seus arquivos de política esperado. Todos os computadores no arquivo. XML serão analisados em relação a seus arquivos de política correspondentes. Um exemplo de arquivo. XML é % windir%\security\SampleMachineList.xml.|
|/o:\<ResultDir>|Especifica o caminho e o diretório onde os arquivos de resultados de análise devem ser salvo. O padrão é o diretório atual.|
|/u:\<UserName>|Especifica uma credencial de usuário alternativas para usar ao executar a análise em um computador remoto. O padrão é o conectado no usuário.|
|/pw:\<senha >|Especifica uma credencial de usuário alternativas para usar ao executar a análise em um computador remoto. O padrão é a senha do usuário conectado.|
|/t:\<Threads>|Especifica o número de operações simultâneas de análise pendentes que deveria ser mantida durante a análise (DefaultValue = 40, MinValue = 1, MaxValue = 1000).|
|/l|Faz com que o processo de análise a serem registrados. Um arquivo de log será gerado para cada computador que está sendo analisado. Os arquivos de log serão armazenados no mesmo diretório que os arquivos de resultado. Use o **/o** opção para especificar o diretório para os arquivos de resultado.|
|/e|Registrar um evento no log de eventos do aplicativo, se uma incompatibilidade for encontrada.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Scwcmd.exe só está disponível em computadores que executam o Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="BKMK_Examples"></a>Exemplos

Para analisar uma diretiva de segurança contra webpolicy.xml o arquivo, digite:
```
scwcmd analyze /p:webpolicy.xml

```
Para analisar uma diretiva de segurança no computador chamado webserver contra webpolicy.xml o arquivo usando as credenciais da conta webadmin, digite:
```
scwcmd analyze /m:webserver /p:webpolicy.xml /u:webadmin

```
Para analisar uma diretiva de segurança contra o arquivo webpolicy.xml, com um máximo de 100 threads e enviar os resultados para um arquivo chamado resultados no compartilhamento de resultserver, digite:
```
scwcmd analyze /i:webpolicy.xml /t:100 /o:\\resultserver\results

```
Para analisar uma diretiva de segurança para a OU WebServers contra webpolicy.xml o arquivo usando as credenciais de DomainAdmin, digite:
```
scwcmd analyze /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)