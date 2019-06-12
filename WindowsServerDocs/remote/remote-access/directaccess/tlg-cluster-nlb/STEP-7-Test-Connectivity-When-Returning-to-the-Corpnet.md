---
title: ETAPA 7 de testar a conectividade ao retornar para a rede corporativa
description: Este tópico faz parte do guia de laboratório de teste - demonstração do DirectAccess em um Cluster com Windows NLB para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a7252d0-6db8-4a9d-98ee-75082ecd2929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: af22b1bbc923b9a06e4aebb910690050af1b7492
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446637"
---
# <a name="step-7-test-connectivity-when-returning-to-the-corpnet"></a>ETAPA 7 de testar a conectividade ao retornar para a rede corporativa

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Muitos dos seus usuários se moverá entre locais remotos e da rede corporativa, portanto, é importante que quando eles retornarem da rede corporativa que eles sejam capazes de acessar os recursos sem precisar fazer qualquer configuração é alterado. Acesso remoto torna isso possível, pois quando o cliente do DirectAccess retornará para a rede corporativa, ele é capaz de fazer uma conexão com o servidor de local de rede. Depois que a conexão HTTPS é estabelecida com êxito para o servidor de local de rede, o cliente do DirectAccess desabilita a configuração do cliente DirectAccess e usa uma conexão direta à rede corporativa.  
  
### <a name="test-connectivity-on-client1"></a>Testar a conectividade em CLIENT1  
  
1. Encerre o CLIENT1 e, em seguida, desconecte o CLIENT1 do comutador virtual ou sub-rede da rede doméstica e conectá-lo ao comutador virtual ou sub-rede Corpnet. Ativar o CLIENT1 e faça logon como CORP\User1.  
  
2. Abra uma janela elevada do Windows PowerShell, digite **ipconfig/all**, e pressione ENTER. A saída irá indicar que o CLIENT1 possui um endereço IP local e que não há nenhum ativo 6to4, Teredo ou IP-HTTPS túnel.  
  
3. Testar a conectividade com o compartilhamento de rede no APP2. Sobre o **inicie** tela, digite<strong>\\\APP2\Files</strong>, e pressione ENTER. Você poderá abrir o arquivo nessa pasta.  
  


