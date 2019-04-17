---
title: "Defina a pontuação WinSAT no servidor"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 911dc494-0f8f-4723-93d6-2106f914b906
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 77866acccac13ac48da8779700c8654f2c7f3277
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="set-the-winsat-score-on-the-server"></a>Defina a pontuação WinSAT no servidor

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você deve definir a pontuação WinSAT CPU para um servidor que está executando o sistema operacional Windows Server Essentials para otimizar a resolução de vídeo de streaming. Você pode fazer isso criando e instalando o arquivo. XML que contém as informações de pontuações do WinSAT.  
  
## <a name="obtain-the-winsat-cpu-score"></a>Obter a pontuação WinSAT CPU  
 Você terá um programa no OPK chamado WinServerSAT.exe que detecta a pontuação WinSAT CPU e coloca essas informações no arquivo WinServerSAT.xml que lê o sistema operacional.  
  
#### <a name="to-obtain-the-winsat-cpu-score"></a>Para obter a pontuação WinSAT CPU  
  
1.  Copie o Resources\WinServerSAT\\ * na mídia do ADK no computador de referência.  
  
2.  No computador de referência, abra uma janela de Prompt de comando com privilégios elevados.  
  
3.  Se a pasta %ProgramFiles%\Windows server\bin\oem. não existir, digite o seguinte comando e pressione Enter.  
  
     **mkdir "%ProgramFiles%\Windows server\bin\oem."**  
  
4.  Digite o seguinte comando e pressione Enter.  
  
     **WinServerSAT.exe "%ProgramFiles%\Windows Server\Bin\OEM\WinServerSAT.xml"**  
  
 O exemplo a seguir mostra o conteúdo XML do arquivo WinServerSAT.xml que é criado.  
  
```  
  
<?xml version="1.0" encoding="UTF-16"?>  
<WinSAT>  
   <CpuScore description="WinSAT CPU score">WinSAT_Score</CpuScore>  
</WinSAT>  
```  
  
 Onde *WinSAT_Score* é substituído com o valor que é descoberto no servidor.  
  
> [!IMPORTANT]
>  Você deve remover WinServerSAT.exe, winsat.prx, WinSAT e WinSATEncode.wmv arquivos do computador de referência antes de capturar a imagem.
