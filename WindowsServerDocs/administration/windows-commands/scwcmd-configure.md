---
title: scwcmd configure
description: Artigo de referência para o comando scwcmd configure, que aplica uma política de segurança gerada pelo assistente de configuração de segurança (SCW) a um computador.
ms.topic: reference
ms.assetid: 6528b9dc-3d82-4228-b734-ed717458d74c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a57d2142f8fc7fd788a5669c5318ff6444c34734
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388583"
---
# <a name="scwcmd-configure"></a>scwcmd configure

> Aplica-se a: Windows Server 2012 R2 e Windows Server 2012

Aplica uma política de segurança gerada pelo assistente de configuração de segurança (SCW) a um computador. Essa ferramenta de linha de comando também aceita uma lista de nomes de computador como entrada.

## <a name="syntax"></a>Sintaxe

```
scwcmd configure [[[/m:<computername> | /ou:<OuName>] /p:<policy>] | /i:<computerlist>] [/u:<username>] [/pw:<password>] [/t:<threads>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| opção`<computername>` | Especifica o nome NetBIOS, o nome DNS ou o endereço IP do computador a ser configurado. Se o parâmetro **/m** for especificado, o parâmetro **/p** também deverá ser especificado. |
| /ou`<OuName>` | Especifica o FQDN (nome de domínio totalmente qualificado) de uma UO (unidade organizacional) em Active Directory Domain Services. Se o parâmetro **/ou** for especificado, o parâmetro **/p** também deverá ser especificado. Todos os computadores da OU serão configurados em relação à política especificada. |
| /p`<policy>` | Especifica o caminho e o nome do arquivo de política. XML a ser usado para executar a configuração. |
| /i`<computerlist>` | Especifica o caminho e o nome de arquivo de um arquivo. XML que contém uma lista de computadores junto com seus arquivos de política esperados. Todos os computadores no arquivo. XML serão analisados em relação aos seus arquivos de política correspondentes. Um arquivo. XML de exemplo é `%windir%\security\SampleMachineList.xml` . |
| /u`<username>` | Especifica uma credencial de usuário alternativa a ser usada ao executar a configuração em um computador remoto. O padrão é o usuário conectado. |
| pt`<password>` | Especifica uma credencial de usuário alternativa a ser usada ao executar a configuração em um computador remoto. O padrão é a senha do usuário conectado. |
| /t:`<threads>` | Especifica o número de operações de configuração pendentes simultâneas que devem ser mantidas durante a análise. O intervalo de valores é 1-1000, com um valor padrão de 40. |
| /l | Faz com que o processo de análise seja registrado. Um arquivo de log será gerado para cada computador que está sendo analisado. Os arquivos de log serão armazenados no mesmo diretório que os arquivos de resultado. Use a opção **/o** para especificar o diretório dos arquivos de resultado. |
| /e | Registre um evento no log de eventos do aplicativo se uma incompatibilidade for encontrada. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para configurar uma política de segurança no arquivo *webpolicy.xml*, digite:

```
scwcmd configure /p:webpolicy.xml
```

Para configurar uma política de segurança para o computador em *172.16.0.0* em relação ao arquivo *webpolicy.xml* usando as credenciais da conta *WebAdmin* , digite:

```
scwcmd configure /m:172.16.0.0 /p:webpolicy.xml /u:webadmin
```

Para configurar uma política de segurança em todos os computadores da lista *campusmachines.xml* com um *máximo de 100 threads*, digite:

```
scwcmd configure /i:campusmachines.xml /t:100
```

Para configurar uma política de segurança para a *UO de servidores* Webno arquivo *webpolicy.xml* usando as credenciais *DomainAdmin* , digite:

```
scwcmd configure /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando scwcmd analyze](scwcmd-analyze.md)

- [comando de registro scwcmd](scwcmd-register.md)

- [comando scwcmd rollback](scwcmd-rollback.md)

- [comando scwcmd transform](scwcmd-transform.md)

- [comando scwcmd View](scwcmd-view.md)