---
title: Solucionando problemas do AD FS - regras de declarações
description: Este documento descreve como solucionar problemas de sintaxe de regra de declarações com o AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 027b2afc9e580253ec820e7e5be14419387ddd44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837917"
---
# <a name="ad-fs-troubleshooting---claims-rules-syntax"></a>Sintaxe de regras de declarações do AD FS de solução de problemas-
Uma declaração é uma instrução que um sujeito faz sobre si mesmo ou outro assunto.  Declarações emitidas por uma terceira parte confiável e são determinados valores de um ou mais e, em seguida, empacotados em tokens de segurança emitidos pelo servidor do AD FS.  Este artigo lida com a sintaxe de declarações e a criação.  Para obter informações sobre declarações de emissão ver [AD FS de solução de problemas - emissão de declarações](ad-fs-tshoot-claims-issuance.md).

>[!NOTE]  
>Você pode usar [ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) sobre o [ADFS ajuda](https://adfshelp.microsoft.com) site para ajudar a solucionar problemas de declarações.   

## <a name="how-claim-rules-are-processed"></a>Como as regras de declaração são processadas
As regras de declaração são processadas por meio de [pipeline de declarações](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md) usando o [mecanismo de declarações](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md). O mecanismo de declarações é um componente lógico do serviço de federação que examina o conjunto de declarações de entrada apresentadas por um usuário e, em seguida, dependendo da lógica em cada regra, produzirá um conjunto de declarações de saída.

## <a name="how-to-create-a-claim-rule"></a>Como criar uma regra de declaração
Regras de declaração são criadas separadamente para cada relação de confiança federada dentro do Serviço de Federação e não são compartilhadas entre várias relações de confiança. Você pode criar uma regra de um [modelo de regra de declaração](../../ad-fs/technical-reference/determine-the-type-of-claim-rule-template-to-use.md), começar do zero por meio da criação de regra usando o [linguagem de regra de declaração](../../ad-fs/technical-reference/when-to-use-a-custom-claim-rule.md) ou usar o Windows PowerShell para personalizar uma regra.

## <a name="understanding-the-components-of-the-claim-rule-language"></a>Entendendo os componentes da linguagem de regras de declaração
A linguagem de regra de declaração consiste dos seguintes componentes, separados pela "= >" operador:

- Uma condição - usada para verificar declarações de entrada e determinar se a instrução de emissão da regra deve ser executada.  Ele representa uma expressão lógica que deve ser avaliada como true para executar a parte do corpo de regra.

- Uma instrução de emissão

Exemplo:

```c:[type == "Name", value == "domain user"] => issue(type = "Role", value = "employee");``` 

A declaração a seguir tem o seguinte:
- condição - `c:[type == "Name", value == "domain user"] ` -avalia a declaração de entrada se o nome de conta do windows é um usuário de domínio
- emissão - `issue(type = "Role", value = "employee")` - se a condição for verdadeira, adiciona uma nova declaração à declaração de entrada com a função do funcionário.

Para obter mais informações sobre declarações e a sintaxe, consulte [The Role of the Claims Rule Language](../../ad-fs/technical-reference/the-role-of-the-claim-rule-language.md).

## <a name="claims-rule-editor"></a>Editor de regras de declarações
Verificação de sintaxe é executada pelo editor de regra de declarações depois de ter concluído a declaração e clicar **Okey**.  Portanto, se você tiver a sintaxe incorreta, em seguida, o editor permitirá que você sabe.

![declarações](media/ad-fs-tshoot-claims/claims1.png)

## <a name="event-logs"></a>Logs de eventos
Ao procurar tentando solucionar problemas de uma declaração usando os logs a melhor abordagem é procurar saída de declarações.  Você pode procurar eventos 1000 e 1001 no log de eventos.

![declarações](media/ad-fs-tshoot-claims/claims2.png)

## <a name="creating-a-sample-application"></a>Criando um aplicativo de exemplo
Você também pode criar um aplicativo de exemplo os ecos suas declarações.  Por exemplo, você pode usar um aplicativo de exemplo e crie uma terceira que tem a mesma declaração que você está tentando solucionar problemas e veja se o aplicativo tem quaisquer problemas com essa declaração.

![declarações](media/ad-fs-tshoot-claims/claim4.png)

Um aplicativo web bom exemplo está disponível aqui.  Esse aplicativo é um aplicativo web simples que retorna as declarações que ele recebe de parte confiável.  Para usá-lo, você precisa editar o aplicativo Web. config por:
- alterando https://app1.contoso.com/sampapp para a URL que você usará para hospedar o sampapp
- alterar todas as instâncias de sts.contoso.com apontar o servidor de Federação do AD FS
- Substituindo a impressão digital com sua impressão digital

![declarações](media/ad-fs-tshoot-claims/claims3.png)

O seguinte [artigo de blog](https://blogs.technet.microsoft.com/tangent_thoughts/2015/02/20/install-and-configure-a-simple-net-4-5-sample-federated-application-samapp/) tem instruções detalhadas, excelente, para configurar isso.

## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)