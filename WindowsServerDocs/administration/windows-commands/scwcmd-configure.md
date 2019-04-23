---
title: Configurar scwcmd
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6528b9dc-3d82-4228-b734-ed717458d74c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 31838ac7299cc30a7b7dde3beb47669df772487c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857497"
---
# <a name="scwcmd-configure"></a>Scwcmd: configure

> Aplica-se a: Windows Server 2012 R2, Windows Server 2012

Aplica uma política de segurança gerados pelo Assistente de configuração de segurança ACS a um computador. Essa ferramenta de linha de comando também aceita uma lista de nomes de computador como entrada.

## <a name="syntax"></a>Sintaxe

```
scwcmd configure [[[/m:<ComputerName> | /ou:<OuName>] /p:<Policy>] | /i:<ComputerList>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/m:\<ComputerName>|Especifica o nome NetBIOS, nome DNS ou endereço IP do computador para configurar. Se o **/m** parâmetro for especificado, o **p** parâmetro também deve ser especificado.|
|/ou:\<OuName>|Especifica o nome de domínio totalmente qualificado (FQDN) de uma unidade organizacional (UO) no Active Directory Domain Services. Se o **/ou** parâmetro for especificado, o **p** parâmetro também deve ser especificado. Todos os computadores na UO serão analisados de acordo com a política determinada.|
|/p:\<política >|Especifica o nome de arquivo e caminho do arquivo a ser usado para executar a configuração de diretiva. XML.|
|/i:\<ComputerList>|Especifica o nome de arquivo e caminho de um arquivo. XML que contém uma lista de computadores juntamente com seus arquivos de política esperado. Todos os computadores no arquivo. XML serão configurados de acordo com seus arquivos de política correspondentes. Um exemplo de arquivo. XML é % windir%\security\SampleMachineList.xml.|
|/u:\<UserName>|Especifica uma credencial de usuário alternativo a ser usado ao configurar um computador remoto. O padrão é o conectado no usuário.|
|/pw:\<senha >|Especifica uma credencial de usuário alternativo a ser usado ao configurar um computador remoto. O padrão é a senha do usuário conectado.|
|/t:\<Threads>|Especifica o número de operações pendentes simultâneas que deveria ser mantida durante o processo de configuração (valor padrão = 40, MinValue = 1, MaxValue = 1000).|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Scwcmd.exe só está disponível em computadores que executam o Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="BKMK_Examples"></a>Exemplos

Para configurar uma política de segurança contra webpolicy.xml o arquivo, digite:
```
scwcmd configure /p:webpolicy.xml
```
Para configurar uma política de segurança para o computador no 172.16.0.0 contra webpolicy.xml o arquivo usando as credenciais da conta webadmin, digite:
```
scwcmd configure /m:172.16.0.0 /p:webpolicy.xml /u:webadmin
```
Para configurar uma política de segurança em todos os computadores em campusmachines.xml a lista com um máximo de 100 threads, digite:
```
scwcmd configure /i:campusmachines.xml /t:100
```
Para configurar uma política de segurança em todos os computadores na OU WebServers contra webpolicy.xml o arquivo usando as credenciais da conta DomainAdmin, digite:
```
scwcmd configure /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)