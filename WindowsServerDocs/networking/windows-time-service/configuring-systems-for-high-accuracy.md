---
ms.assetid: ''
title: Configurar sistemas para oferecer alta precisão
description: A sincronização de tempo no Windows 10 e no Windows Server 2016 foi substancialmente aprimorada.  Em condições operacionais razoáveis, os sistemas podem ser configurados para manter a precisão de 1 ms (milissegundo) ou melhor (com respeito ao UTC).
author: dcuomo
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 71f15f9da1f477ec8632fd2eb900e650f83ef3de
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87409596"
---
# <a name="configuring-systems-for-high-accuracy"></a>Configurar sistemas para oferecer alta precisão
>Aplica-se a: Windows Server 2016 e Windows 10 versão 1607 ou posterior

A sincronização de tempo no Windows 10 e no Windows Server 2016 foi substancialmente aprimorada.  Em condições operacionais razoáveis, os sistemas podem ser configurados para manter a precisão de 1 ms (milissegundo) ou melhor (com respeito ao UTC).

As diretrizes a seguir ajudarão você a configurar seus sistemas para obter alta precisão.  Este artigo aborda os seguintes requisitos:

- Sistemas operacionais com suporte
- Configuração do sistema

> [!WARNING]
> **Metas de precisão anteriores dos sistemas operacionais**<br>
>O Windows Server 2012 R2 e versões inferiores não podem atender aos mesmos objetivos de alta precisão. Esses sistemas operacionais não têm suporte para alta precisão.
>
>Nessas versões, o Serviço de Horário do Windows atendeu aos seguintes requisitos:
>
> - Forneceu a precisão de tempo necessária para atender aos requisitos de autenticação do Kerberos versão 5.
> - Forneceu um tempo menos preciso para clientes e servidores Windows ingressados em uma floresta do Active Directory comum.
>
>Tolerâncias maiores no 2012 R2 e abaixo estão fora da especificação de design do Serviço de Horário do Windows.

## <a name="windows-10-and-windows-server-2016-default-configuration"></a>Configuração Padrão do Windows 10 e do Windows Server 2016

Embora demos suporte à precisão de até 1 ms no Windows 10 ou no Windows Server 2016, a maioria dos clientes não exige tempo altamente preciso.

Assim, a **configuração padrão** destina-se a atender aos mesmos requisitos dos sistemas operacionais anteriores que são:

- Fornecer a precisão de tempo necessária para atender aos requisitos de autenticação do Kerberos versão 5.
- Fornecer um tempo menos preciso para clientes e servidores Windows ingressados em uma floresta do Active Directory comum.

## <a name="how-to-configure-systems-for-high-accuracy"></a>Como configurar sistemas para oferecer alta precisão

>[!IMPORTANT]
>**Observação sobre a compatibilidade de sistemas altamente precisos**<br>
> A precisão do tempo envolve a distribuição de ponta a ponta de tempo preciso de uma fonte de horário autoritativo para o dispositivo final.  Qualquer coisa que adiciona assimetria em medições ao longo desse caminho influenciará negativamente a precisão que pode ser obtida em seus dispositivos.
>
>Por esse motivo, documentamos o [Limite de suporte para configurar o Serviço de Horário do Windows para ambientes de alta precisão](support-boundary.md) descrevendo os requisitos ambientais que também devem ser atendidos para alcançar metas de alta precisão.

### <a name="operating-system-requirements"></a>Requisitos do Sistema Operacional

As configurações de alta precisão exigem o Windows 10 ou o Windows Server 2016.  Todos os dispositivos Windows na topologia de tempo devem atender a esse requisito, incluindo servidores de horário do Windows de estrato mais alto e, em cenários virtualizados, os hosts Hyper-V que executam as máquinas virtuais sensíveis ao tempo. Todos esses dispositivos devem ser pelo menos Windows 10 ou Windows Server 2016.

Na ilustração mostrada abaixo, as máquinas virtuais que exigem alta precisão estão executando o Windows 10 ou o Windows Server 2016.  Da mesma forma, o host Hyper-V no qual as máquinas virtuais residem e o servidor de horário do Windows upstream também devem executar o Windows Server 2016.

![Topologia do tempo – 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/Topology2016.png)

>[!TIP]
>**Determinar a versão do Windows**<br>
> Você pode executar o comando `winver` em um prompt de comando para verificar se a versão do sistema operacional é 1607 (ou superior) e se o build do sistema operacional é 14393 (ou superior) conforme mostrado abaixo:
>
> ![Winver – 2016 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/winver2016.png)

### <a name="system-configuration"></a>Configuração do sistema

Atingir destinos de alta precisão requer a configuração do sistema.  Há várias maneiras de executar essa configuração, incluindo diretamente no Registro ou por meio da política de grupo.  Mais informações para cada uma dessas configurações podem ser encontradas na Referência Técnica do Serviço de Horário do Windows – [Ferramentas de Serviço de Horário do Windows](Windows-Time-Service-Tools-and-Settings.md#windows-time-service-tools).

#### <a name="windows-time-service-startup-type"></a>Tipo de Inicialização do Serviço de Horário do Windows

O Serviço de Horário do Windows (W32Time) deve ser executado continuamente.  Para fazer isso, configure o tipo de inicialização do Serviço de Horário do Windows como início “Automático”.

![Configuração Automática](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/AutomaticService.PNG)

#### <a name="cumulative-one-way-network-latency"></a>Latência de rede unidirecional cumulativa

A incerteza da medição e o “ruído” aparecem conforme a latência de rede aumenta.  Assim, é imperativo que uma latência de rede esteja dentro de um limite razoável.  Os requisitos específicos dependem de sua precisão de destino e são descritos no artigo [Limite de suporte para configurar o Serviço de Horário do Windows para ambientes de alta precisão](support-boundary.md).

Para calcular a latência de rede unidirecional cumulativa, adicione os atrasos unidirecionais individuais entre pares dos nós cliente-servidor NTP na topologia de tempo, começando com o destino e terminando na fonte de horário de estrato 1 de alta precisão.

Por exemplo: Considere uma hierarquia de sincronização de tempo com uma fonte altamente precisa, dois servidores NTP intermediários A e B e o computador de destino nessa ordem. Para obter a latência de rede cumulativa entre o destino e a origem, meça os RTTs (tempos de ida e volta) médios NTP individuais entre:

- O destino e o servidor de horário B
- O servidor B e o servidor de horário A
- O servidor de horário A e a fonte

Essa medida pode ser obtida usando a ferramenta w32tm.exe da caixa de entrada.  Para fazer isso:

1. Execute o cálculo do destino e do servidor de horário B.

    `w32tm /stripchart /computer:TimeServerB /rdtsc /samples:450 > c:\temp\Target_TsB.csv`

2. Realize o cálculo do servidor de horário B em relação ao (apontado no) servidor de horário A.

    `w32tm /stripchart /computer:TimeServerA /rdtsc /samples:450 > c:\temp\Target_TsA.csv`

3. Realize o cálculo do servidor de horário A em relação à origem.

4. Em seguida, adicione o RoundTripDelay médio medido na etapa anterior e divida por 2 para obter o atraso de rede cumulativo entre o destino e a origem.

## <a name="registry-settings"></a>Configurações do Registro

### <a name="minpollinterval"></a>MinPollInterval

Configura o menor intervalo em log2 segundos permitido para sondagem do sistema.

| Descrição | Valor |
|--|--|
| Localização principal | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config |
| Configuração | 6 |
| Resultado | O intervalo de sondagem mínimo agora é de 64 segundos. |

O seguinte comando sinaliza o Horário do Windows para selecionar as configurações atualizadas: `w32tm /config /update`

### <a name="maxpollinterval"></a>MaxPollInterval

Configura o maior intervalo em log2 segundos permitido para sondagem do sistema.

| Descrição | Valor |
|--|--|
| Localização principal | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config |
| Configuração | 6 |
| Resultado | O intervalo de sondagem máximo agora é de 64 segundos. |

O seguinte comando sinaliza o Horário do Windows para selecionar as configurações atualizadas: `w32tm /config /update`

### <a name="updateinterval"></a>UpdateInterval

O número de tiques de relógio entre os ajustes de correção de fase.

| Descrição | Valor |
|--|--|
| Localização principal | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config |
| Configuração | 100 |
| Resultado | O número de tiques de relógio entre os ajustes de correção de fase agora é de 100 tiques. |

O seguinte comando sinaliza o Horário do Windows para selecionar as configurações atualizadas: `w32tm /config /update`

### <a name="specialpollinterval"></a>SpecialPollInterval

Configura o intervalo de sondagem em segundos quando o sinalizador SpecialInterval 0x1 está habilitado.

| Descrição | Valor |
|--|--|
| Localização principal | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient |
| Configuração | 64 |
| Resultado | O intervalo de sondagem agora é de 64 segundos. |

O seguinte comando reinicia o Horário do Windows para selecionar as configurações atualizadas: `net stop w32time && net start w32time`

### <a name="frequencycorrectrate"></a>FrequencyCorrectRate

| Descrição | Valor |
|--|--|
| Localização principal | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config |
| Configuração | 2 |
