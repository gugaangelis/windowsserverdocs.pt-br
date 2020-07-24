---
title: iSCSI Target Server Overview
TOCTitle: iSCSI Target Server
ms.prod: windows-server
ms.technology: storage-iscsi
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: 34515988c46b680701cc39b3948fcd53645741f4
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86961348"
---
# <a name="iscsi-target-server-overview"></a>Visão geral do servidor de destino iSCSI

Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico fornece uma breve visão geral do servidor de destino iSCSI, um serviço de função no Windows Server que permite disponibilizar o armazenamento por meio do protocolo iSCSI. Isso é útil para fornecer acesso ao armazenamento no Windows Server para clientes que não podem se comunicar por meio do protocolo SMB nativo de compartilhamento de arquivos do Windows.

O servidor de destino iSCSI é ideal para o seguinte:

* Inicialização de rede **e disco**     Usando adaptadores de rede com capacidade de inicialização ou um carregador de software, você pode implantar centenas de servidores sem disco. Com o servidor de destino iSCSI, a implantação é rápida. Em testes internos da Microsoft, 256 computadores foram implantados em 34 minutos. Usando discos rígidos virtuais diferentes, você pode economizar até 90% do espaço de armazenamento usado para as imagens do sistema operacional. Isso é ideal para grandes implantações de imagens de sistemas operacionais idênticos, por exemplo, em máquinas virtuais que executam o Hyper-V ou em clusters de computação de alto desempenho (HPC).

* Armazenamento de aplicativos **do servidor**     Alguns aplicativos exigem armazenamento em bloco. O Servidor de Destino iSCSI pode fornecer esses aplicativos com armazenamento em bloco continuamente disponível. Como o armazenamento é acessível remotamente, ele também pode consolidar o armazenamento em bloco para escritórios centrais ou filiais.

* **Armazenamento heterogêneo**     o servidor de destino iSCSI dá suporte a iniciadores iSCSI não Microsoft, facilitando o compartilhamento de armazenamento em servidores em um ambiente de software misto.

* Ambientes de desenvolvimento **, teste, demonstração e laboratório**     Quando o servidor de destino iSCSI está habilitado, um computador executando o sistema operacional Windows Server torna-se um dispositivo de armazenamento de blocos acessível pela rede. Isso é útil para testar aplicativos antes da implantação em uma SAN (Rede de Área de Armazenamento).

## <a name="block-storage-requirements"></a>Requisitos de armazenamento em bloco

A habilitação do Servidor do destino iSCSI para fornecer armazenamento em bloco aproveita sua rede Ethernet existente. Não é necessário hardware adicional. Se a alta disponibilidade for um critério importante, considere criar um cluster de alta disponibilidade. Você vai precisar de armazenamento compartilhado para o cluster de alta disponibilidade; seja um armazenamento Fibre Channel de hardware ou uma matriz de armazenamento SAS (serial attached SCSI).

Se você habilitar o cluster convidado, terá que fornecer armazenamento em bloco. Qualquer servidor que execute o software Windows Server com o Servidor de Destino iSCSI pode oferecer armazenamento em bloco.

## <a name="see-also"></a>Consulte Também

[Armazenamento em bloco de destino iSCSI, Como](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh848268(v%3dws.11))  
[Novidades no servidor de destino iSCSI do Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn305893(v%3dws.11))
