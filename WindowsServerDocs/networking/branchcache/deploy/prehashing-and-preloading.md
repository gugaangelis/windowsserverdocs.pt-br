---
title: Realizar o hash prévio e o pré-carregamento de conteúdo em servidores de cache hospedado (opcionais)
description: Este tópico faz parte do BranchCache Deployment Guide para Windows Server 2016, que demonstra como implantar o BranchCache nos modos de cache hospedado e distribuído para otimizar o uso de largura de banda WAN em filiais.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 5a09d9f1-1049-447f-a9bf-74adf779af27
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b421132a44240520e3e3ba294623584c36b18ab4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866997"
---
# <a name="prehashing-and-preloading-content-on-hosted-cache-servers-optional"></a>Realizar o hash prévio e o pré-carregamento de conteúdo em servidores de cache hospedado (opcionais)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para forçar a criação de informações de conteúdo - também chamadas hashes - em servidores de arquivos e da Web habilitado para BranchCache. Você também pode coletar os dados nos servidores de arquivo e da web em pacotes que podem ser transferidos para os servidores de cache hospedado remoto.  Isso fornece a capacidade de pré-carregar o conteúdo em servidores remotos de cache hospedado para que os dados estão disponíveis para o primeiro acesso de cliente.  
  
Você deve ser um membro da **administradores**, ou equivalente a executar este procedimento.  
  
### <a name="to-prehash-content-and-preload-the-content-on-hosted-cache-servers"></a>Pré-hash do conteúdo e pré-carregar o conteúdo em servidores de cache hospedado  
  
1.  Faça logon para o arquivo ou o servidor Web que contém os dados que você deseja pré-carregar e identificar as pastas e arquivos que você deseja carregar em um ou mais servidores de cache hospedado remoto.  
  
2.  Execute o Windows PowerShell como administrador. Para cada pasta e arquivo, execute as `Publish-BCFileContent` comando ou o `Publish-BCWebContent` comando, dependendo do tipo de servidor de conteúdo, para disparar a geração de hashes e adicionar dados a um pacote de dados.  
  
3.  Depois que todos os dados adicionados ao pacote de dados, exportá-lo usando o `Export-BCCachePackage` comando para produzir um arquivo de pacote de dados.  
  
4.  Mova o arquivo de pacote de dados para os servidores de cache hospedado remoto usando tecnologia de transferência de arquivo de sua escolha.  FTP, SMB, HTTP, DVD e discos rígidos portáteis são viáveis todos os transportes.  
  
5.  Importar o arquivo de pacote de dados nos servidores remotos de cache hospedado usando o `Import-BCCachePackage` comando.  
  

