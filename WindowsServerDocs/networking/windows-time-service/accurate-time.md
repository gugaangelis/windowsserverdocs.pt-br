---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: Hora precisa do Windows Server 2016
description: Precisão de sincronização de hora no Windows Server 2016 melhorou substancialmente, mantendo de modo completo com versões anteriores NTP compatibilidade com versões mais antigas do Windows.
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: ea4d957ee68f14f4568d3cefe664736585e50cce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864007"
---
# <a name="accurate-time-for-windows-server-2016"></a>Hora precisa do Windows Server 2016

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou posterior

O serviço de tempo do Windows é um componente que usa um modelo de plug-in para provedores de sincronização de hora do cliente e servidor.  Há dois provedores de cliente interno no Windows e plug-ins de terceiros estão disponíveis. Usa um provedor [NTP (RFC 1305)](https://tools.ietf.org/html/rfc1305) ou [MS NTP](https://msdn.microsoft.com/library/cc246877.aspx) para sincronizar a hora do sistema local para um servidor de referência em conformidade NTP e/ou MS-NTP. O outro provedor é para o Hyper-V e sincroniza as máquinas virtuais (VM) para o host do Hyper-V.  Quando existem vários provedores, o Windows escolhe o melhor provedor usando o nível de camada em primeiro lugar, seguido por um atraso de raiz, dispersão de raiz e, por fim, o deslocamento de tempo.

>[!NOTE]
>Para obter uma visão geral do serviço de tempo do Windows, dê uma olhada neste [vídeo de visão geral](https://aka.ms/WS2016TimeVideo).

<!-- Not sure what to do with the following -->
Neste tópico, discutiremos... Esses tópicos como eles se relacionam permitir tempo preciso: 

- Melhorias
- Medidas
- Práticas recomendadas

>[!IMPORTANT]
>Um adendo mencionado no artigo do Windows 2016 precisos tempo pode ser baixado [aqui](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf).  Este documento fornece mais detalhes sobre as metodologias de nossos testes e medidas.



>[!NOTE] 
>É o modelo de plug-in de provedor de tempo do windows [documentadas no TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

## <a name="domain-hierarchy"></a>Hierarquia de domínio
Configurações de domínio e autônomo funcionam de forma diferente.

- Membros do domínio usam um protocolo NTP seguro, que usa a autenticação para garantir a segurança e a autenticidade da referência de tempo.  Membros do domínio sincronizar com um relógio mestre determinado pela hierarquia de domínio e um sistema de pontuação.  Em um domínio, há uma camada hierárquica de stratums de tempo, no qual cada controlador de domínio aponta para um controlador de domínio pai com uma camada de tempo mais preciso.  A hierarquia é resolvido para o controlador de domínio primário ou um controlador de domínio na floresta de raiz ou um controlador de domínio com o sinalizador de domínio GTIMESERV, que denota um bom tempo de servidor para o domínio.  Consulte a [especificar um Local confiável tempo de serviço usando GTIMESERV](#GTIMESERV) seção abaixo.

- As máquinas autônomas são configuradas para usar time.windows.com por padrão.  Esse nome é resolvido pelo servidor DNS, que deve apontar para um recurso de propriedade da Microsoft.  Como todas as referências de hora localizado remotamente, interrupções de rede, pode impedir a sincronização.  Cargas de tráfego de rede e caminhos de rede assimétrica poderão reduzir a precisão da sincronização de tempo.  Para precisão de 1 ms, você não pode depender de uma fonte de tempo remoto.

Como convidados Hyper-V terá pelo menos dois provedores de tempo do Windows para sua escolha, a hora do host e o NTP, você poderá ver diferentes comportamentos com o domínio ou autônomo quando em execução como convidado.

> [!NOTE] 
> Para obter mais informações sobre a hierarquia de domínio e o sistema de pontuação, consulte o ["Qual é o serviço de tempo do Windows?"](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) .

> [!NOTE]
> Stratum é um conceito usado em provedores de NTP e o Hyper-V e seu valor indica o local de relógios na hierarquia.  Camada 1 é reservada para o relógio do nível mais alto e a camada 0 é reservada para o hardware considerado precisos e tem pouco ou nenhum atraso associado a ele.  Camada 2 se comunicar com servidores de camada 1, a camada 3 para a camada 2 e assim por diante.  Enquanto uma camada inferior geralmente indica um relógio mais preciso, é possível encontrar discrepâncias.  Além disso, W32time aceita apenas o tempo de stratum 15 ou abaixo.  Para ver a camada de um cliente, use *w32tm /query /status*.

## <a name="critical-factors-for-accurate-time"></a>Fatores críticos para o tempo preciso
Em todos os casos para o tempo preciso, há três fatores críticos:

1. **Relógio sólida de origem** -o relógio do código-fonte no seu domínio precisa estar estável e precisas. Isso geralmente significa instalar um dispositivo GPS ou apontando para uma fonte de camada 1, considerando #3. A analogia vai, se você tiver dois barcos sobre a água e você está tentando medir a altitude de um em comparação com os outros, a precisão é melhor se o cruzeiro de origem é muito estável e não movendo. O mesmo vale para o tempo e, se o relógio do seu código-fonte não é estável, em seguida, toda a cadeia de relógios sincronizados é afetada e aumentada em cada estágio. Ele também deve ser acessível porque interrupções na conexão irá interferir com a sincronização de hora. E, finalmente, ele deve ser seguro. Se o tempo de referência não está corretamente mantidas ou operados por usuários possivelmente mal-intencionados, você poderia expor seu domínio para ataques de tempo com base.
2. **O relógio de cliente estável** -relógios um cliente estável garante que o descompasso natural do oscilador é containable.  O NTP usa várias amostras de potencialmente vários servidores NTP a condição e o relógio de computadores locais de disciplina.  Ele não entra as alterações de tempo, mas em vez disso, reduz o ou acelera o relógio local para que você aborda o tempo preciso rapidamente e permanecer precisas entre as solicitações NTP.  No entanto, se o oscilador de relógio computador do cliente não é estável, flutuações mais entre os ajustes podem ocorrer, e os algoritmos que usa o Windows para o relógio de condição não funcionam com precisão.  Em alguns casos, as atualizações de firmware podem ser necessário tempo preciso.
3. **A comunicação de NTP simétrica** -é essencial que a conexão para a comunicação de NTP é simétrica.  NTP usa cálculos para ajustar o tempo que assumem que o patch de rede é simétrico.  Se o caminho do pacote NTP se vão para o servidor leva uma quantidade diferente de tempo para retornar, a precisão é afetada.  Por exemplo, o caminho pode alterar devido a alterações na topologia de rede ou pacotes serem roteados por meio de dispositivos que têm velocidades de interface diferente.


Para dispositivos alimentado por bateria, móveis e portátil, você deve considerar estratégias diferentes.  De acordo com a nossa recomendação, manter o horário com precisão requer o relógio para ser disciplinada uma vez por segundo, que se correlaciona com a frequência de atualização do relógio. Essas configurações serão consumir a bateria mais que o esperado e pode interferir com energia salvando modos disponíveis no Windows para esses dispositivos. Dispositivos alimentado por bateria também tem alguns modos de energia que pararem todos os aplicativos sejam executados, o que interfere capacidade da W32time para o relógio de disciplina e manter o horário com precisão. Além disso, os relógios em dispositivos móveis podem não ser muito precisos para começar.  Ambientes condições ambientais afetam a precisão do relógio e um dispositivo móvel pode mover de uma condição de ambiente para a próxima que pode interferir em sua capacidade de manter o tempo com precisão.  Portanto, a Microsoft não recomenda que você configure dispositivos portáteis de alimentado por bateria com configurações de alta precisão. 

## <a name="why-is-time-important"></a>Por que o tempo é importante?  
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



[!INCLUDE [windows-server-2016-improvements](windows-server-2016-improvements.md)]
