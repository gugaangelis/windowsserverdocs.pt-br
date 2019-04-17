---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: "Distribuir certificados para computadores cliente usando a política de grupo"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d3a7e05e4d16565b17b69de254e353df749bbc3a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>Distribuir certificados para computadores cliente usando a política de grupo

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Você pode usar o procedimento a seguir para empilhar os certificados Secure Sockets Layer \(SSL\) apropriados \ (ou equivalente certificados que são encadeados para um root\ confiável) para servidores de federação de conta, servidores de Federação do recurso e servidores Web para cada computador cliente na floresta do parceiro de conta usando a política de grupo.  
  
A associação ao grupo **Admins. do domínio** ou **administradores corporativos**, ou equivalente, no Active Directory Domain Services \(AD DS\) é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>Para distribuir certificados para computadores cliente usando a política de grupo  
  
1.  Em um controlador de domínio na floresta de organização do parceiro de conta, inicie o **Group Policy Management** snap\-in.  
  
2.  Encontre um \(GPO\) objeto de política de grupo existente ou criar um novo GPO para conter as configurações de certificado. Certifique-se de que o GPO é associado um domínio, site ou unidade organizacional \(OU\) onde residem as contas de usuário e computador apropriadas.  
  
3.  Clique Right\ o GPO e clique em **editar**.  
  
4.  Na árvore de console, abra **políticas de chave do computador Configuration\\Policies\\Windows Settings\\Security Settings\\Public**, clique right\ **autoridades de certificação confiáveis**e clique em **importar**.  
  
5.  Sobre o **bem-vindo ao Assistente para importação de certificados** página, clique em **próxima**.  
  
6.  Sobre o **arquivo a ser importado** página, digite o caminho para os arquivos de certificado apropriado \ (por exemplo, \\\fs1\\c$\\fs1.cer\) e, em seguida, clique em **próxima**.  
  
7.  Sobre o **repositório de certificados** página, clique em **colocar todos os certificados no repositório a seguir**e clique em **próxima**.  
  
8.  Sobre o **concluir o Assistente para importação de certificados** de página, verifique se as informações que você forneceu estão precisas e, em seguida, clique em **concluir**.  
  
9. Repita as etapas 2 a 6 para adicionar certificados adicionais para cada um dos servidores de federação no farm.  
