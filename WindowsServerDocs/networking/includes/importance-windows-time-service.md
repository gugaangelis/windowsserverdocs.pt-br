---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server-threshold
ms:topic: include
ms.openlocfilehash: 4e8e3234b89630bf16148eef644f0c6607ad38bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852337"
---
## <a name="importance-of-time-protocols"></a>Importância dos protocolos de tempo
Protocolos de tempo a comunicação entre dois computadores para trocar informações de tempo e, em seguida, usar essas informações para sincronizar seus relógios. Com o protocolo de tempo de serviço de tempo do Windows, um cliente solicita informações de tempo de um servidor e sincroniza seu relógio com base nas informações que são recebidas.
  
O serviço de tempo do Windows usa o NTP para ajudar a sincronizar a hora em uma rede. NTP é um protocolo de tempo de Internet que inclui os algoritmos de disciplina necessários para a sincronização de relógios. NTP é um protocolo de tempo mais preciso do que o simples SNTP Network Time Protocol () que é usado em algumas versões do Windows; No entanto, W32Time continua a dar suporte a SNTP para habilitar a compatibilidade com versões anteriores com computadores que executam serviços de tempo com base no SNTP como o Windows 2000.

Há vários motivos diferentes, que talvez seja necessário horário com precisão.  O caso típico para Windows é Kerberos, que exige que 5 minutos de precisão entre o cliente e servidor.  No entanto, há muitas outras áreas que podem ser afetadas pela precisão de tempo incluindo:


- As normas governamentais, como:
    - precisão de 50 ms para FINRA nos EUA
    - 1 ms ESMA (MiFID II) na UE.
- Algoritmos de criptografia
- Sistemas distribuídos, como Cluster/SQL/trocar e bancos de dados de documento
- Estrutura de Blockchain para transações de bitcoin
- Logs distribuídos e análise de ameaças 
- Replicação do AD
- PCI (Payment Card Industry), a precisão de segundo atualmente 1