---
ms.assetid: ''
title: Configurando os sistemas de alta precisão
description: Sincronização de hora no Windows 10 e Windows Server 2016 foi consideravelmente aprimorada.  Em condições de operação razoáveis, os sistemas podem ser configurados para manter a 1 ms (milissegundos) melhor (em relação ao UTC) ou precisão.
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: d872252180d49bd41a91e9a8eaf516b834ed242a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814667"
---
# <a name="configuring-systems-for-high-accuracy"></a>Configurando os sistemas de alta precisão
>Aplica-se a: Windows Server 2016 e Windows 10 versão 1607 ou posterior

Sincronização de hora no Windows 10 e Windows Server 2016 foi consideravelmente aprimorada.  Em condições de operação razoáveis, os sistemas podem ser configurados para manter a 1 ms (milissegundos) melhor (em relação ao UTC) ou precisão.

As diretrizes a seguir ajudará você a configurar seus sistemas para alcançar alta precisão.  Este artigo discute os seguintes requisitos:

- Sistemas operacionais com suporte
- Configuração do sistema 

> [!WARNING]
> **Metas de precisão em sistemas operacionais anteriores**<br>
>Windows Server 2012 R2 e abaixo podem não atender os mesmos objetivos de alta precisão. Não há suporte para esses sistemas operacionais de alta precisão.
>
>Nessas versões, o serviço de tempo do Windows atendido os seguintes requisitos:
>
> - Fornecida a precisão de tempo necessário para atender aos requisitos de autenticação Kerberos versão 5.
> - Fornecido tempo preciso livremente para clientes do Windows e servidores associados a uma floresta do Active Directory comuns.
>
>Tolerâncias maior no 2012 R2 e anterior estão fora de especificação de design do serviço de tempo do Windows.

## <a name="windows-10-and-windows-server-2016-default-configuration"></a>Windows 10 e a configuração padrão do Windows Server 2016

Enquanto damos suporte a precisão de até 1 ms no Windows 10 ou Windows Server 2016, a maioria dos clientes não exigem tempo altamente preciso.

Como tal, o **configuração padrão** destinados a satisfazer os mesmos requisitos que em sistemas operacionais anteriores que devem:

- Forneça a precisão de tempo necessário para atender aos requisitos de autenticação Kerberos versão 5.
- Fornece tempo preciso livremente para clientes do Windows e servidores associados a uma floresta do Active Directory comuns.

## <a name="how-to-configure-systems-for-high-accuracy"></a>Como configurar sistemas de alta precisão

>[!IMPORTANT]
>**Nota sobre a capacidade de suporte de sistemas altamente precisos**<br>
> A distribuição de ponta a ponta de tempo preciso da fonte de horário autoritativo para o dispositivo final envolve a precisão de tempo.  Tudo o que adiciona assymetry em medidas ao longo desse caminho negativamente influenciarão precisão afetará a precisão que pode ser obtida em seus dispositivos.
>
>Por esse motivo, documentamos a [limite de suporte para configurar o serviço de tempo do Windows para ambientes de alta precisão](support-boundary.md) as necessidades do ambiente que também devem ser atendidas para alcançar metas de alta precisão de estrutura de tópicos.

### <a name="operating-system-requirements"></a>Requisitos do Sistema Operacional

Configurações de alta precisão exigem o Windows 10 ou Windows Server 2016.  Todos os dispositivos do Windows na topologia de tempo devem atender a esse requisito, incluindo servidores de tempo do Windows stratum maior e em virtualizado cenários, os Hosts do Hyper-V que executam as máquinas virtuais de detecção de hora. Todos esses dispositivos devem ser pelo menos o Windows 10 ou Windows Server 2016.

Na ilustração abaixo, as máquinas virtuais que exigem alta precisão estão executando o Windows 10 ou Windows Server 2016.  Da mesma forma, o Host Hyper-V no qual residem as máquinas virtuais e o servidor upstream de tempo do Windows também deve executar o Windows Server 2016.

![Topologia de tempo - 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/Topology2016.png)


>[!TIP] 
>**Determinando a versão do Windows**<br>
> Você pode executar o comando `winver` em um prompt de comando para verificar se o sistema operacional a versão é 1607 (ou superior) e Build do sistema operacional é 14393 (ou superior) conforme mostrado abaixo:
>
> ![Winver - 2016 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/winver2016.png)

### <a name="system-configuration"></a>Configuração do sistema

Alcançar as metas de alta precisão requer a configuração do sistema.  Há várias maneiras para executar essa configuração, incluindo diretamente no registro ou por meio da diretiva de grupo.  Para obter mais informações para cada uma dessas configurações podem ser encontradas na referência técnica do serviço de tempo Windows – [ferramentas de serviço de tempo do Windows](Windows-Time-Service-Tools-and-Settings.md#windows-time-service-tools).

#### <a name="windows-time-service-startup-type"></a>Tipo de inicialização do serviço de tempo do Windows

O serviço de tempo do Windows (W32Time) deve ser executado continuamente.  Para fazer isso, configure o tipo de inicialização do serviço de tempo do Windows para iniciar 'Automático'.

![Configuração Automática](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/AutomaticService.PNG)

#### <a name="cumulative-one-way-network-latency"></a>Latência de rede unidirecional cumulativa

Incerteza de medição e o "ruído" surgir como aumentos de latência de rede.  Como tal, é imperativo que uma latência de rede seja dentro de um limite razoável.  Os requisitos específicos dependem de sua precisão de destino e são descritos na [limite de suporte para configurar o serviço de tempo do Windows para ambientes de alta precisão](support-boundary.md) artigo.

Para calcular a latência de rede unidirecional cumulativa, adicione os atrasos unidirecionais individuais entre pares de nós de cliente-servidor NTP na topologia do tempo, começando com o destino e terminando com a camada de alta precisão 1 fonte de tempo.

Por exemplo:  Considere uma hierarquia de tempo de sincronização com uma fonte altamente precisa, dois servidores NTP intermediários A e B e o computador de destino, nessa ordem. Para obter a latência de rede cumulativa entre a origem e destino, medir a média de ida e volta NTP individual (RTTs) o tempo entre:

- O servidor de destino e a hora em B
- O servidor de horário B e um servidor de horário
- Um servidor de horário e o código-fonte

Essa medida pode ser obtida usando a ferramenta de w32tm.exe da caixa de entrada.  Para fazer isso:
<!-- Use PowerShell to import the CSV then average the RTT Column -->

1. Executar o cálculo do servidor de destino e a hora em B.
    
    `w32tm /stripchart /computer:TimeServerB /rdtsc /samples:450 > c:\temp\Target_TsB.csv`

2. Executar o cálculo do tempo o servidor b contra (apontado) servidor de horário um.
    
    `w32tm /stripchart /computer:TimeServerA /rdtsc /samples:450 > c:\temp\Target_TsA.csv`

3. Executar o cálculo do servidor de horário uma em relação à fonte.
 
4. Em seguida, adicione que o RoundTripDelay médio medido na etapa anterior e divida por 2 para obter o atraso de rede cumulativa entre origem e destino.

#### <a name="registry-settings"></a>Configurações do Registro

# <a name="minpollintervaltabminpollinterval"></a>[MinPollInterval](#tab/MinPollInterval)
Configura o menor intervalo em segundos de log2 permitido para a consulta do sistema.

|  |  | 
|---------|---------|
|Local de chave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Configuração    | 6        |
|Resultado | Agora, o intervalo de sondagem mínimo é 64 segundos. |

O comando a seguir indica o tempo do Windows para acompanhar as configurações atualizadas:

`w32tm /config /update`


# <a name="maxpollintervaltabmaxpollinterval"></a>[MaxPollInterval](#tab/MaxPollInterval)
Configura o maior intervalo em segundos de log2 permitido para a consulta do sistema.

|  |  |  
|---------|---------|
|Local de chave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Configuração    | 6        |
|Resultado | Agora, o intervalo de sondagem máximo é 64 segundos.  |

O comando a seguir indica o tempo do Windows para acompanhar as configurações atualizadas:

`w32tm /config /update`

# <a name="updateintervaltabupdateinterval"></a>[UpdateInterval](#tab/UpdateInterval)
O número de tiques do relógio entre os ajustes de correção de fase.

|  |  |  
|---------|---------|
|Local de chave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config       |
|Configuração    | 100        |
|Resultado | O número de tiques do relógio entre os ajustes de correção de fase agora é tiques de 100. |

O comando a seguir indica o tempo do Windows para acompanhar as configurações atualizadas:

`w32tm /config /update`

# <a name="specialpollintervaltabspecialpollinterval"></a>[SpecialPollInterval](#tab/SpecialPollInterval)
Configura o intervalo de sondagem em segundos quando o sinalizador SpecialInterval 0x1 está habilitado.

|  |  |  
|---------|---------|
|Local de chave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient        |
|Configuração    | 64        |
|Resultado | Agora, o intervalo de sondagem é 64 segundos. |

O comando a seguir reinicia a hora do Windows para acompanhar as configurações atualizadas:

`net stop w32time && net start w32time`

# <a name="frequencycorrectratetabfrequencycorrectrate"></a>[FrequencyCorrectRate](#tab/FrequencyCorrectRate)

|  |  |  
|---------|---------|
|Local de chave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config      |
|Configuração    | 2        |


---
