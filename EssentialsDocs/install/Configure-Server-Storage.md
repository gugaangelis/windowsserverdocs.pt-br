---
title: Configurar Armazenamento de Servidor
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: ef7ddcdd-3a74-40ca-9487-c3a6fc5155a5
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: be4bf42f2fe287ad6bd96b9ea5735147ec253e0d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89621838"
---
# <a name="configure-server-storage"></a>Configurar Armazenamento de Servidor

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

## <a name="sample-hard-disk-configurations"></a>Exemplo de configurações de disco rígido
 A tabela a seguir sugere exemplos de configurações de disco rígido. As estimativas baseiam-se no uso e no funcionamento normais e não abrangem os problemas que afetariam um desempenho ideal. Você pode usar qualquer tipo de disco rígido com suporte para essas configurações (SATA ou SCSI, por exemplo), com base nas preferências e nas necessidades do cliente.

> [!IMPORTANT]
>   O Windows Server Essentials deve ser instalado como C: volume e o tamanho do volume deve ser de pelo menos 60 GB. Recomenda-se criar duas partições no disco do sistema operacional, e não usar C: (partição do sistema) para armazenar nenhum dado de negócio.

|Nível de servidor|Configuração de disco|
|------------------|------------------------|
|Entrada|-Dois discos físicos<br /><br /> -Configurado como um conjunto espelhado RAID 1 que contém o seguinte:<br /><br /> -C: volume? 60 GB<br /><br /> -D: volume? 1000 GB|
|Médio|-Três discos físicos<br /><br /> -Configurado como um conjunto de RAID 5 que contém o seguinte:<br /><br /> -C: volume? 60 GB<br /><br /> -D: volume? 1500 GB|
|Alta|-Cinco ou mais discos físicos totais<br /><br /> -Dois discos em um conjunto espelhado RAID 1 que contém o volume C:? 100 GB<br /><br /> -Todos os discos restantes em um conjunto de RAID 5 que contém o seguinte:<br /><br /> -D: volume? 1500 GB<br /><br /> -E: volume? 1500 GB|

 Essas recomendações consideram o tamanho do sistema operacional instalado, o tamanho médio do armazenamento de dados utilizado pelo servidor e o crescimento esperado de armazenamento de dados ao longo do tempo de vida útil do servidor. Os volumes podem ser partições em um disco físico único ou podem ser colocados em discos físicos separados. Como o servidor armazena dados importantes para seu cliente, é recomendável que você use vários discos físicos e ajude a proteger os dados do cliente usando o RAID de hardware ou os espaços de armazenamento.

## <a name="configuring-your-server-backup"></a>Configurando o backup do servidor
 Além dos discos rígidos internos no servidor, os clientes devem considerar o uso de discos rígidos USB externos para fazer backups. O ideal seria que o cliente tivesse, no mínimo, dois discos rígidos externos com capacidade suficiente para fazer backup de todos os dados do servidor. Usando discos rígidos externos, o cliente poderá retirar um disco do local por noite para proteger ainda mais os dados.

## <a name="partition-configuration"></a>Configuração da partição
 Durante a Configuração Inicial do servidor, um conjunto de pastas de servidor padrão, que inclui pastas compartilhadas e a pasta de backup do computador cliente, é criado na maior partição de dados no Disco 0.

## <a name="see-also"></a>Consulte Também

 [Introdução com o Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md) [criando e personalizando a imagem](Creating-and-Customizing-the-Image.md) [personalizações adicionais](Additional-Customizations.md) [preparando a imagem para](Preparing-the-Image-for-Deployment.md) [testar a implantação da experiência do cliente](Testing-the-Customer-Experience.md)

