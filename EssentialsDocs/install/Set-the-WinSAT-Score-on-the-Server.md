---
title: Definir a Pontuação do WinSAT no Servidor
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 911dc494-0f8f-4723-93d6-2106f914b906
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 2f469f902f28642890723552ac460e844281c7b8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819829"
---
# <a name="set-the-winsat-score-on-the-server"></a>Definir a Pontuação do WinSAT no Servidor

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você deve definir a pontuação de CPU do WinSAT para um servidor que esteja executando o sistema operacional Windows Server Essentials para otimizar a resolução de streaming de vídeo. Faça isso criando e instalando o arquivo .xml que contém as informações sobre a pontuação do WinSAT.  
  
## <a name="obtain-the-winsat-cpu-score"></a>Obtenha a pontuação da CPU do WinSAT  
 O programa chamado WinServerSAT.exe, fornecido com o OPK, detecta a pontuação da CPU do WinSAT e a coloca no arquivo WinServerSAT.xml que pode ser lido pelo sistema operacional.  
  
#### <a name="to-obtain-the-winsat-cpu-score"></a>Para obter a pontuação da CPU do WinSAT  
  
1. Copie o Resources\WinServerSAT\\* na mídia do ADK para o computador de referência.  
  
2. Neste computador, abra uma janela do Prompt de Comando elevada.  
  
3. Se a pasta %ProgramFiles%\Windows Server\Bin\OEM não existir, digite o seguinte comando e pressione Enter.  
  
    **mkdir "%ProgramFiles%\Windows Server\Bin\OEM"**  
  
4. Digite o seguinte comando e pressione Enter.  
  
    **WinServerSAT. exe "%ProgramFiles%\Windows Server\Bin\OEM\WinServerSAT.xml"**  
  
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
