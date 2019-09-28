---
title: Realizar o hash prévio e o pré-carregamento de conteúdo em servidores de cache hospedado (opcionais)
description: Este tópico faz parte do guia de implantação do BranchCache para o Windows Server 2016, que demonstra como implantar o BranchCache em modos de cache distribuídos e hospedados para otimizar o uso de largura de banda WAN em filiais.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 5a09d9f1-1049-447f-a9bf-74adf779af27
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1a0694ebe30c5a927fb2f1152adc792f8b0c9157
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356384"
---
# <a name="prehashing-and-preloading-content-on-hosted-cache-servers-optional"></a>Realizar o hash prévio e o pré-carregamento de conteúdo em servidores de cache hospedado (opcionais)

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este procedimento para forçar a criação de informações de conteúdo – também chamadas de hashes-em servidores Web e de arquivos habilitados para BranchCache. Você também pode reunir os dados em arquivos e servidores Web em pacotes que podem ser transferidos para servidores de cache hospedados remotos.  Isso fornece a capacidade de pré-carregar conteúdo em servidores de cache hospedados remotos para que os dados estejam disponíveis para o primeiro acesso para cliente.  
  
Você deve ser membro de **Administradores**ou equivalente para executar este procedimento.  
  
### <a name="to-prehash-content-and-preload-the-content-on-hosted-cache-servers"></a>Para colocar o conteúdo de pré-hash e pré-carregar o conteúdo em servidores de cache hospedados  
  
1.  Faça logon no arquivo ou servidor Web que contém os dados que você deseja pré-carregar e identifique as pastas e os arquivos que você deseja carregar em um ou mais servidores de cache hospedados remotamente.  
  
2.  Execute o Windows PowerShell como administrador. Para cada pasta e arquivo, execute o comando `Publish-BCFileContent` ou o comando `Publish-BCWebContent`, dependendo do tipo de servidor de conteúdo, para disparar a geração de hash e para adicionar dados a um pacote de dados.  
  
3.  Depois que todos os dados tiverem sido adicionados ao pacote de dados, exporte-os usando o comando `Export-BCCachePackage` para produzir um arquivo de pacote de dados.  
  
4.  Mova o arquivo de pacote de dados para os servidores de cache hospedado remoto usando sua opção de tecnologia de transferência de arquivo.  FTP, SMB, HTTP, DVD e discos rígidos portáteis são Transportações viáveis.  
  
5.  Importe o arquivo de pacote de dados nos servidores de cache hospedado remoto usando o comando `Import-BCCachePackage`.  
  

