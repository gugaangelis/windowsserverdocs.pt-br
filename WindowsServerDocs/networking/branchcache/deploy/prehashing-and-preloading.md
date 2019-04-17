---
title: Prehashing e conteúdo de pré-carregamento em servidores de Cache hospedado (opcionais)
description: Este tópico faz parte do BranchCache implantação guia para Windows Server 2016, que demonstra como implantar BranchCache nos modos de cache hospedado e distribuídos para otimizar o uso de largura de banda WAN em filiais.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 5a09d9f1-1049-447f-a9bf-74adf779af27
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c3d1ed62c6dca5b1de0ff560fde0a2e43ed0d080
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="prehashing-and-preloading-content-on-hosted-cache-servers-optional"></a>Prehashing e conteúdo de pré-carregamento em servidores de Cache hospedado (opcionais)

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para forçar a criação de conteúdo informações - também chamadas de hashes - nos servidores Web e arquivo BranchCache habilitados. Você também pode coletar os dados no arquivo e servidores web em pacotes que podem ser transferidos para servidores remotos cache hospedado.  Isso fornece a capacidade de pré-carregar conteúdo em servidores remotos cache hospedado para que os dados estão disponíveis para o primeiro acesso do cliente.  
  
Você deve ser um membro do **administradores**, ou equivalente a executar este procedimento.  
  
### <a name="to-prehash-content-and-preload-the-content-on-hosted-cache-servers"></a>Prehash conteúdo e pré-carregar o conteúdo em servidores de cache hospedado  
  
1.  Faça logon no arquivo ou servidor Web que contém os dados que você deseja pré-carregar e identificar as pastas e arquivos que você deseja carregar em um ou mais servidores remotos cache hospedado.  
  
2.  Execute o Windows PowerShell como administrador. Para cada pasta e arquivo, execute o `Publish-BCFileContent`comando ou o `Publish-BCWebContent`comando, dependendo do tipo de servidor de conteúdo, para acionar a geração de hash e adicionar dados a um pacote de dados.  
  
3.  Depois que todos os dados foi adicionado para o pacote de dados, exportá-la usando o `Export-BCCachePackage`comando para produzir um arquivo de pacote de dados.  
  
4.  Mova o arquivo do pacote de dados para os servidores remotos cache hospedado usando sua escolha de tecnologia de transferência de arquivo.  FTP, SMB, HTTP, DVD e discos rígidos portáteis são transportes tudo viáveis.  
  
5.  Importe o arquivo de pacote de dados nos servidores do cache hospedado remoto usando o `Import-BCCachePackage`comando.  
  

