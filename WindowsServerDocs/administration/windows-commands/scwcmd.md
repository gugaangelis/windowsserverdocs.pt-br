---
title: scwcmd
description: Artigo de referência para a ferramenta de linha de comando scwcmd.exe incluída com o ACS (Assistente de configuração de segurança).
ms.topic: reference
ms.assetid: 188ae881-c7d4-4a7a-b967-8fdc79f5f345
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 87bd2c718bec18810026c5701b955d5ef655b53e
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388916"
---
# <a name="scwcmd"></a>scwcmd

> Aplica-se a: Windows Server 2012 R2 e Windows Server 2012

A ferramenta de linha de comando Scwcmd.exe incluída com o ACS (Assistente de configuração de segurança) pode ser usada para executar as seguintes tarefas:

- Analise um ou vários servidores com uma política gerada pelo ACS.

- Configure um ou vários servidores com uma política gerada pelo ACS.

- Registrar uma extensão de banco de dados de configuração de segurança com o ACS.

- Reverter políticas do ACS.

- Transforme uma política gerada pelo ACS em arquivos nativos com suporte pelo Política de Grupo.

- Exibir resultados da análise no formato HTML.

> [!NOTE]
> Se você usar **scwcmd** para configurar, analisar ou reverter uma política em um servidor remoto, o ACS deverá ser instalado no servidor remoto.

## <a name="syntax"></a>Sintaxe

```
scwcmd analyze
scwcmd configure
scwcmd register
scwcmd rollback
scwcmd transform
scwcmd view
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| [scwcmd analyze](scwcmd-analyze.md) | Determina se um computador está em conformidade com uma política. |
| [scwcmd configure](scwcmd-configure.md) | Aplica uma política de segurança gerada pelo ACS a um computador.|
| [scwcmd register](scwcmd-register.md) | Estende ou personaliza o banco de dados de configuração de segurança do ACS registrando um arquivo de banco de dados de configuração de segurança que contém definições de função, tarefa, serviço ou porta. |
| [scwcmd rollback](scwcmd-rollback.md) | Aplica a política de reversão mais recente disponível e, em seguida, exclui essa política de reversão. |
| [scwcmd transform](scwcmd-transform.md) | Transforma um arquivo de política de segurança gerado usando o ACS em um novo objeto de Política de Grupo (GPO) no Active Directory Domain Services. |
| [scwcmd view](scwcmd-view.md) | Renderiza um arquivo. xml usando uma transformação. xsl especificada. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
