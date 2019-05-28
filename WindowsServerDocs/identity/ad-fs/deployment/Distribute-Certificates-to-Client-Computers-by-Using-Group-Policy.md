---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: Distribuir certificados para computadores cliente usando a diretiva de grupo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 11cdd9c75ca588ebeac9387e6512fee439621bf8
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192160"
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>Distribuir certificados para computadores cliente usando a diretiva de grupo


Você pode usar o procedimento a seguir para enviar por push apropriados Secure Sockets Layer \(SSL\) certificados \(ou o equivalente em certificados vinculados a uma raiz confiável\) para servidores de federação de conta servidores de federação de recursos e servidores Web para cada computador cliente na floresta do parceiro de conta usando a diretiva de grupo.  
  
Associação na **Admins. do domínio** ou **administradores de empresa**, ou equivalente, nos serviços de domínio do Active Directory \(AD DS\) é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>Para distribuir certificados para computadores cliente usando a diretiva de grupo  
  
1.  Em um controlador de domínio na floresta da organização do parceiro de conta, inicie o **gerenciamento de política de grupo** encaixar\-no.  
  
2.  Localizar um objeto de diretiva de grupo existente \(GPO\) ou criar um novo GPO para conter as configurações de certificado. Certifique-se de que o GPO está associado com o domínio, site ou unidade organizacional \(UO\) onde residem as contas de usuário e computador apropriadas.  
  
3.  À direita\-clique no GPO e, em seguida, clique em **editar**.  
  
4.  Na árvore de console, abra **configuração do computador\\diretivas\\configurações do Windows\\as configurações de segurança\\diretivas de chave pública**, à direita\-clique **Autoridades de certificação raiz confiáveis**e, em seguida, clique em **importação**.  
  
5.  Na página **Bem-vindo ao Assistente para Importação de Certificados**, clique em **Avançar**.  
  
6.  Sobre o **arquivo a ser importado** página, digite o caminho para os arquivos de certificado apropriado \(por exemplo, \\ \\fs1\\c$\\fs1.cer\)e, em seguida, clique em **Próxima**.  
  
7.  Sobre o **Store do certificado** , clique em **colocar todos os certificados no repositório a seguir**e, em seguida, clique em **próximo**.  
  
8.  Sobre o **Concluindo o Assistente de importação de certificado** página, verifique se as informações que você forneceu são precisas e, em seguida, clique em **concluir**.  
  
9. Repita as etapas 2 a 6 para adicionar certificados adicionais para cada um dos servidores de federação no farm.  
