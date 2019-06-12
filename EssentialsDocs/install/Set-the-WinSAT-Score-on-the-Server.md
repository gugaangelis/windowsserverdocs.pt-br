---
title: Definir a Pontuação do WinSAT no Servidor
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
ms.openlocfilehash: 4e5ce037c7a8c802419cd980fc0272c4f687c6a6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433456"
---
# <a name="set-the-winsat-score-on-the-server"></a>Definir a Pontuação do WinSAT no Servidor

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você deve definir a pontuação da CPU do WinSAT para um servidor que está executando o sistema de operacional Windows Server Essentials para otimizar a resolução do vídeo de streaming. Faça isso criando e instalando o arquivo .xml que contém as informações sobre a pontuação do WinSAT.  
  
## <a name="obtain-the-winsat-cpu-score"></a>Obtenha a pontuação da CPU do WinSAT  
 O programa chamado WinServerSAT.exe, fornecido com o OPK, detecta a pontuação da CPU do WinSAT e a coloca no arquivo WinServerSAT.xml que pode ser lido pelo sistema operacional.  
  
#### <a name="to-obtain-the-winsat-cpu-score"></a>Para obter a pontuação da CPU do WinSAT  
  
1. Copie o Resources\WinServerSAT\\* na mídia ADK para o computador de referência.  
  
2. Neste computador, abra uma janela do Prompt de Comando elevada.  
  
3. Se a pasta %ProgramFiles%\Windows Server\Bin\OEM não existir, digite o seguinte comando e pressione Enter.  
  
    **mkdir "%ProgramFiles%\Windows Server\Bin\OEM"**  
  
4. Digite o seguinte comando e pressione Enter.  
  
    **WinServerSAT.exe "%ProgramFiles%\Windows Server\Bin\OEM\WinServerSAT.xml"**  
  
   O exemplo a seguir mostra o conteúdo XML do arquivo WinServerSAT.xml que foi criado.  
  
```  
  
<?xml version="1.0" encoding="UTF-16"?>  
<WinSAT>  
   <CpuScore description="WinSAT CPU score">WinSAT_Score</CpuScore>  
</WinSAT>  
```  
  
 O *WinSAT_Score* foi substituído pelo valor detectado no servidor.  
  
> [!IMPORTANT]
>  É necessário remover os arquivos WinServerSAT.exe, winsat.prx, winsat.wmv e WinSATEncode.wmv do computador de referência antes de capturar a imagem.
