---
title: Proteger o volume do sistema com proteção de disco
description: Fornece informações sobre a proteção de disco para os serviços do MultiPoint
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 18694665-ed65-4d84-8505-f460cf3df907
author: evaseydl
manager: scotman
ms.author: evas
ms.openlocfilehash: a663bb27a7480066c997844d68b33774f9032e92
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855207"
---
# <a name="protecting-the-system-volume-with-disk-protection"></a>Proteger o volume do sistema com proteção de disco
MultiPoint Services fornece a opção de apagar instantaneamente todas as alterações para o volume do sistema cada vez que o computador for iniciado. Se você habilitar o recurso de proteção de disco, todas as modificações a unidade, como corrupção de configuração ou a introdução de tipos de malware, são desfeitas na próxima vez que o computador for reiniciado. Esse é um recurso conveniente para os administradores que desejam garantir que uma imagem de softwares conhecidos de "boas" ou "ouro" é carregada sempre. Atualizações automáticas ou aplicação de patches de software pode ser agendada, por exemplo, no meio da noite. A consideração de planejamento é se você deseja ter os usuários finais capaz de fazer alterações, como instalação de software, da Internet. Com esse recurso habilitado, se desejar que os usuários para ser capaz de armazenar os arquivos, o compartilhamento de arquivos precisarão ser fora do volume do sistema.  
  
