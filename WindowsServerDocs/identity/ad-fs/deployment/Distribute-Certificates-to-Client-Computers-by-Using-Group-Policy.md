---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: Distribuir certificados para computadores cliente usando Política de Grupo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1d9692bc099174f15b77e792087f4c7055bf85d1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359625"
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>Distribuir certificados para computadores cliente usando Política de Grupo


Você pode usar o procedimento a seguir para enviar por push os certificados apropriados protocolo SSL \(SSL @ no__t-1 \(or certificados equivalentes que se encadeadom a uma raiz confiável @ no__t-3 para servidores de Federação de conta, servidores de Federação de recursos e Servidores Web para cada computador cliente na floresta do parceiro de conta usando Política de Grupo.  
  
A associação em **Administradores de domínio** ou administradores de **empresa**, ou equivalente, em Active Directory Domain Services \(AD DS @ no__t-3 é o mínimo necessário para concluir este procedimento.  Examinar os detalhes sobre como usar as contas apropriadas e as associações de grupo em \( [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477) http:\/\/go.Microsoft.com\/fwlink\/? \=LinkId 83477.\)   
  
### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>Para distribuir certificados para computadores cliente usando o Política de Grupo  
  
1.  Em um controlador de domínio na floresta da organização do parceiro de conta, inicie o snap do **política de grupo Management** @ no__t-1in.  
  
2.  Localize um objeto de Política de Grupo existente \(GPO @ no__t-1 ou crie um novo GPO para conter as configurações de certificado. Verifique se o GPO está associado ao domínio, site ou unidade organizacional \(OU @ no__t-1 em que residem as contas de usuário e computador apropriadas.  
  
3.  Right @ no__t-0click o GPO e clique em **Editar**.  
  
4.  Na árvore de console, abra **configuração do computador @ no__t-1Policies @ no__t-2Windows Settings @ no__t-3Security Settings @ no__t-4Public Key Policies**, Right @ no__t-5click **Trusted root certifications**e clique em **Import** .  
  
5.  Na página **Bem-vindo ao Assistente para Importação de Certificados**, clique em **Avançar**.  
  
6.  Na página **arquivo a ser importado** , digite o caminho para os arquivos de certificado apropriados \(Para exemplo, \\ @ no__t-3fs1 @ no__t-4c $ @no__t -5fs1. cer @ no__t-6 e clique em **Avançar**.  
  
7.  Na página **repositório de certificados** , clique em **Coloque todos os certificados no repositório a seguir**e, em seguida, clique em **Avançar**.  
  
8.  Na página **concluindo o assistente para importação de certificados** , verifique se as informações fornecidas são precisas e clique em **concluir**.  
  
9. Repita as etapas de 2 a 6 para adicionar certificados adicionais para cada um dos servidores de Federação no farm.  
