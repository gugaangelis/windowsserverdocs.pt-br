---
title: Habilitar ou desabilitar a Proteção de disco
description: Saiba como usar a proteção de disco com os serviços do MultiPoint
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 00aba4c4-0244-4b39-8c85-c46fd96e1d6a
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 7f51491ff81077ec3d6eeb5d42322350a68bb018
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859249"
---
# <a name="enable-or-disable-disk-protection"></a>Habilitar ou desabilitar a Proteção de disco
O recurso Proteção de Disco permite que você redefina seu sistema MultiPoint Services para um estado especificado sempre que o sistema for reiniciado. Usando a Proteção de Disco, os usuários podem temporariamente fazer alterações no sistema MultiPoint Services e, em seguida, essas alterações são descartadas quando o servidor é reiniciado. Exemplos de alterações que serão descartadas quando o servidor for reiniciado incluem a personalização do perfil de um usuário, a gravação de arquivos, a alteração de configurações ou a instalação de aplicativos.  
  
## <a name="enable-disk-protection"></a>Habilitar proteção de disco  
  
1.  No MultiPoint Manager, clique na guia **página inicial** e, em seguida, em * nome do computador ***tarefas**, clique em **habilitar proteção de disco**.  
  
2.  Analise as informações e clique em **OK**.  
  
Depois que o sistema for reiniciado, serão descartadas quaisquer alterações feitas no sistema, incluindo novos aplicativos que são instalados, em cada reinicialização subsequente.  
  
## <a name="disable-disk-protection"></a>Desabilitar proteção de disco  
  
1.  No MultiPoint Manager, clique na guia **página inicial** e, em seguida, em * nome do computador ***tarefas**, clique em **desabilitar proteção de disco**.  
  
2.  Analise as informações e clique em **OK**.  
  
Depois que o sistema for reiniciado, todas as alterações feitas no sistema, incluindo os aplicativos instalados no servidor, são permanentes e não serão descartadas na próxima vez em que o sistema for reiniciado.  
  
