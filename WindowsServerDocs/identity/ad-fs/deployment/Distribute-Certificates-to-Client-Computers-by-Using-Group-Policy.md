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


Você pode usar o procedimento a seguir para encaminhar as protocolo SSL apropriadas \(SSL\) certificados \(ou certificados equivalentes que se encadeadom a um\) raiz confiável para servidores de Federação de conta, servidores de Federação de recursos e servidores Web para cada computador cliente na floresta do parceiro de conta usando Política de Grupo.  
  
A associação em **Administradores de domínio** ou administradores de **empresa**, ou equivalente, em Active Directory Domain Services \(AD DS\) é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=\)83477.   
  
### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>Para distribuir certificados para computadores cliente usando o Política de Grupo  
  
1.  Em um controlador de domínio na floresta da organização do parceiro de conta, inicie o snap\-de **Gerenciamento de política de grupo** no.  
  
2.  Localize um objeto de Política de Grupo existente \(GPO\) ou crie um novo GPO para conter as configurações de certificado. Verifique se o GPO está associado ao domínio, site ou unidade organizacional \(UO\) onde residem as contas de usuário e computador apropriadas.  
  
3.  À direita\-clique no GPO e, em seguida, clique em **Editar**.  
  
4.  Na árvore de console, abra **configuração do computador\\políticas\\configurações do Windows\\configurações de segurança\\políticas de chave pública**, direito\-clique em **autoridades de certificação raiz confiáveis**e clique em **importar**.  
  
5.  Na página **Bem-vindo ao Assistente para Importação de Certificados**, clique em **Avançar**.  
  
6.  Na página **arquivo a ser importado** , digite o caminho para os arquivos de certificado apropriados \(por exemplo, \\\\FS1\\c $\\FS1. cer\)e clique em **Avançar**.  
  
7.  Na página **repositório de certificados** , clique em **Coloque todos os certificados no repositório a seguir**e, em seguida, clique em **Avançar**.  
  
8.  Na página **concluindo o assistente para importação de certificados** , verifique se as informações fornecidas são precisas e clique em **concluir**.  
  
9. Repita as etapas de 2 a 6 para adicionar certificados adicionais para cada um dos servidores de Federação no farm.  
