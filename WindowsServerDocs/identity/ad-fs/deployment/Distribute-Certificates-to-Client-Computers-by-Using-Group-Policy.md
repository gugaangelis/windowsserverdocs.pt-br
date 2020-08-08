---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: Distribuir certificados para computadores cliente usando Política de Grupo
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 35b543a7a249130d1fd66a0d86424b5bcb0b8d96
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962890"
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>Distribuir certificados para computadores cliente usando Política de Grupo


Você pode usar o procedimento a seguir para encaminhar os \( certificados SSL protocolo SSL apropriados \) ou os \( certificados equivalentes que se encadeam a uma raiz confiável \) para servidores de Federação de conta, servidores de Federação de recursos e servidores Web para cada computador cliente na floresta do parceiro de conta usando política de grupo.

A associação em **Administradores de domínio** ou administradores de **empresa**, ou equivalente, em Active Directory Domain Services \( AD DS \) é o mínimo necessário para concluir este procedimento.  Examinar os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477) \( http: \/ \/ go.Microsoft.com \/ fwlink \/ ? LinkId \= 83477 \) .

### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>Para distribuir certificados para computadores cliente usando o Política de Grupo

1.  Em um controlador de domínio na floresta da organização do parceiro de conta, inicie o snap-in de **Gerenciamento de política de grupo** \- .

2.  Localize um GPO de objeto de Política de Grupo existente \( \) ou crie um novo GPO para conter as configurações de certificado. Verifique se o GPO está associado ao domínio, ao site ou à \( UO da unidade organizacional \) onde residem as contas de usuário e computador apropriadas.

3.  Clique com o botão direito \- do mouse no GPO e clique em **Editar**.

4.  Na árvore de console, abra **configuração do computador políticas configurações do \\ \\ Windows \\ configurações de segurança \\ políticas de chave pública**, clique com o botão direito do mouse \- em **autoridades de certificação raiz confiáveis**e clique em **importar**.

5.  Na página **Bem-vindo ao Assistente para Importação de Certificados**, clique em **Avançar**.

6.  Na página **arquivo a ser importado** , digite o caminho para os arquivos de certificado apropriados, \( por exemplo, \\ \\ FS1 \\ c $ \\ FS1. cer \) e clique em **Avançar**.

7.  Na página **repositório de certificados** , clique em **Coloque todos os certificados no repositório a seguir**e, em seguida, clique em **Avançar**.

8.  Na página **concluindo o assistente para importação de certificados** , verifique se as informações fornecidas são precisas e clique em **concluir**.

9. Repita as etapas de 2 a 6 para adicionar certificados adicionais para cada um dos servidores de Federação no farm.
