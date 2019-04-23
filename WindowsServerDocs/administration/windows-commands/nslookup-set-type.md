---
title: nslookup set type
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5248e314-fac1-413e-81dc-bbe0a0873ba5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: accf8ccbe147a831ee306dd670384b90844ff4fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835947"
---
# <a name="nslookup-set-type"></a>nslookup set type

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o tipo de registro de recurso para a consulta.
## <a name="syntax"></a>Sintaxe
```
set type=<ResourceRecordtype>
```
## <a name="parameters"></a>Parâmetros
<ResourceRecordtype> Especifica um tipo de registro de recurso DNS. O tipo de registro de recurso padrão é A. A tabela a seguir lista os valores válidos para esse comando.
|Valor|Descrição|
|-----|--------|
|A|Especifica um endereço IP de computador|
|QUALQUER|Especifica um endereço IP de computador.|
|CNAME|Especifica um nome canônico para um alias.|
|GID|Especifica um identificador de grupo de um nome de grupo.|
|HINFO|Especifica o tipo de sistema operacional e a CPU de um computador.|
|MB|Especifica um nome de domínio da caixa de correio.|
|MG|Especifica um membro do grupo de email.|
|MINFO|Especifica as informações da lista de caixa de correio ou email.|
|MR|Especifica o nome de domínio.|
|MX|Especifica o servidor de mensagens.|
|NS|Especifica um servidor de nomes DNS para a zona nomeada.|
|PTR|Especifica um computador nome se a consulta é um endereço IP; Caso contrário, especifica o ponteiro para outras informações.|
|SOA|Especifica a início de autoridade para uma zona DNS.|
|TXT|Especifica as informações de texto.|
|UID|Especifica o identificador de usuário.|
|UINFO|Especifica as informações do usuário.|
|WKS|Descreve um serviço conhecido.|
{Ajuda | ?}
Exibe um resumo breve dos **nslookup** subcomandos
## <a name="remarks"></a>Comentários
-   O **definir o tipo** comando executa a mesma função que o **definir querytype** comando.
-   Para obter mais informações sobre tipos de registro de recurso, consulte a solicitação de comentários (Rfc) 1035.
## <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[nslookup definir querytype](nslookup-set-querytype.md)
