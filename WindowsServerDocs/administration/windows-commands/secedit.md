---
title: comandos Secedit
description: Artigo de referência para os comandos Secedit, que comparam suas configurações de segurança atuais com os modelos de segurança especificados.
ms.topic: reference
ms.assetid: 58ed57ed-08e3-403d-a363-0620b358637a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ceb1a28376c17ab9d08689c7b0367dd90fdecc4f
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388283"
---
# <a name="secedit-commands"></a>comandos Secedit

Configura e analisa a segurança do sistema comparando a configuração de segurança atual com os modelos de segurança especificados.

> [!NOTE]
> O MMC (console de gerenciamento Microsoft) e o snap-in de análise e configuração de segurança não estão disponíveis no Server Core.

## <a name="syntax"></a>Sintaxe

```
secedit /analyze
secedit /configure
secedit /export
secedit /generaterollback
secedit /import
secedit /validate
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| [secedit/Analyze](secedit-analyze.md) | Permite que você analise as configurações atuais de sistemas em relação às configurações de linha de base que são armazenadas em um banco de dados.  Os resultados da análise são armazenados em uma área separada do banco de dados e podem ser exibidos no snap-in configuração de segurança e análise. |
| [secedit/configure](secedit-configure.md) | Permite que você configure um sistema com as configurações de segurança armazenadas em um banco de dados. |
| [secedit/export](secedit-export.md) | Permite exportar configurações de segurança armazenadas em um banco de dados do. |
| [/generaterollback secedit](secedit-generaterollback.md) | Permite gerar um modelo de reversão em relação a um modelo de configuração. |
| [secedit/Import](secedit-import.md) | Permite que você importe um modelo de segurança para um banco de dados para que as configurações especificadas no modelo possam ser aplicadas a um sistema ou analisadas em um sistema. |
| [/Validate secedit](secedit-validate.md) | Permite validar a sintaxe de um modelo de segurança. |

#### <a name="remarks"></a>Comentários

- Se não houver nenhum FilePath especificado, todos os nomes de FileName serão padronizados para o diretório atual.

- Os resultados da análise são armazenados em uma área separada do banco de dados e podem ser exibidos no snap-in de configuração e análise de segurança para o MMC.

- Se os modelos de segurança forem criados usando o snap-in modelo de segurança e se você executar o snap-in configuração e análise de segurança em relação a esses modelos, os seguintes arquivos serão criados:

    | Arquivo | Descrição |
    |--|--|
    | scesrv. log | <ul><li>**Local:**`%windir%\security\logs`</li><li>**Criado por:** Sistema Operacional</li><li>**Tipo de arquivo:** Texto</li><li>**Taxa de atualização:** Substituído quando `secedit analyze` , `secedit configure` `secedit export` ou `secedit import` é executado.</li><li>**Conteúdo:** Contém os resultados da análise agrupada por tipo de política.</li></ul> |
    | *nome do usuário selecionado*. sdb | <ul><li>**Local:**`%windir%\<user account>\Documents\Security\Database`</li><li>**Criado por:** Executando o snap-in de configuração e análise de segurança</li><li>**Tipo de arquivo:** Próprio</li><li>**Taxa de atualização:** Atualizado sempre que um novo modelo de segurança é criado.</li><li>**Conteúdo:** Políticas de segurança local e modelos de segurança criados pelo usuário.</li></ul> |
    | *nome do usuário selecionado*. log | <ul><li>**Local:** Definido pelo usuário, mas usa como padrão `%windir%\<user account>\Documents\Security\Logs`</li><li>**Criado por:** Executar os `secedit analyze` `secedit configure` comandos ou, ou usando o snap-in configuração e análise de segurança.</li><li>**Tipo de arquivo:** Texto</li><li>**Taxa de atualização:** Substituído quando `secedit analyze` ou `secedit configure` é executado, ou usando o snap-in configuração e análise de segurança.</li><li>**Conteúdo:** Nome do arquivo de log, data e hora e os resultados da análise ou investigação.</li></ul> |
    | *nome do usuário selecionado*. inf | <ul><li>**Local:**`%windir%\*<user account>\Documents\Security\Templates`</li><li>**Criado por:** Executando o snap-in do modelo de segurança.</li><li>**Tipo de arquivo:** Texto</li><li>**Taxa de atualização:** Substituído sempre que o modelo de segurança é atualizado.</li><li>**Conteúdo:** Contém as informações de configuração do modelo para cada política selecionada usando o snap-in do.</li></ul> |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
