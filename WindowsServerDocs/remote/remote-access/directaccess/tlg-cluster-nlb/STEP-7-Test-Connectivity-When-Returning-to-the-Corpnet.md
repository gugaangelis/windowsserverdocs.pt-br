---
title: ETAPA 7 testar a conectividade ao retornar ao corpnet
description: Este tópico faz parte do guia de laboratório de teste – demonstre o DirectAccess em um cluster com o NLB do Windows para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a7252d0-6db8-4a9d-98ee-75082ecd2929
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 491533ae5d141de4ab4f15126d8977cf15c8f7f4
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314682"
---
# <a name="step-7-test-connectivity-when-returning-to-the-corpnet"></a>ETAPA 7 testar a conectividade ao retornar ao corpnet

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Muitos de seus usuários se moverão entre locais remotos e o corpnet, portanto, é importante que, quando eles retornam ao corpnet, eles possam acessar recursos sem precisar fazer nenhuma alteração de configuração. O acesso remoto torna isso possível porque quando o cliente DirectAccess retorna para o corpnet, ele é capaz de estabelecer uma conexão com o servidor de local de rede. Depois que a conexão HTTPS for estabelecida com êxito ao servidor de local de rede, o cliente DirectAccess desabilitará a configuração do cliente DirectAccess e usará uma conexão direta com o corpnet.  
  
### <a name="test-connectivity-on-client1"></a>Testar a conectividade em CLIENT1  
  
1. Desligue o CLIENT1 e desconecte o CLIENT1 da sub-rede HomeNet ou do comutador virtual e conecte-o à sub-rede corpnet ou ao comutador virtual. Ative o CLIENT1 e faça logon como CORP\User1.  
  
2. Abra uma janela do Windows PowerShell com privilégios elevados, digite **ipconfig/all**e pressione Enter. A saída indicará que CLIENT1 tem um endereço IP local e que não há nenhum túnel 6to4, Teredo ou IP-HTTPS ativo.  
  
3. Teste a conectividade com o compartilhamento de rede em APP2. Na tela **Iniciar** , digite<strong>\\\APP2\FILES</strong>e pressione Enter. Você poderá abrir o arquivo nessa pasta.  
  


