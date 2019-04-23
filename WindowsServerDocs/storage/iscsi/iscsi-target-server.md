---
title: iSCSI Target Server Overview
TOCTitle: iSCSI Target Server
ms.prod: windows-server-threshold
ms.technology: storage-iscsi
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: 14dd373b71c1be2f1a4ff157b9c631530532fc80
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837937"
---
# <a name="iscsi-target-server-overview"></a>Visão geral do servidor de destino iSCSI

Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico fornece uma visão geral do servidor de destino, um serviço de função no Windows Server que permite que você disponibilizar por meio do protocolo iSCSI de armazenamento iSCSI. Isso é útil para fornecer acesso ao armazenamento em seu servidor do Windows para clientes que não podem se comunicar sobre o arquivo do Windows nativo compartilhando o protocolo SMB.

O servidor de destino iSCSI é ideal para o seguinte:

* **Inicialização de rede e sem disco**   ao usar adaptadores de rede habilitados para inicialização ou um carregador de software, você pode implantar centenas de servidores sem disco. Com o servidor de destino iSCSI, a implantação é rápida. Em testes internos da Microsoft, 256 computadores foram implantados em 34 minutos. Usando discos rígidos virtuais diferentes, você pode economizar até 90% do espaço de armazenamento usado para as imagens do sistema operacional. Isso é ideal para grandes implantações de imagens de sistemas operacionais idênticos, por exemplo, em máquinas virtuais que executam o Hyper-V ou em clusters de computação de alto desempenho (HPC).

* **Armazenamento de aplicativos de servidor**   alguns aplicativos requerem armazenamento em bloco. O Servidor de Destino iSCSI pode fornecer esses aplicativos com armazenamento em bloco continuamente disponível. Como o armazenamento é acessível remotamente, ele também pode consolidar o armazenamento em bloco para escritórios centrais ou filiais.

* **Armazenamento heterogêneo**   o servidor de destino iSCSI dá suporte a iniciadores iSCSI que não são da Microsoft, facilitando compartilhar armazenamentos em servidores em um ambiente de software misto.

* **Ambientes de desenvolvimento, teste, demonstração e laboratório**   quando o servidor de destino iSCSI estiver habilitado, um computador que executa o sistema operacional Windows Server torna-se um dispositivo de armazenamento de bloco acessível pela rede. Isso é útil para testar aplicativos antes da implantação em uma SAN (Rede de Área de Armazenamento).

## <a name="block-storage-requirements"></a>Requisitos de armazenamento em bloco

A habilitação do Servidor do destino iSCSI para fornecer armazenamento em bloco aproveita sua rede Ethernet existente. Não é necessário hardware adicional. Se a alta disponibilidade for um critério importante, considere criar um cluster de alta disponibilidade. Você vai precisar de armazenamento compartilhado para o cluster de alta disponibilidade; seja um armazenamento Fibre Channel de hardware ou uma matriz de armazenamento SAS (serial attached SCSI).

Se você habilitar o cluster convidado, terá que fornecer armazenamento em bloco. Qualquer servidor que execute o software Windows Server com o Servidor de Destino iSCSI pode oferecer armazenamento em bloco.

## <a name="see-also"></a>Consulte também

[Armazenamento de bloco de destino, iSCSI como](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh848268(v%3dws.11))  
[O que há de novo no servidor no Windows Server de destino iSCSI](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn305893(v%3dws.11))

