---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: Hora precisa no Windows Server 2016
description: A precisão da sincronização de hora no Windows Server 2016 foi substancialmente aprimorada, mantendo, ao mesmo tempo, a compatibilidade completa do NTP com versões mais antigas do Windows.
author: eross-msft
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 3320c67d52978f0e9abaae7d5bec9b4fcb727fd6
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315087"
---
# <a name="accurate-time-for-windows-server-2016"></a>Hora precisa no Windows Server 2016

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou posterior

O Serviço de Horário do Windows é um componente que usa um modelo de plug-in para provedores de sincronização de hora do cliente e do servidor.  Há dois provedores de cliente internos no Windows e há plug-ins de terceiros disponíveis. Um provedor usa o [NTP (RFC 1305)](https://tools.ietf.org/html/rfc1305) ou o [MS-NTP](https://msdn.microsoft.com/library/cc246877.aspx) para sincronizar a hora do sistema local com um servidor de referência em conformidade com o NTP e/ou o MS-NTP. O outro provedor destina-se ao Hyper-V e sincroniza as VMs (máquinas virtuais) com o host do Hyper-V.  Quando houver vários provedores, o Windows escolherá o melhor provedor usando o nível de camada primeiro, seguido pelo atraso de raiz, dispersão de raiz e, por fim, a diferença de horário.

> [!NOTE]
> Para obter uma visão geral rápida do Serviço de Horário do Windows, assista a este [vídeo de visão geral de alto nível](https://aka.ms/WS2016TimeVideo).

Neste tópico, abordaremos... estes tópicos conforme estão relacionados com a habilitação da hora precisa: 

- Aprimoramentos
- Medidas
- Práticas recomendadas

> [!IMPORTANT]
> Um adendo referenciado pelo artigo Hora precisa do Windows 2016 pode ser baixado [aqui](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf).  Esse documento fornece mais detalhes sobre nossas metodologias de teste e medição.

> [!NOTE] 
> O modelo de plug-in do provedor de horário do Windows está [documentado no TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

## <a name="domain-hierarchy"></a>Hierarquia de domínio
As configurações autônomas e de domínio funcionam de maneira diferente.

- Os membros do domínio usam um protocolo NTP seguro, que usa a autenticação para garantir a segurança e a autenticidade da referência de tempo.  Os membros do domínio são sincronizados com um relógio mestre determinado pela hierarquia de domínio e um sistema de pontuação.  Em um domínio, há uma camada hierárquica de camadas de tempo, na qual cada controlador de domínio aponta para um controlador de domínio pai com uma camada de tempo mais precisa.  A hierarquia é resolvida como o controlador de domínio primário ou um controlador de domínio na floresta raiz ou um controlador de domínio com o sinalizador de domínio do GTIMESERV, o que indica um Servidor de Horário Certo para o domínio.  Confira a seção [Especificar um serviço de hora confiável local usando o GTIMESERV] abaixo.

- Os computadores autônomos são configurados para usar o time.windows.com por padrão.  Esse nome é resolvido pelo servidor DNS, que deverá apontar para um recurso pertencente à Microsoft.  Como todas as referências de tempo localizadas remotamente, as interrupções de rede podem impedir a sincronização.  As cargas de tráfego de rede e os caminhos de rede assimétrica poderão reduzir a precisão da sincronização de hora.  Para uma precisão de 1 ms, você não pode depender de fontes de horário remotas.

Como os convidados do Hyper-V terão, pelo menos, dois provedores de tempo do Windows à escolha, a hora do host e o NTP, você poderá ver comportamentos diferentes com Domínio ou Autônomo na execução como convidado.

> [!NOTE] 
> Para obter mais informações sobre a hierarquia de domínio e o sistema de pontuação, confira [“O que é o Serviço de Horário do Windows?”](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) .

> [!NOTE]
> A camada é um conceito usado nos provedores NTP e Hyper-V; o valor delas indica a localização dos relógios na hierarquia.  A camada 1 é reservada para o relógio de nível mais alto e a camada 0 é reservada para o hardware considerado preciso e que tenha pouco ou nenhum atraso associado a ele.  A camada 2 se comunica com os servidores da camada 1, a camada 3 com a camada 2 e assim por diante.  Embora uma camada mais baixa geralmente indique um relógio mais preciso, é possível encontrar discrepâncias.  Além disso, o W32Time aceita apenas a hora da camada 15 ou abaixo.  Para ver a camada de um cliente, use *w32tm /query /status*.

## <a name="critical-factors-for-accurate-time"></a>Fatores críticos para a hora precisa
Em cada caso, para uma hora precisa, há três fatores críticos:

1. **Relógio de origem sólido**: o relógio de origem no domínio precisa ser estável e preciso. Isso geralmente significa a instalação de um dispositivo GPS ou apontá-lo para uma origem da camada 1, levando o nº 3 em conta. A analogia continua: se você tiver dois barcos na água e estiver tentando medir a altitude de um em comparação com o outro, a precisão será melhor se o barco de origem for muito estável e não estiver se movendo. O mesmo acontece com o tempo e, se o relógio de origem não estiver estável, a cadeia inteira de relógios sincronizados será afetada e ampliada em cada fase. Ele também precisa estar acessível porque as interrupções na conexão interferirão na sincronização da hora. E, finalmente, ele precisa ser seguro. Se a referência de tempo não for mantida corretamente ou for operada por uma parte potencialmente mal-intencionada, você poderá expor seu domínio a ataques baseados em tempo.
2. **Relógio de cliente estável**: um relógio de cliente estável garante que o descompasso natural do oscilador possa ser confinado.  O NTP usa várias amostras de potencialmente vários servidores NTP para condicionar e disciplinar o relógio dos computadores locais.  Ele não percorre as alterações de horário, mas, em vez disso, reduz ou acelera o relógio local para que você se aproxime da hora exata rapidamente e fique preciso entre as solicitações do NTP.  No entanto, se o oscilador do relógio do computador cliente não for estável, mais flutuações entre os ajustes poderão ocorrer e os algoritmos que o Windows usa para condicionar o relógio não funcionarão com precisão.  Em alguns casos, atualizações de firmware podem ser necessárias para obter uma hora precisa.
3. **Comunicação do NTP simétrica**: é essencial que a conexão para a comunicação do NTP seja simétrica.  O NTP usa cálculos para ajustar o tempo que pressupõem que o patch de rede seja simétrico.  Se o caminho que o pacote NTP usa para o servidor levar um período diferente para ser retornado, a precisão será afetada.  Por exemplo, o caminho pode ser alterado devido a alterações na topologia de rede ou os pacotes estão sendo roteados por meio de dispositivos com velocidades de interface diferentes.

Para dispositivos com bateria, móveis e portáteis, é necessário considerar estratégias diferentes.  De acordo com nossa recomendação, a manutenção de hora precisa exige que o relógio seja disciplinado uma vez por segundo, o que se correlaciona com a Frequência de Atualização do Relógio. Essas configurações consumirão mais energia da bateria do que o esperado e podem interferir nos modos de economia de energia disponíveis no Windows para esses dispositivos. Os dispositivos com bateria também têm alguns modos de energia que interrompem a execução de todos os aplicativos, o que interfere na capacidade do W32Time de disciplinar o relógio e manter a hora precisa. Além disso, primeiramente, os relógios em dispositivos móveis podem não ser muito precisos.  As condições do ambiente afetam a precisão do relógio. Um dispositivo móvel pode passar de uma condição de ambiente para a seguinte, o que pode interferir na capacidade de manter a hora precisa.  Portanto, a Microsoft não recomenda que você configure dispositivos portáteis com bateria usando configurações de alta precisão. 

## <a name="why-is-time-important"></a>Por que a hora é importante?  
Há muitas razões diferentes pelas quais você pode necessitar da hora precisa.  O caso típico do Windows é o Kerberos, que exige 5 minutos de precisão entre o cliente e o servidor.  No entanto, há muitas outras áreas que podem ser afetadas pela precisão do tempo, incluindo:


- Regulamentações do governo como:
    - 50 ms de precisão para FINRA nos EUA
    - 1 ms para ESMA (MiFID II) na UE.
- Algoritmos de criptografia
- Sistemas distribuídos, como BDs de Documentos e Cluster/SQL/Exchange
- Estrutura de blockchain para transações de bitcoin
- Logs distribuídos e análise de ameaças 
- Replicação do AD
- PCI (Payment Card Industry), atualmente, precisão de 1 segundo



[!INCLUDE [windows-server-2016-improvements](windows-server-2016-improvements.md)]
