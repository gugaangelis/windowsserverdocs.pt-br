---
title: Scwcmd analyze
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0259271b-be5b-48d7-a51d-8b9b6786efb4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 017363ff2a60f9348290813c357560fe9fe3ba2a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722156"
---
# <a name="scwcmd-analyze"></a>Scwcmd: analyze

> Aplica-se a: Windows Server 2012 R2, Windows Server 2012

Determina se um computador está em conformidade com uma política. Os resultados são retornados em um arquivo. xml. Também aceita uma lista de nomes de computador como entrada. Para exibir os resultados em seu navegador, use a **exibição scwcmd** e especifique **%windir%\security\msscw\TransformFiles\scwanalysis.xsl** como a transformação. Xsl.

## <a name="syntax"></a>Sintaxe

```
scwcmd analyze [[[/m:<ComputerName> | /ou:<Ou>] /p:<Policy>] | /i:<ComputerList>] [/o:
<ResultDir>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>] [/l] [/e]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/m:\<ComputerName>|Especifica o nome NetBIOS, o nome DNS ou o endereço IP do computador a ser analisado. Se o parâmetro **/m** for especificado, o parâmetro **/p** também deverá ser especificado.|
|/ou:\<OuName>|Especifica o FQDN (nome de domínio totalmente qualificado) de uma UO (unidade organizacional) em Active Directory Domain Services. Se o parâmetro **/ou** for especificado, o parâmetro **/p** também deverá ser especificado. Todos os computadores na UO serão analisados em relação à política fornecida.|
|/p:\<política>|Especifica o caminho e o nome do arquivo de política. XML a ser usado para executar a análise.|
|/i:\<computerlist>|Especifica o caminho e o nome de arquivo de um arquivo. XML que contém uma lista de computadores junto com seus arquivos de política esperados. Todos os computadores no arquivo. XML serão analisados em relação aos seus arquivos de política correspondentes. Um arquivo. XML de exemplo é%windir%\security\SampleMachineList.xml.|
|/o:\<ResultDir>|Especifica o caminho e o diretório em que os arquivos de resultado da análise devem ser salvos. O padrão é o diretório atual.|
|/u:\<username>|Especifica uma credencial de usuário alternativa a ser usada ao executar a análise em um computador remoto. O padrão é o usuário conectado.|
|/PW:\<senha>|Especifica uma credencial de usuário alternativa a ser usada ao executar a análise em um computador remoto. O padrão é a senha do usuário conectado.|
|/t:\<threads>|Especifica o número de operações de análise pendentes simultâneas que devem ser mantidas durante a análise (DefaultValue = 40, MinValue = 1, MaxValue = 1000).|
|/l|Faz com que o processo de análise seja registrado. Um arquivo de log será gerado para cada computador que está sendo analisado. Os arquivos de log serão armazenados no mesmo diretório que os arquivos de resultado. Use a opção **/o** para especificar o diretório dos arquivos de resultado.|
|/e|Registre um evento no log de eventos do aplicativo se uma incompatibilidade for encontrada.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Scwcmd. exe só está disponível em computadores que executam o Windows Server 2008 R2, o Windows Server 2008 ou o Windows Server 2003.

## <a name="examples"></a>Exemplos

Para analisar uma política de segurança em relação ao arquivo webpolicy. xml, digite:
```
scwcmd analyze /p:webpolicy.xml

```
Para analisar uma política de segurança no computador chamado WebServer no arquivo webpolicy. xml usando as credenciais da conta WebAdmin, digite:
```
scwcmd analyze /m:webserver /p:webpolicy.xml /u:webadmin

```
Para analisar uma política de segurança em relação ao arquivo webpolicy. xml, com um máximo de 100 threads e gerar os resultados em um arquivo chamado Results no compartilhamento resultserver, digite:
```
scwcmd analyze /i:webpolicy.xml /t:100 /o:\\resultserver\results

```
Para analisar uma política de segurança para a UO de servidores Webno arquivo webpolicy. xml usando as credenciais do DomainAdmin, digite:
```
scwcmd analyze /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)