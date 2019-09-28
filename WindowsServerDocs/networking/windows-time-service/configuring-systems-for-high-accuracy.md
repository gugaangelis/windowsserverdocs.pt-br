---
ms.assetid: ''
title: Configurando sistemas para alta precisão
description: A sincronização de tempo no Windows 10 e no Windows Server 2016 foi substancialmente aprimorada.  Em condições operacionais razoáveis, os sistemas podem ser configurados para manter a precisão de 1 ms (milissegundo) ou melhor (em relação ao UTC).
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: b7cd256fdbbdbe7432e5b5d5b16254314132560f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405201"
---
# <a name="configuring-systems-for-high-accuracy"></a>Configurando sistemas para alta precisão
>Aplica-se a: Windows Server 2016 e Windows 10 versão 1607 ou posterior

A sincronização de tempo no Windows 10 e no Windows Server 2016 foi substancialmente aprimorada.  Em condições operacionais razoáveis, os sistemas podem ser configurados para manter a precisão de 1 ms (milissegundo) ou melhor (em relação ao UTC).

As diretrizes a seguir ajudarão você a configurar seus sistemas para obter alta precisão.  Este artigo aborda os seguintes requisitos:

- Sistemas operacionais com suporte
- Configuração do sistema 

> [!WARNING]
> **Metas de exatidão dos sistemas operacionais anteriores**<br>
>O Windows Server 2012 R2 e inferior não podem atender aos mesmos objetivos de alta precisão. Esses sistemas operacionais não têm suporte para alta precisão.
>
>Nessas versões, o serviço de tempo do Windows satisfez os seguintes requisitos:
>
> - Forneceu a precisão de tempo necessária para atender aos requisitos de autenticação do Kerberos versão 5.
> - Fornecido um tempo menos preciso para clientes e servidores Windows que ingressaram em uma floresta Active Directory comum.
>
>Tolerâncias maiores no 2012 R2 e abaixo estão fora da especificação de design do serviço de tempo do Windows.

## <a name="windows-10-and-windows-server-2016-default-configuration"></a>Configuração padrão do Windows 10 e do Windows Server 2016

Embora ofereçamos suporte à precisão de até 1 ms no Windows 10 ou no Windows Server 2016, a maioria dos clientes não exige tempo altamente preciso.

Dessa forma, a **configuração padrão** destina-se a atender aos mesmos requisitos dos sistemas operacionais anteriores que são:

- Forneça a precisão de tempo necessária para atender aos requisitos de autenticação do Kerberos versão 5.
- Forneça tempo muito preciso para clientes e servidores Windows ingressados em uma floresta Active Directory comum.

## <a name="how-to-configure-systems-for-high-accuracy"></a>Como configurar sistemas para alta precisão

>[!IMPORTANT]
>**Observação sobre a compatibilidade de sistemas altamente precisos**<br>
> A precisão do tempo envolve a distribuição de ponta a ponta de tempo preciso da fonte de tempo autoritativa para o dispositivo final.  Qualquer coisa que adiciona assymetry em medidas ao longo desse caminho influenciará negativamente a precisão afetará a precisão obtida em seus dispositivos.
>
>Por esse motivo, documentamos o [limite de suporte para configurar o serviço de tempo do Windows para ambientes de alta precisão](support-boundary.md) , descrevendo os requisitos ambientais que também devem ser satisfeitos para alcançar metas de alta precisão.

### <a name="operating-system-requirements"></a>Requisitos do Sistema Operacional

As configurações de alta precisão exigem o Windows 10 ou o Windows Server 2016.  Todos os dispositivos Windows na topologia de tempo devem atender a esse requisito, incluindo servidores de tempo Windows de estrato mais altos e em cenários virtualizados, os hosts Hyper-V que executam as máquinas virtuais sensíveis ao tempo. Todos esses dispositivos devem ser pelo menos Windows 10 ou Windows Server 2016.

Na ilustração mostrada abaixo, as máquinas virtuais que exigem alta precisão estão executando o Windows 10 ou o Windows Server 2016.  Da mesma forma, o host Hyper-V no qual as máquinas virtuais residem e o servidor de horário do Windows upstream também deve executar o Windows Server 2016.

![Topologia de tempo-1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/Topology2016.png)


>[!TIP] 
>**Determinando a versão do Windows**<br>
> Você pode executar o comando `winver` em um prompt de comando para verificar se a versão do sistema operacional é 1607 (ou superior) e se a compilação do sistema operacional é 14393 (ou superior), conforme mostrado abaixo:
>
> ![Winver-2016 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/winver2016.png)

### <a name="system-configuration"></a>Configuração do sistema

Atingir destinos de alta precisão requer configuração do sistema.  Há várias maneiras de executar essa configuração, incluindo diretamente no registro ou por meio da diretiva de grupo.  Mais informações sobre cada uma dessas configurações podem ser encontradas na referência técnica do serviço de tempo do Windows – [ferramentas de serviço de tempo do Windows](Windows-Time-Service-Tools-and-Settings.md#windows-time-service-tools).

#### <a name="windows-time-service-startup-type"></a>Tipo de inicialização do serviço de tempo do Windows

O serviço de tempo do Windows (W32Time) deve ser executado continuamente.  Para fazer isso, configure o tipo de inicialização do serviço de tempo do Windows para ' automático ' Iniciar.

![Configuração Automática](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/AutomaticService.PNG)

#### <a name="cumulative-one-way-network-latency"></a>Latência de rede unidirecional cumulativa

A incerteza de medida e o "ruído" aumenta à medida que a latência de rede cresce.  Como tal, é imperativo que uma latência de rede esteja dentro de um limite razoável.  Os requisitos específicos dependem da precisão de destino e são descritos no artigo [limite de suporte para configurar o serviço de tempo do Windows para ambientes de alta precisão](support-boundary.md) .

Para calcular a latência de rede unidirecional cumulativa, adicione os atrasos individuais unidirecionais entre pares de nós cliente-servidor NTP na topologia de tempo, começando com o destino e terminando com a fonte de tempo de estrato de alta precisão.

Por exemplo: Considere uma hierarquia de sincronização de tempo com uma fonte altamente precisa, dois servidores NTP intermediários A e B e o computador de destino nessa ordem. Para obter a latência de rede cumulativa entre o destino e a origem, meça a média de tempos de ida e volta de NTP individuais (RTTs) entre:

- O destino e o servidor de horário B
- Hora servidor B e servidor de horário A
- Hora a e a origem

Essa medida pode ser obtida usando a ferramenta inbox. exe da caixa de entrada.  Para fazer isso:

1. Execute o cálculo do destino e do servidor de horário B.
    
    `w32tm /stripchart /computer:TimeServerB /rdtsc /samples:450 > c:\temp\Target_TsB.csv`

2. Realize o cálculo do servidor de horário b em relação ao servidor de horário a (apontado) a.
    
    `w32tm /stripchart /computer:TimeServerA /rdtsc /samples:450 > c:\temp\Target_TsA.csv`

3. Faça o cálculo do servidor de horário a em relação à origem.
 
4. Em seguida, adicione o RoundTripDelay médio medido na etapa anterior e divida por 2 para obter o atraso de rede cumulativo entre o destino e a origem.

#### <a name="registry-settings"></a>Configurações do Registro

# <a name="minpollintervaltabminpollinterval"></a>[MinPollInterval](#tab/MinPollInterval)
Configura o menor intervalo em log2 segundos permitido para sondagem do sistema.

|  |  | 
|---------|---------|
|Local da chave     | HKLM: \ SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Configuração    | 6        |
|Resultado | O intervalo de sondagem mínimo agora é de 64 segundos. |

O comando a seguir sinaliza o tempo do Windows para selecionar as configurações atualizadas:

`w32tm /config /update`


# <a name="maxpollintervaltabmaxpollinterval"></a>[MaxPollInterval](#tab/MaxPollInterval)
Configura o maior intervalo em log2 segundos permitido para sondagem do sistema.

|  |  |  
|---------|---------|
|Local da chave     | HKLM: \ SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Configuração    | 6        |
|Resultado | O intervalo de sondagem máximo agora é de 64 segundos.  |

O comando a seguir sinaliza o tempo do Windows para selecionar as configurações atualizadas:

`w32tm /config /update`

# <a name="updateintervaltabupdateinterval"></a>[UpdateInterval](#tab/UpdateInterval)
O número de tiques de relógio entre os ajustes de correção de fase.

|  |  |  
|---------|---------|
|Local da chave     | HKLM: \ SYSTEM\CurrentControlSet\Services\W32Time\Config       |
|Configuração    | 100        |
|Resultado | O número de tiques de relógio entre ajustes de correção de fase agora é de 100 tiques. |

O comando a seguir sinaliza o tempo do Windows para selecionar as configurações atualizadas:

`w32tm /config /update`

# <a name="specialpollintervaltabspecialpollinterval"></a>[SpecialPollInterval](#tab/SpecialPollInterval)
Configura o intervalo de sondagem em segundos quando o sinalizador SpecialInterval 0x1 está habilitado.

|  |  |  
|---------|---------|
|Local da chave     | HKLM: \ SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient        |
|Configuração    | 64        |
|Resultado | O intervalo de sondagem agora é de 64 segundos. |

O comando a seguir reinicia o tempo do Windows para selecionar as configurações atualizadas:

`net stop w32time && net start w32time`

# <a name="frequencycorrectratetabfrequencycorrectrate"></a>[FrequencyCorrectRate](#tab/FrequencyCorrectRate)

|  |  |  
|---------|---------|
|Local da chave     | HKLM: \ SYSTEM\CurrentControlSet\Services\W32Time\Config      |
|Configuração    | 2        |


---
