---
title: AD FS solução de problemas – regras de declarações
description: Este documento descreve como solucionar problemas de sintaxe de regra de declarações com AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.openlocfilehash: 349b673b7c062fd8f14d9a9fd857e1d7c859d3de
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954192"
---
# <a name="ad-fs-troubleshooting---claims-rules-syntax"></a>AD FS solução de problemas – sintaxe de regras de declarações
Uma declaração é uma instrução que um assunto faz sobre si mesmo ou outro assunto.  As declarações são emitidas por uma terceira parte confiável e recebem um ou mais valores e, em seguida, são empacotadas em tokens de segurança que são emitidos pelo servidor de AD FS.  Este artigo lida com a sintaxe e a criação de declarações.  Para obter informações sobre a emissão de declarações [, consulte AD FS solução de problemas-emissão de declarações](ad-fs-tshoot-claims-issuance.md).

>[!NOTE]
>Você pode usar o [ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) no site de [ajuda do ADFS](https://adfshelp.microsoft.com) para ajudar a solucionar problemas de declarações.

## <a name="how-claim-rules-are-processed"></a>Como as regras de declaração são processadas
As regras de declaração são processadas por meio do [pipeline de declarações](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md) usando o [mecanismo de declarações](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md). O mecanismo de declarações é um componente lógico do Serviço de Federação que examina o conjunto de declarações de entrada apresentadas por um usuário e em seguida, dependendo da lógica em cada regra, produzirá um conjunto de declarações de saída.

## <a name="how-to-create-a-claim-rule"></a>Como criar uma regra de declaração
Regras de declaração são criadas separadamente para cada relação de confiança federada dentro do Serviço de Federação e não são compartilhadas entre várias relações de confiança. Você pode criar uma regra a partir de um [modelo de regra de declaração](../../ad-fs/technical-reference/determine-the-type-of-claim-rule-template-to-use.md), começar do zero criando a regra usando o idioma da regra de [declaração](../../ad-fs/technical-reference/when-to-use-a-custom-claim-rule.md) ou usar o Windows PowerShell para personalizar uma regra.

## <a name="understanding-the-components-of-the-claim-rule-language"></a>Entendendo os componentes da linguagem de regras de declaração
O idioma da regra de declaração consiste nos seguintes componentes, separados pelo operador "=>":

- Uma condição-usada para verificar as declarações de entrada e determinar se a instrução de emissão da regra deve ser executada.  Representa uma expressão lógica que deve ser avaliada como true para executar a parte do corpo da regra.

- Uma instrução de emissão

Exemplo:

```c:[type == "Name", value == "domain user"] => issue(type = "Role", value = "employee");```

A declaração a seguir tem o seguinte:
- condição – `c:[type == "Name", value == "domain user"] ` avalia a declaração de entrada de se o nome da conta do Windows é um usuário de domínio
- emissão – `issue(type = "Role", value = "employee")` se a condição for verdadeira, o adicionará uma nova declaração à declaração de entrada com a função de funcionário.

Para obter mais informações sobre declarações e a sintaxe, consulte [a função do idioma da regra de declarações](../../ad-fs/technical-reference/the-role-of-the-claim-rule-language.md).

## <a name="claims-rule-editor"></a>Editor de regras de declarações
A verificação de sintaxe é executada pelo editor de regras de declarações depois que você concluir a declaração e clicar em **OK**.  Portanto, se você tiver a sintaxe incorreta, o editor informará que você saberá.

![declarações](media/ad-fs-tshoot-claims/claims1.png)

## <a name="event-logs"></a>Logs de eventos
Ao tentar solucionar problemas de uma declaração usando os logs, a melhor abordagem é procurar a saída de declarações.  Você pode procurar eventos 1000 e 1001 no log de eventos.

![declarações](media/ad-fs-tshoot-claims/claims2.png)

## <a name="creating-a-sample-application"></a>Criando um aplicativo de exemplo
Você também pode criar um aplicativo de exemplo para ecoar suas declarações.  Por exemplo, você pode usar um aplicativo de exemplo e criar uma terceira parte confiável que tenha a mesma declaração que está tentando solucionar problemas e ver se o aplicativo tem algum problema com essa declaração.

![declarações](media/ad-fs-tshoot-claims/claim4.png)

Um bom aplicativo Web de exemplo está disponível aqui.  Esse aplicativo é um aplicativo Web simples que retorna as declarações recebidas da terceira parte confiável.  Para usar isso, você precisa editar o aplicativo web.config da seguinte forma:
- alterando https://app1.contoso.com/sampapp para a URL que você usará para hospedar o sampapp
- alterando todas as instâncias de sts.contoso.com para apontar AD FS servidor de Federação
- Substituindo a impressão digital pela impressão digital

![declarações](media/ad-fs-tshoot-claims/claims3.png)

O [artigo de blog](/archive/blogs/tangent_thoughts/install-and-configure-a-simple-net-4-5-sample-federated-application-samapp) a seguir tem instruções excelentes e detalhadas para configurar isso.

## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)
