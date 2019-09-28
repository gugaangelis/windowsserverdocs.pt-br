---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: Tempo preciso para o Windows Server 2016
description: A precisão da sincronização de tempo no Windows Server 2016 foi aprimorada substancialmente, mantendo a compatibilidade de NTP completa com versões mais antigas do Windows.
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 1399ed6a50085baa37f06c09b8c3e18ca8bca98b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395709"
---
# <a name="accurate-time-for-windows-server-2016"></a>Tempo preciso para o Windows Server 2016

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou posterior

O serviço de tempo do Windows é um componente que usa um modelo de plug-in para provedores de sincronização de cliente e servidor.  Há dois provedores de cliente internos no Windows e há plug-ins de terceiros disponíveis. Um provedor usa [NTP (RFC 1305)](https://tools.ietf.org/html/rfc1305) ou [MS-NTP](https://msdn.microsoft.com/library/cc246877.aspx) para sincronizar a hora do sistema local com um servidor de referência compatível com NTP e/ou MS-NTP. O outro provedor é para Hyper-V e sincroniza máquinas virtuais (VM) para o host do Hyper-V.  Quando houver vários provedores, o Windows escolherá o melhor provedor usando o nível de estrato primeiro, seguido por atraso raiz, dispersão de raiz e, por fim, o deslocamento de tempo.

> [!NOTE]
> Para obter uma visão geral rápida do serviço de tempo do Windows, veja este [vídeo de visão geral de alto nível](https://aka.ms/WS2016TimeVideo).

Neste tópico, discutiremos... Estes tópicos estão relacionados para habilitar o tempo preciso: 

- Na
- Medidas
- Práticas recomendadas

> [!IMPORTANT]
> Um adendo referenciado pelo artigo de tempo preciso do Windows 2016 pode ser baixado [aqui](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf).  Este documento fornece mais detalhes sobre nossas metodologias de teste e medição.

> [!NOTE] 
> O modelo de plug-in do provedor de tempo do Windows está [documentado no TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

## <a name="domain-hierarchy"></a>Hierarquia de domínio
Configurações autônomas e de domínio funcionam de maneira diferente.

- Os membros do domínio usam um protocolo NTP seguro, que usa a autenticação para garantir a segurança e a autenticidade da referência de tempo.  Os membros do domínio sincronizam com um relógio mestre determinado pela hierarquia de domínio e um sistema de pontuação.  Em um domínio, há uma camada hierárquica de imposições de tempo, na qual cada DC aponta para um controlador de domínio pai com um estrato de tempo mais preciso.  A hierarquia é resolvida para o PDC ou um DC na floresta raiz ou um DC com o sinalizador de domínio GTIMESERV, que denota um bom servidor de horário para o domínio.  Consulte a seção [especificar um serviço de horário confiável local usando o GTIMESERV](#GTIMESERV) abaixo.

- Os computadores autônomos são configurados para usar o time.windows.com por padrão.  Esse nome é resolvido pelo servidor DNS, que deve apontar para um recurso de propriedade da Microsoft.  Como todas as referências de horário localizadas remotamente, as interrupções de rede podem impedir a sincronização.  Cargas de tráfego de rede e caminhos de rede assimétrica podem reduzir a precisão da sincronização de tempo.  Para uma precisão de 1 ms, você não pode depender de fontes de tempo remotas.

Como os convidados do Hyper-V terão pelo menos dois provedores de tempo do Windows para escolher, o tempo de host e o NTP, você poderá ver comportamentos diferentes com o domínio ou autônomo ao executar como convidado.

> [!NOTE] 
> Para obter mais informações sobre a hierarquia de domínio e o sistema de pontuação, consulte o ["o que é o serviço de tempo do Windows?"](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) .

> [!NOTE]
> O estrato é um conceito usado nos provedores NTP e Hyper-V, e seu valor indica o local dos relógios na hierarquia.  O estrato 1 é reservado para o clock de nível mais alto e o estrato 0 é reservado para que o hardware tenha sido considerado preciso e tenha pouco ou nenhum atraso associado a ele.  O estrato 2 conversa com servidores de estrato 1, o estrato 3 para o estrato 2 e assim por diante.  Embora uma camada mais baixa geralmente indique um relógio mais preciso, é possível encontrar discrepâncias.  Além disso, o W32time aceita apenas o tempo do estrato 15 ou abaixo.  Para ver o estrato de um cliente, use *w32tm/Query/status*.

## <a name="critical-factors-for-accurate-time"></a>Fatores críticos para o tempo preciso
Em cada caso, para um tempo preciso, há três fatores críticos:

1. **Relógio de origem sólido** -o relógio de origem em seu domínio precisa ser estável e preciso. Isso geralmente significa instalar um dispositivo GPS ou apontar para uma origem de estrato 1, levando #3 em conta. A analogia vai, se você tiver duas Boats na água e estiver tentando medir a altitude de uma em comparação com a outra, sua precisão será melhor se o barco de origem for muito estável e não estiver se movendo. O mesmo acontece com o tempo e, se o relógio de origem não estiver estável, a cadeia inteira de relógios sincronizados será afetada e ampliada em cada estágio. Ele também deve estar acessível porque as interrupções na conexão interferirão na sincronização de tempo. E, finalmente, deve ser seguro. Se a referência de tempo não for mantida corretamente ou operada por uma parte potencialmente mal-intencionada, você poderá expor seu domínio a ataques baseados em tempo.
2. **Relógio de cliente estável** -um relógio de cliente estável garante que a descompasso natural do Oscillator seja confinada.  O NTP usa várias amostras de potencialmente vários servidores NTP para condição e disciplina do relógio de seus computadores locais.  Ele não aborda as alterações de horário, mas, em vez disso, reduz ou acelera o relógio local para que você se aproximar do tempo exato rapidamente e fique preciso entre as solicitações de NTP.  No entanto, se o Oscillator do relógio do computador cliente não for estável, mais flutuações entre os ajustes poderão ocorrer e os algoritmos que o Windows usa para a condição de que o relógio não funcione com precisão.  Em alguns casos, as atualizações de firmware podem ser necessárias para o tempo preciso.
3. **Comunicação de NTP simétrico** – é essencial que a conexão para comunicação de NTP seja simétrica.  O NTP usa cálculos para ajustar o tempo que pressupõe que o patch de rede seja simétrico.  Se o caminho que o pacote NTP leva para o servidor levar um período de tempo diferente para retornar, a precisão será afetada.  Por exemplo, o caminho pode ser alterado devido a alterações na topologia de rede, ou os pacotes que estão sendo roteados por meio de dispositivos com velocidades de interface diferentes.

Para dispositivos com bateria, móveis e portáteis, você deve considerar estratégias diferentes.  De acordo com nossa recomendação, manter o tempo preciso exige que o relógio seja disciplinado uma vez por segundo, que se correlaciona com a frequência de atualização do relógio. Essas configurações consumirão mais energia da bateria do que o esperado e podem interferir nos modos de economia de energia disponíveis no Windows para tais dispositivos. Os dispositivos com bateria também têm determinados modos de energia que interrompem a execução de todos os aplicativos, o que interfere na capacidade de W32time's de disciplinar o relógio e manter o tempo preciso. Além disso, os relógios em dispositivos móveis podem não ser muito precisos para começar.  As condições ambientais de ambiente afetam a precisão do relógio e um dispositivo móvel pode passar de uma condição de ambiente para a seguinte, o que pode interferir em sua capacidade de manter o tempo preciso.  Portanto, a Microsoft não recomenda que você configure dispositivos portáteis alimentados pela bateria com configurações de alta precisão. 

## <a name="why-is-time-important"></a>Por que o tempo é importante?  
Há muitas razões diferentes pelas quais você pode precisar de tempo preciso.  O caso típico do Windows é o Kerberos, que exige 5 minutos de precisão entre o cliente e o servidor.  No entanto, há muitas outras áreas que podem ser afetadas pela precisão do tempo, incluindo:


- Regulamentos do governo como:
    - 50 ms de precisão para FINRA nos EUA
    - 1 MS ESMA (MiFID II) na UE.
- Algoritmos de criptografia
- Sistemas distribuídos, como cluster/SQL/Exchange e bancos de documentos
- Estrutura Blockchain para transações Bitcoin
- Logs distribuídos e análise de ameaças 
- Replicação do AD
- PCI (setor de cartão de pagamento), precisão de 1 segundo no momento



[!INCLUDE [windows-server-2016-improvements](windows-server-2016-improvements.md)]
