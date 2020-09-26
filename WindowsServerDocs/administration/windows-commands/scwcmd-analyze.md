---
title: scwcmd analyze
description: Artigo de referência para o comando scwcmd analyze, que determina se um computador está em conformidade com uma política.
ms.topic: reference
ms.assetid: 0259271b-be5b-48d7-a51d-8b9b6786efb4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0d891262ebf04b1b8e604bc4756a3ca05888f8fa
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388602"
---
# <a name="scwcmd-analyze"></a>scwcmd analyze

> Aplica-se a: Windows Server 2012 R2 e Windows Server 2012

Determina se um computador está em conformidade com uma política. Os resultados são retornados em um arquivo. xml.

Esse comando também aceita uma lista de nomes de computador como entrada. Para exibir os resultados em seu navegador, use a **exibição scwcmd** e especifique `%windir%\security\msscw\TransformFiles\scwanalysis.xsl` como a transformação. Xsl.

## <a name="syntax"></a>Sintaxe

```
scwcmd analyze [[[/m:<computername> | /ou:<OuName>] /p:<policy>] | /i:<computerlist>] [/o:<resultdir>] [/u:<username>] [/pw:<password>] [/t:<threads>] [/l] [/e]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| opção`<computername>` | Especifica o nome NetBIOS, o nome DNS ou o endereço IP do computador a ser analisado. Se o parâmetro **/m** for especificado, o parâmetro **/p** também deverá ser especificado. |
| /ou`<OuName>` | Especifica o FQDN (nome de domínio totalmente qualificado) de uma UO (unidade organizacional) em Active Directory Domain Services. Se o parâmetro **/ou** for especificado, o parâmetro **/p** também deverá ser especificado. Todos os computadores na UO serão analisados em relação à política fornecida. |
| /p`<policy>` | Especifica o caminho e o nome do arquivo de política. XML a ser usado para executar a análise. |
| /i`<computerlist>` | Especifica o caminho e o nome de arquivo de um arquivo. XML que contém uma lista de computadores junto com seus arquivos de política esperados. Todos os computadores no arquivo. XML serão analisados em relação aos seus arquivos de política correspondentes. Um arquivo. XML de exemplo é `%windir%\security\SampleMachineList.xml` . |
| /o`<resultdir>` | Especifica o caminho e o diretório em que os arquivos de resultado da análise devem ser salvos. O padrão é o diretório atual. |
| /u`<username>` | Especifica uma credencial de usuário alternativa a ser usada ao executar a análise em um computador remoto. O padrão é o usuário conectado. |
| pt`<password>` | Especifica uma credencial de usuário alternativa a ser usada ao executar a análise em um computador remoto. O padrão é a senha do usuário conectado. |
| /t:`<threads>` | Especifica o número de operações de análise pendentes simultâneas que devem ser mantidas durante a análise. O intervalo de valores é 1-1000, com um valor padrão de 40. |
| /l | Faz com que o processo de análise seja registrado. Um arquivo de log será gerado para cada computador que está sendo analisado. Os arquivos de log serão armazenados no mesmo diretório que os arquivos de resultado. Use a opção **/o** para especificar o diretório dos arquivos de resultado. |
| /e | Registre um evento no log de eventos do aplicativo se uma incompatibilidade for encontrada. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para analisar uma política de segurança em relação ao arquivo *webpolicy.xml*, digite:

```
scwcmd analyze /p:webpolicy.xml
```

Para analisar uma política de segurança no computador chamado *WebServer* em relação ao arquivo *webpolicy.xml* usando as credenciais da conta *WebAdmin* , digite:

```
scwcmd analyze /m:webserver /p:webpolicy.xml /u:webadmin
```

Para analisar uma política de segurança em relação ao arquivo *webpolicy.xml*, com um *máximo de 100 threads*e gerar os resultados para um arquivo chamado Results no compartilhamento *resultserver* , digite:

```
scwcmd analyze /i:webpolicy.xml /t:100 /o:\\resultserver\results
```

Para analisar uma política de segurança para a *UO de servidores* Webno arquivo *webpolicy.xml* usando as credenciais *DomainAdmin* , digite:

```
scwcmd analyze /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de configuração de scwcmd](scwcmd-configure.md)

- [comando de registro scwcmd](scwcmd-register.md)

- [comando scwcmd rollback](scwcmd-rollback.md)

- [comando scwcmd transform](scwcmd-transform.md)

- [comando scwcmd View](scwcmd-view.md)