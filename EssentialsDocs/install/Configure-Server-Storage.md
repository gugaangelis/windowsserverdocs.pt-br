---
title: Configurar armazenamento do servidor
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef7ddcdd-3a74-40ca-9487-c3a6fc5155a5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6de485f6fd46464ba707bc0871f60ac2fec5a1db
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="configure-server-storage"></a>Configurar armazenamento do servidor

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

## <a name="sample-hard-disk-configurations"></a>Exemplos de configurações de disco rígido  
 A tabela a seguir sugere exemplos de configurações de disco rígido. As estimativas são baseadas no uso típico e funcionalidade, mas eles não tratam de questões que afetam o desempenho ideal. Você pode usar qualquer tipo de discos rígidos com suporte para essas configurações (como SATA ou SCSI), com base nas preferências e necessidades do cliente.  
  
> [!IMPORTANT]
>   Windows Server Essentials deve ser instalado como volume c: e o tamanho do volume deve ter pelo menos 60 GB. É recomendável que você crie duas partições no disco do sistema operacional e não usa a unidade c: (partição do sistema) para armazenar dados comerciais.  
  
|Nível de servidor|Configuração de disco|  
|------------------|------------------------|  
|Entrada|-Dois discos físicos<br /><br /> -Configurado como um conjunto RAID 1 espelhado que contém o seguinte:<br /><br /> -Volume c? 60 GB<br /><br /> -D: volume? 1000 GB|  
|Médio|-Três discos físicos<br /><br /> -Configurado como um conjunto RAID 5 que contém o seguinte:<br /><br /> -Volume c? 60 GB<br /><br /> -D: volume? 1500 GB|  
|Alto|-Cinco ou mais discos físicos totais<br /><br /> -Dois discos de um conjunto RAID 1 espelhado que contém o volume c:? 100 GB<br /><br /> -Discos todos os itens restantes em um conjunto RAID 5 que contém o seguinte:<br /><br /> -D: volume? 1500 GB<br /><br /> -E: volume? 1500 GB|  
  
 Essas recomendações levam em conta o tamanho do sistema operacional instalado, o tamanho médio do armazenamento de dados que usa o servidor e o crescimento do armazenamento de dados esperado ao longo do tempo de vida do servidor. Os volumes podem ser partições em um único disco físico ou podem ser colocados em discos físicos separados. Como o servidor armazena dados importantes para o cliente, é recomendável que você use vários discos físicos e ajuda a proteger seus dados do cliente s usando espaços de armazenamento ou de RAID hardware.  
  
## <a name="configuring-your-server-backup"></a>Configurando o backup do servidor  
 Além dos discos rígidos internos no servidor, os clientes devem considerar o uso de discos rígidos externos USB para backups. Idealmente, o cliente tenha pelo menos dois discos rígidos externos com capacidade suficiente para fazer backup de todos os dados no servidor. Se forem usados discos rígidos externos, o cliente poderá transferir um disco externo cada noite, para proteger ainda mais os dados.  
  
## <a name="partition-configuration"></a>Configuração de partição  
 Durante a configuração inicial do servidor, um conjunto de pastas padrão do servidor que incluem pastas compartilhadas e a pasta de backup do computador cliente são criadas na maior partição de dados no disco 0.  
  
## <a name="see-also"></a>Consulte também  

 [Introdução ao Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)

 [Introdução ao Windows Server Essentials ADK](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Criar e personalizar a imagem](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](../install/Additional-Customizations.md)   
 [Preparando a imagem para implantação](../install/Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](../install/Testing-the-Customer-Experience.md)

