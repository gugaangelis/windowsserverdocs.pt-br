---
ms.assetid: 6086947f-f9ef-4e18-9f07-6c7c81d7002c
title: Ferramentas e configurações do Serviço de Tempo do Windows
author: Teresa-Motiv
ms.author: v-tea
ms.date: 02/24/2020
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 76ec8a817f0c500380c9bef6fc1ee7eb8dddc105
ms.sourcegitcommit: 319796ec327530c9656ac103b89bd48cc8d373f6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83790567"
---
# <a name="windows-time-service-tools-and-settings"></a>Ferramentas e configurações do Serviço de Tempo do Windows

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou posterior

Neste tópico, você aprenderá sobre as ferramentas e as configurações do Serviço de Horário do Windows (W32Time).  

Caso você deseje sincronizar a hora apenas para um computador cliente ingressado no domínio, confira [Configurar um computador cliente para sincronização automática de hora no domínio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29). Para obter tópicos adicionais sobre como configurar o Serviço de Horário do Windows, confira [Em que local encontrar informações de configuração de Serviço de Horário do Windows](windows-time-service-top.md).  

> [!CAUTION]  
> Você não deve usar o comando **Net time** para configurar nem definir a hora em que o Serviço de Tempo do Windows está em execução.  
>
> Além disso, em computadores mais antigos que executam o Windows XP ou versões anteriores, o comando **Net time /querysntp** exibe o nome de um servidor do protocolo NTP com o qual um computador está configurado para sincronizar, mas esse servidor NTP só é usado quando o cliente de hora do computador está configurado como NTP ou AllSync. Esse comando já foi preterido.  

A maioria dos computadores membros do domínio tem um tipo de cliente de tempo de NT5DS, o que significa que eles sincronizam o tempo com base na hierarquia de domínio. A única exceção típica disso é o controlador de domínio que funciona como o mestre de operações do emulador PDC (controlador de domínio primário) do domínio raiz da floresta. O mestre de operações do emulador PDC geralmente é configurado para sincronizar a hora com uma fonte de hora externa. Para ver a configuração do cliente de hora de um computador (no Windows Server 2008 e no Windows Vista em diante), execute o comando **W32tm /query /configuration** em um prompt de comandos com privilégios elevados e leia a linha **Type** na saída do comando. Para obter mais informações, confira [Como o Serviço de Horário do Windows funciona](How-the-Windows-Time-Service-Works.md). Além disso, execute o comando **reg query HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters** e leia o valor de **NtpServer** na saída do comando.  

> [!IMPORTANT]  
> Antes do Windows Server 2016, o serviço W32Time não era criado para atender a necessidades de aplicativos sensíveis ao tempo. Contudo, agora as atualizações do Windows Server 2016 permitem que você implemente uma solução visando uma precisão de 1 milissegundo no domínio. Para obter mais informações, confira [Hora precisa do Windows 2016](accurate-time.md) e [Limite de suporte para configurar o Serviço de Hora do Windows em ambientes de alta precisão](support-boundary.md).  

## <a name="windows-time-service-tools"></a>Ferramentas do Serviço de Hora do Windows  

A ferramenta a seguir está associada ao Serviço de Hora do Windows.  

### <a name="w32tmexe-windows-time"></a>W32tm.exe: Tempo do Windows  

**Categoria**  

Essa ferramenta faz parte da instalação padrão do Windows (Windows XP e versões posteriores) e do Windows Server (Windows Server 2003 e versões posteriores).  

**Compatibilidade entre versões**  

Essa ferramenta funciona na instalação padrão do Windows (Windows XP e versões posteriores) e do Windows Server (Windows Server 2003 e versões posteriores).  

Use o W32tm.exe para definir as configurações do Serviço de Hora do Windows e diagnosticar problemas do serviço de hora. O W32tm.exe é a ferramenta de linha de comando preferencial para configurar, monitorar ou solucionar problemas do Serviço de Hora do Windows.  

As tabelas a seguir descrevem os parâmetros que podem ser usados com o W32tm.exe.  

**Parâmetros primários do W32tm.exe**  

|Parâmetro |Descrição |
| --- | --- |
|**w32tm /?** |Exibe a ajuda da linha de comando do W32tm |
|**w32tm /register** |Registra o Serviço de Hora para ser executado como um serviço e adiciona as informações de configuração padrão ao Registro. |
|**w32tm /unregister** |Cancela o registro do Serviço de Hora e remove todas as informações de configuração do Registro. |
|**w32tm /monitor [/domain:\<*domain name*>] [/computers:\<*name*>[,\<*name*>[,\<*name*>...]]] [/threads:\<*num*>]** |Monitora o Serviço de Hora do Windows.<p>**/domain**: especifica o domínio a ser monitorado. Se nenhum nome de domínio for especificado ou se a opção **/domain** nem a opção **/computers** for especificada, o domínio padrão será usado. Esta opção pode ser usada mais de uma vez.<p>**/computers**: monitora a lista de computadores fornecida. Os nomes de computador são separados por vírgulas, sem espaços. Se um nome for prefixado com um **\*** , ele será tratado como um PDC. Esta opção pode ser usada mais de uma vez.<p>**/threads**: especifica o número de computadores a serem analisados simultaneamente. O valor padrão é três. O intervalo permitido é de 1 a 50. |
|**w32tm /ntte \<NT *time epoch*>** |Converte uma hora do sistema do Windows NT (medida em intervalos de 10<sup>-7</sup> segundos começando à 0h de 1º de janeiro de 1601) em um formato legível. |
|**w32tm /ntpte \<NTP *time epoch*>** |Converte uma hora do NTP (medida em intervalos de 2<sup>-32</sup> segundos começando à 0h de 1º de janeiro de 1900) em um formato legível. |
|**w32tm /resync [/computer:\<*computer*>] [/nowait] [/rediscover] [/soft]** |Informa a um computador que ele deve ressincronizar seu relógio assim que possível, descartando todas as estatísticas de erro acumuladas.<p>**/computer:\<*computer*>** : especifica o computador que deve ser ressincronizado. Se não for especificado, o computador local será ressincronizado.<p>**/nowait**: não aguardar a ressincronização; retorno imediato. Caso contrário, aguardar a ressincronização ser concluída antes de retornar.<p>**/rediscover**: detecta novamente a configuração de rede e redescobre as fontes de rede e, em seguida, faz a ressincronização.<p>**/soft**: faz a ressincronização usando as estatísticas de erro existentes. Não é útil, fornecido para compatibilidade. |
|**w32tm /stripchart /computer:\<*target*> [/period:\<*refresh*>] [/dataonly] [/samples:\<*count*>] [/rdtsc]** |Exibe um gráfico de faixas da diferença entre este e outro computador.<p>**/computer:\<*target*>** : o computador em relação ao qual a diferença será medida.<p>**/period:\<*refresh*>** : o tempo entre amostras, em segundos. O padrão é dois segundos.<p>**/dataonly**: exibe somente os dados, sem gráficos.<p>**/samples:\<*count*>** : coleta amostras de \<*count*> e, em seguida, interrompe o processo. Se não for especificado, os exemplos serão coletados até que **CTRL+C** seja pressionado.<br/><br/>**/rdtsc**: para cada amostra, essa opção imprime valores separados por vírgula junto com os cabeçalhos **RdtscStart**, **RdtscEnd**, **FileTime**, **RoundtripDelay** e **NtpOffset**, em vez do gráfico de texto.<br/><ul><li>**RdtscStart**: valor do [RDTSC (Contador de Carimbos de Data/Hora de Leitura)](https://en.wikipedia.org/wiki/Time_Stamp_Counter) coletado antes da geração da solicitação NTP.</li><li>**RdtscEnd**: valor do RDTSC coletado logo após o recebimento e o processamento da resposta NTP.</li><li>**FileTime**: valor de FILETIME local usado na solicitação NTP.</li><li>**RoundtripDelay**: tempo decorrido em segundos entre a geração da solicitação NTP e o processamento da resposta NTP recebida, obtido conforme os cálculos de viagem de ida e volta do NTP.</li><li>**NTPOffset**: diferença de horário em segundos entre o computador local e o servidor NTP, obtida conforme os cálculos da diferença do NTP.</li></ul> |
|**w32tm /config [/computer:\<*target*>] [/update] [/manualpeerlist:\<*peers*>] [/syncfromflags:\<*source*>] [/LocalClockDispersion:\<*seconds*>] [/reliable:(YES\|NO)] [/largephaseoffset:\<*milliseconds*>]** |**/computer:\<*target*>** : ajusta a configuração de \<*target*>. Se não for especificada, o padrão será o computador local.<p>**/update**: notifica o Serviço de Hora de que a configuração foi alterada, fazendo com que as alterações entrem em vigor.<p>**/manualpeerlist:\<*peers*>** : define a lista de pares manuais como \<*peers*>, que é uma lista delimitada por espaço de endereços IP e/ou DNS. Ao especificar vários pares, essa opção deve ser colocada entre aspas.<p>**/syncfromflags:\<*source*>** : define de quais fontes o cliente NTP deve fazer a sincronização. \<*source*> deve ser uma lista separada por vírgula destas palavras-chave (não diferencia maiúsculas de minúsculas):<ul><li>**MANUAL**: inclui pares da lista de pares manuais.</li><li>**DOMHIER**: faz a sincronização de um DC (controlador de domínio) na hierarquia de domínio.</li></ul>**/LocalClockDispersion:\<*seconds*>** : configura a precisão do relógio interno que o W32Time presumirá quando não puder adquirir a hora das fontes configuradas.<p>**/reliable:(YES\|NO)** : define se este computador é uma fonte de hora confiável. Essa configuração só é significativa em controladores de domínio.<ul><li>**SIM**: este computador é um serviço de hora confiável.</li><li>**NÃO**: este computador não é um serviço de hora confiável.</li></ul>**/largephaseoffset:\<*milliseconds*>** : define a diferença de tempo entre a hora local e da rede, que o W32Time vai considerar um pico. |
|**w32tm /tz** |Exibir as configurações de fuso horário atuais. |
|**w32tm /dumpreg [/subkey:\<*key*>] [/computer:\<*target*>]** |Exibir os valores associados a uma determinada chave do Registro.<p>A chave padrão é **HKLM\System\CurrentControlSet\Services\W32Time** (a chave raiz do serviço de hora).<p>**/subkey:\<*key*>** : exibe os valores associados à subchave <key> da chave padrão.<p>**/computer:\<*target*>** : consulta as configurações do Registro para o computador \<*target*> |
|**w32tm /query [/computer:\<*target*>] {/source \| /configuration \| /peers \| /status} [/verbose]** |Exibe as informações do Serviço de Hora do Windows de um computador. Esse parâmetro foi disponibilizado pela primeira vez no cliente de Hora do Windows no Windows Vista e no Windows Server 2008.<p>**/computer:\<*target*>** : consulta as informações de \<*target*>. Se não for especificado, o valor padrão será o computador local.<p>**/source**: exibe a fonte de hora.<p>**/configuration**: exibe a configuração da hora de execução e a origem da configuração. No modo detalhado, exibir a configuração indefinida ou não usada também.<p>**/peers**: exibe uma lista de pares e o respectivo status.<p>**/status**: exibe o status do Serviço de Hora do Windows.<p>**/verbose**: define o modo detalhado para exibir mais informações. |
|**w32tm /debug {/disable \| {/enable /file:\<*name*> /size:/<*bytes*> /entries:\<*value*> [/truncate]}}** |habilita ou desabilita o log privado do Serviço de Hora do Windows do computador local. Esse parâmetro foi disponibilizado pela primeira vez no cliente de Hora do Windows no Windows Vista e no Windows Server 2008.<p>**/disable**: desabilita o log privado.<p>**/enable**: habilita o log privado.<ul><li>**file:\<*name*>** : especifica o nome de arquivo absoluto.</li><li>**size:\<*bytes*>** : especifica o tamanho máximo para o log circular.</li><li>**entries:\<*value*>** : contém uma lista de sinalizadores, especificados por número e separados por vírgulas, que especificam os tipos de informações que devem ser registradas em log. Os números válidos são de 0 a 300. Uma variedade de números é válida, além de números únicos, como 0-100.103.106. O valor 0-300 é para registrar em log todas as informações.</li></ul>**/truncate**: trunca o arquivo, se ele existir. |

Para obter mais informações sobre o **W32tm.exe**, confira [Ajuda do Windows](https://support.microsoft.com/hub/4338813/windows-help?os=windows-10).  

**Exemplos**

Caso deseje definir o cliente local de Hora do Windows para apontar para dois servidores de horário diferentes, um chamado ntpserver.contoso.com e outro chamado clock.adatum.com, digite o seguinte comando na linha de comando e, em seguida, selecione ENTER:

```cmd
w32tm /config /manualpeerlist:"ntpserver.contoso.com clock.adatum.com" /syncfromflags:manual /update
```

Para obter uma lista de servidores NTP válidos disponíveis na Internet para sincronização de hora externa, confira [Uma lista de servidores de horário do protocolo SNTP disponíveis na Internet](https://go.microsoft.com/fwlink/?linkid=60401).

Caso deseje verificar a configuração do cliente de Hora do Windows em um computador cliente baseado no Windows que tenha um nome de host CONTOSOW1, execute o seguinte comando:

```cmd
W32tm /query /computer:contosoW1 /configuration
```

O resultado desse comando é uma lista de parâmetros de configuração que são definidos para o cliente de Hora do Windows.

> [!IMPORTANT]  
> [O Windows Server 2016 aprimorou os algoritmos de sincronização de tempo](https://aka.ms/WS2016Time) para se alinhar com as especificações de RFC. Portanto, se você quiser definir o cliente local de Horário do Windows para apontar para vários pares, é altamente recomendável preparar três ou mais servidores de horário diferentes.
>  
> Se você tiver apenas dois servidores de horário, deverá especificar o sinalizador (0x2) **UseAsFallbackOnly** para eliminar a prioridade de um deles. Por exemplo, se você quiser priorizar ntpserver.contoso.com em vez de clock.adatum.com, execute o comando a seguir.
> ```cmd
> w32tm /config /manualpeerlist:"ntpserver.contoso.com,0x8 clock.adatum.com,0xa" /syncfromflags:manual /update
> ```
> Para entender o significado do sinalizador especificado, consulte [Entradas de subchave "HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters"](#parameters).

## <a name="using-group-policy-to-configure-the-windows-time-service"></a>Como usar a Política de Grupo para configurar o Serviço de Hora do Windows

O Serviço de Hora do Windows armazena várias propriedades de configuração como entradas do Registro. Você pode usar Objetos de Política de Grupo para configurar a maior parte dessas informações. Por exemplo, use GPOs para configurar um computador para ser um NTPServer ou um NTPClient, configurar o mecanismo de sincronização de hora e configurar um computador para ser uma fonte de hora confiável.  

> [!NOTE]  
> As configurações da Política de Grupo do Serviço de Hora do Windows podem ser definidas nos controladores de domínio do Windows Server 2003, do Windows Server 2003 R2, do Windows Server 2008 e do Windows Server 2008 R2 e só podem ser aplicadas aos computadores que executam o Windows Server 2003, o Windows Server 2003 R2, o Windows Server 2008 e o Windows Server 2008 R2.  

O Windows armazena as informações de política do Serviço de Hora do Windows no arquivo de modelo administrativo W32Time.admx, em **Configuração do Computador\Modelos Administrativos\Sistema\Serviço de Hora do Windows**. Ele armazena as informações de configuração que as políticas definem no Registro e usa essas entradas do Registro para configurar as entradas do Registro para o Serviço de Hora do Windows. Como resultado, os valores definidos pela Política de Grupo substituem os valores pré-existentes na seção do Serviço de Hora do Windows do Registro.

> [!IMPORTANT]  
> Algumas das configurações predefinidas do GPO são diferentes das entradas do Registro padrão correspondentes. Se planejar usar um GPO para definir qualquer configuração de Horário do Windows, examine os [Valores predefinidos para as configurações de Política de Grupo de Serviço de Horário do Windows são diferentes das entradas do Registro de Serviço de Horário do Windows correspondentes no Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=186066). Essa edição se aplica ao Windows Server 2008 R2, ao Windows Server 2008, ao Windows Server 2003 R2 e ao Windows Server 2003.

Por exemplo, suponha que você edite as configurações de política na política **Configurar o Cliente NTP do Windows**.

As alterações são armazenadas na seguinte localização no modelo administrativo:

> **Configuração do Computador\Modelos Administrativos\Sistema\Serviço de Hora do Windows\Provedores de horário\Configurar o Cliente NTP do Windows**

O Windows carrega essas configurações na área de política do Registro na seguinte subchave:

> **HKLM\Software\Policies\Microsoft\W32time\TimeProviders\NtpClient**

Em seguida, o Windows usa as configurações de política para definir as entradas do Registro do Serviço de Hora do Windows relacionadas na seguinte subchave:

> **HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Time Providers\NTPClient\\**

A tabela a seguir lista as políticas que podem ser configuradas para o Serviço de Hora do Windows e as subchaves do Registro que essas políticas afetam.

> [!NOTE]  
> Quando você remove uma configuração de Política de Grupo, o Windows remove a entrada correspondente da área de política do Registro.

|Política<sup>1</sup> |Localizações do Registro<sup>2,</sup> <sup>3</sup> |
| --- | --- |
|Configurações Globais |W32Time<br />W32Time\Config<br />W32Time\Parameters |
|Time Providers\Configure Windows NTP Client |W32Time\TimeProviders\NtpClient |
|Time Providers\Enable Windows NTP Client |W32Time\TimeProviders\NtpClient |
|Time Providers\Enable Windows NTP Server |W32Time\TimeProviders\NtpServer |

> <sup>1</sup> Caminho da categoria: **Computer Configuration\Administrative Templates\System\Windows Time Service**  
> <sup>2</sup> Subchave: **HKLM\SOFTWARE\Policies\Microsoft**  
> <sup>3</sup> Subchave: **HKLM\SYSTEM\CurrentControlSet\Services**

## <a name="enabling-w32time-logging"></a>Como habilitar o log do W32Time

As três entradas do Registro a seguir não fazem parte da configuração padrão do W32Time, mas podem ser adicionadas ao Registro para obter mais funcionalidades de registro em log. As informações registradas no log de eventos do sistema podem ser modificadas pela alteração do valor da configuração de **EventLogFlags** no Editor de Objeto de Política de Grupo. Por padrão, o serviço de hora registra um evento em log toda vez que ele alterna para uma nova fonte de hora.

Para habilitar o log do W32Time, adicione as seguintes entradas do Registro:  

|Entrada do Registro |Versões |Descrição |
| --- | --- | --- |
|**FileLogEntries** |Todas as versões |Controla o número de entradas criadas no arquivo de log de Hora do Windows. O valor padrão é nenhum, que não registra nenhuma atividade de Horário do Windows. Os valores válidos são de **0** a **300**. Esse valor não afeta as entradas do log de eventos normalmente criadas pelo Horário do Windows |
|**FileLogName** |Todas as versões |Controla a localização e o nome do arquivo de log de Hora do Windows. O valor padrão é em branco e não deve ser alterado, a menos que **FileLogEntries** seja alterado. Um valor válido é um caminho completo e o nome do arquivo que o Horário do Windows usará para criar o arquivo de log. Esse valor não afeta as entradas do log de eventos normalmente criadas pelo Horário do Windows. |
|**FileLogSize** |Todas as versões |Controla o comportamento de log circular dos arquivos de log de Hora do Windows. Quando **FileLogEntries** e **FileLogName** são definidos, a entrada define o tamanho, em bytes, que o arquivo de log pode alcançar antes de substituir as entradas de log mais antigas por entradas novas. Use **1000000** ou um valor maior para essa configuração. Esse valor não afeta as entradas do log de eventos normalmente criadas pelo Horário do Windows. |

## <a name="configuring-how-windows-time-service-resets-the-computer-clock"></a>Como configurar o modo de redefinição do relógio do computador pelo Serviço de Hora do Windows

Para que o W32Time defina o relógio do computador gradualmente, a compensação deve ser menor do que o valor de **MaxAllowedPhaseOffset** e atender à seguinte equação ao mesmo tempo:  

- Windows Server 2016 e versões mais recentes:

  > |**CurrentTimeOffset**| &divide; (16 &times; **PhaseCorrectRate** &times; **pollIntervalInSeconds**) &le; **SystemClockRate** &divide; 2  

- Windows Server 2012 R2 e versões anteriores:

  > |**CurrentTimeOffset**| &divide; (**PhaseCorrectRate** &times; **UpdateInterval**) &le; **SystemClockRate** &divide; 2  

O valor de **CurrentTimeOffset** é medido em tiques do relógio, em que 1 ms = 10.000 tiques do relógio em um sistema do Windows.  

**SystemClockRate** e **PhaseCorrectRate** também são medidos em tiques de relógio. Para obter o valor de **SystemClockRate**, use o seguinte comando e converta-o de segundos em tiques do relógio usando a fórmula *segundos* &times; 1.000 &times; 10.000:  

```cmd
W32tm /query /status /verbose  
ClockRate: 0.0156000s  
```  

**SystemClockRate** é a velocidade do relógio no sistema. Usando 156.000 segundos como exemplo, o valor de **SystemClockRate** será 0,0156000 &times; 1.000 &times; 10.000 = 156.000 tiques do relógio.  

**MaxAllowedPhaseOffset** também é medido em segundos. Para convertê-lo em tiques do relógio, multiplique **MaxAllowedPhaseOffset** &times; 1.000 &times; 10.000.  

Os exemplos a seguir mostram como aplicar esses cálculos quando você usa o Windows Server 2012 R2 ou uma versão anterior.

**Exemplo 1: a hora difere em quatro minutos**

A hora é 11h05 e a amostra de hora que você recebeu de um par e acredita estar correta é 11h09.

> **PhaseCorrectRate** = 1  
>  
> **UpdateInterval** = 30.000 tiques do relógio  
>  
> **SystemClockRate** = 156.000 tiques do relógio  
>  
> **MaxAllowedPhaseOffset** = 10 minutos = 600 segundos = 600 &times; 1.000 &times; 10.000 = 6.000.000.000 tiques do relógio  
>  
> |**CurrentTimeOffset**| = 4 minutos = 4 &times; 60 &times; 1.000 &times; 10.000 = 2.400.000.000 tiques do relógio  
>  
> **CurrentTimeOffset** é &le; **MaxAllowedPhaseOffset**?  
>  
> 2\.400.000.000 &le; 6.000.000.000: TRUE  

AND atende à equação acima?

> (|**CurrentTimeOffset**| &divide; (**PhaseCorrectRate** &times; **UpdateInterval**) &le; **SystemClockRate** &divide; 2)  

2\.400.000.000 / (30.000 &times; 1) é &le; 156.000 &divide; 2?  

80.000 &le; 78.000: FALSE  

Portanto, o W32tm atrasará o relógio imediatamente.  

> [!NOTE]  
> Nesse caso, se você desejar atrasar o relógio lentamente, também precisará ajustar os valores de **PhaseCorrectRate** ou **UpdateInterval** no Registro para garantir que o resultado da equação seja **TRUE**.  

**Exemplo 2: a hora difere em três minutos**

> **PhaseCorrectRate** = 1  
> 
> **UpdateInterval** = 30.000 tiques do relógio  
> 
> SystemClockRate = 156.000 tiques do relógio  
> 
> **MaxAllowedPhaseOffset** = 10 minutos = 600 segundos = 600 &times; 1.000 &times; 10.000 = 6.000.000.000 tiques do relógio  
> 
> |**CurrentTimeOffset**| = 3 minutos = 3 &times; 60 &times; 1.000 &times; 10.000 = 1.800.000.000 tiques do relógio  
> 
> |**CurrentTimeOffset**|  é &le; **MaxAllowedPhaseOffset**?  
> 
> 1\.800.000.000 &le; 6.000.000.000: TRUE  

AND atende à equação acima?

> (|**CurrentTimeOffset**| &divide; (**PhaseCorrectRate** &times; **UpdateInterval**) &le; **SystemClockRate** &divide; 2)  
> 
> 3 minutos &times; (1.800.000.000) &divide; (30.000 &times; 1) é &le; 156.000 &divide; 2?  
> 
> 60.000 é &le; 78.000?: TRUE  

Nesse caso, o relógio será atrasado lentamente.  

## <a name="reference-windows-time-service-registry-entries"></a>Referência: entradas do Registro do Serviço de Hora do Windows

> [!WARNING]  
> As informações sobre essas entradas do Registro são fornecidas como uma referência a ser usada para solucionar problemas ou verificar se as configurações necessárias estão aplicadas. Muitos dos valores na seção W32Time do Registro são usados internamente pelo W32Time para armazenar informações. Não altere esses valores manualmente. As modificações no Registro não são validadas pelo Editor de Registro nem pelo sistema operacional Windows antes de serem aplicadas. Se o Registro contiver valores inválidos, o Windows poderá apresentar erros fatais.  

O Serviço de Hora do Windows armazena informações nas seguintes subchaves do Registro:

- [**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config**](#config)
- [**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters**](#parameters)
- [**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient**](#ntpclient)
- [**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer**](#ntpserver)

Além disso, para fins de solução de problemas, você pode [adicionar entradas para configurar os logs](#enabling-w32time-logging).

Nas tabelas a seguir, "Todas as versões" refere-se às versões do Windows que incluem o Windows 7, o Windows 8, o Windows 10, o Windows Server 2008 e o Windows Server 2008 R2, o Windows Server 2012 e o Windows Server 2012 R2, o Windows Server 2016 e o Windows Server 2019. Algumas entradas só estão disponíveis em versões posteriores do Windows.

> [!NOTE]  
> Alguns dos parâmetros do Registro são medidos em tiques do relógio e outros são medidos em segundos. Para converter a hora de tiques do relógio em segundos, use estes fatores de conversão:  
> - 1 minuto = 60 s  
> - 1 s = 1000 ms  
> - 1 ms = 10.000 tiques do relógio em um sistema Windows, conforme descrito na [Propriedade DateTime.Ticks](https://docs.microsoft.com/dotnet/api/system.datetime.ticks).  
>  
> Por exemplo, 5 minutos passam a ser 5 &times; 60 &times; 1.000 &times; 10.000 = 3.000.000.000 tiques do relógio.  

### <a name="hklmsystemcurrentcontrolsetservicesw32timeconfig-subkey-entries"></a>Entradas da subchave <a id="config"></a>"HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config"

|Entrada de registro |Versões |Descrição |
| --- | --- | --- |
|**AnnounceFlags** |Todas as versões |Controla se este computador está marcado como um servidor de horário confiável. Um computador não será marcado como confiável, a menos que também esteja marcado como um servidor de horário.<ul><li>**0x00**. Não é um servidor de horário</li><li>**0x01**. Sempre um servidor de horário</li><li>**0x02**. Servidor de horário automático</li><li>**0x04**. Servidor de horário sempre confiável</li><li>**0x08**. Servidor de horário confiável automático</li></ul><br />O valor padrão para membros do domínio é **10**. O valor padrão para clientes e servidores autônomos é **10**. |
|**ChainDisable** | |Controla se o mecanismo de encadeamento está desabilitado. Se o encadeamento estiver desabilitado (definido como 0), um RODC (controlador de domínio somente leitura) poderá ser sincronizado com qualquer controlador de domínio, mas os hosts que não tiverem as senhas armazenadas em cache no RODC não poderão ser sincronizados com o RODC. Trata-se de uma configuração booliana, e o valor padrão é **0**.|
|**ChainEntryTimeout** | |Especifica o tempo máximo que uma entrada pode permanecer na tabela de encadeamento antes de a entrada ser considerada como expirada. As entradas expiradas poderão ser removidas quando a próxima solicitação ou resposta for processada. O valor padrão é **16** (segundos). |
|**ChainLoggingRate** | |Controla a frequência com a qual um evento que indica o número de tentativas de encadeamento com e sem êxito é registrado no log do sistema no Visualizador de Eventos. O padrão é **30** (minutos). |
|**ChainMaxEntries** | |Controla o número máximo de entradas permitidas na tabela de encadeamento. Se a tabela de encadeamento estiver cheia e nenhuma entrada expirada puder ser removida, as solicitações de entrada serão descartadas. O valor padrão é **128** (entradas). |
|**ChainMaxHostEntries** | |Controla o número máximo de entradas permitidas na tabela de encadeamento para um host específico. O valor padrão é **4** (entradas). |
|**ClockAdjustmentAuditLimit** |Windows Server 2016 versão 1709 e versões posteriores; Windows 10 versão 1709 e versões posteriores |Especifica os menores ajustes do relógio local que podem ser registrados no log de eventos do serviço W32time no computador de destino. O valor padrão é **800** (PPM – partes por milhão). |
|**ClockHoldoverPeriod** |Windows Server 2016 versão 1709 e versões posteriores; Windows 10 versão 1709 e versões posteriores |Indica o número máximo de segundos que um relógio do sistema pode manter nominalmente a precisão sem sincronizar com uma fonte de hora. Se esse período passar sem o W32time obter novas amostras de um dos provedores de entrada, o W32time iniciará uma redescoberta de fontes de hora. Default: 7.800 segundos. |
|**EventLogFlags** |Todas as versões |Controla os eventos que o serviço de hora registra em log.<ul><li>**0x1**. Salto de tempo</li><li>**0x2**. Alteração da origem</li></ul>O valor padrão em membros do domínio é **2**. O valor padrão em clientes e servidores autônomos é **2**. |
|**FrequencyCorrectRate** |Todas as versões |Controla a velocidade na qual o relógio é corrigido. Se esse valor for muito pequeno, o relógio ficará instável e realizará uma correção excessiva. Se o valor for muito grande, o relógio levará muito tempo para sincronizar. O valor padrão em membros do domínio é **4**. O valor padrão em clientes e servidores autônomos é **4**.<p>**Observação** <br />Zero não é um valor válido para a entrada do Registro **FrequencyCorrectRate**. Em computadores Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2, se o valor for definido como **0**, o Serviço de Hora do Windows vai alterá-lo automaticamente para **1**. |
|**HoldPeriod** |Todas as versões |Controla o período para o qual a detecção de picos é desabilitada, a fim de colocar o relógio local em sincronização rapidamente. Um pico é uma amostra de horário que indica que o horário está incorreto em um número de segundos e, geralmente, é recebido depois que amostras de horário correto são retornadas de maneira consistente. O valor padrão em membros do domínio é **5**. O valor padrão em clientes e servidores autônomos é **5**. |
|**LargePhaseOffset** |Todas as versões |Especifica que uma diferença de horário superior ou igual a esse valor em 10<sup>-7</sup> segundos é considerada um pico. Uma interrupção de rede, como uma grande quantidade de tráfego, pode causar um pico. Um pico será ignorado, a menos que persista por um longo período. O valor padrão em membros do domínio é **50.000.000**. O valor padrão em clientes e servidores autônomos é **50.000.000**. |
|**LastClockRate** |Todas as versões |Mantida pelo W32Time. Contém dados reservados usados pelo sistema operacional Windows. Qualquer alteração nessa configuração pode causar resultados imprevisíveis. O valor padrão em membros do domínio é **156.250**. O valor padrão em clientes e servidores autônomos é **156.250**. |
|**LocalClockDispersion** |Todas as versões |Controla a dispersão (em segundos) que você precisa supor quando a única fonte de hora é o relógio CMOS interno. O valor padrão em membros do domínio é **10**. O valor padrão em clientes e servidores autônomos é **10**. |
|**MaxAllowedPhaseOffset** |Todas as versões |Especifica a diferença máxima (em segundos) para a qual o W32Time tenta ajustar o relógio do computador usando a velocidade do clock. Quando a compensação excede essa taxa, o W32Time define o relógio do computador diretamente. O valor padrão para membros do domínio é **300**. O valor padrão para clientes e servidores autônomos é **1**. Para obter mais informações, confira [Como configurar o modo de redefinição do relógio do computador pelo Serviço de Hora do Windows](#configuring-how-windows-time-service-resets-the-computer-clock). |
|**MaxClockRate** |Todas as versões |Mantida pelo W32Time. Contém dados reservados usados pelo sistema operacional Windows. Qualquer alteração nessa configuração pode causar resultados imprevisíveis. O valor padrão para membros do domínio é **155.860**. O valor padrão para clientes e servidores autônomos é **155.860**. |
|**MaxNegPhaseCorrection** |Todas as versões |Especifica a maior correção de horário negativo, em segundos, feita pelo serviço. Se o serviço determinar que uma alteração maior que essa é necessária, ele registrará um evento em log.<p>**Observação**<br />O valor **0xFFFFFFFF** é um caso especial. Esse valor significa que o serviço sempre corrigirá a hora.<p>O valor padrão para membros do domínio é **0xFFFFFFFF**. O valor padrão para clientes e servidores autônomos é **54.000** (15 horas).|
|**MaxPollInterval** |Todas as versões |Especifica o maior intervalo, em log2 segundos, permitido para o intervalo de sondagem do sistema. Observe que, embora um sistema deva sondar de acordo com o intervalo agendado, um provedor poderá se recusar a produzir amostras quando solicitado. O valor padrão para controladores de domínio é **10**. O valor padrão para membros do domínio é **15**. O valor padrão para clientes e servidores autônomos é **15**. |
|**MaxPosPhaseCorrection** |Todas as versões |Especifica a maior correção de horário positivo em segundos feita pelo serviço. Se o serviço determinar que uma alteração maior que essa é necessária, ele registrará um evento em log.<p>**Observação**<br />O valor **0xFFFFFFFF** é um caso especial. Esse valor significa que o serviço sempre corrigirá a hora.<p>O valor padrão para membros do domínio é **0xFFFFFFFF**. O valor padrão para clientes e servidores autônomos é **54.000** (15 horas). |
|**MinClockRate** |Todas as versões |Mantida pelo W32Time. Contém dados reservados usados pelo sistema operacional Windows. Qualquer alteração nessa configuração pode causar resultados imprevisíveis. O valor padrão para membros do domínio é **155.860**. O valor padrão para clientes e servidores autônomos é **155.860**. |
|**MinPollInterval** |Todas as versões |Especifica o menor intervalo, em log2 segundos, permitido para o intervalo de sondagem do sistema. Observe que, embora um sistema não solicite amostras com mais frequência do que isso, um provedor poderá produzir amostras em horários diferentes do intervalo agendado. O valor padrão para controladores de domínio é **6**. O valor padrão para membros do domínio é **10**. O valor padrão para clientes e servidores autônomos é **10**. |
|**PhaseCorrectRate** |Todas as versões |Controla a velocidade na qual o erro de fase é corrigido. A especificação de um valor pequeno corrige o erro de fase rapidamente, mas pode causar instabilidade do relógio. Se o valor for muito grande, levará mais tempo para corrigir o erro da fase.<p>O valor padrão em membros do domínio é **1**. O valor padrão em clientes e servidores autônomos é **7**.<p>**Observação**<br />Zero não é um valor válido para a entrada do Registro **PhaseCorrectRate**. Em computadores Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2, se o valor for definido como **0**, o Serviço de Hora do Windows vai alterá-lo automaticamente para **1**. |
|**PollAdjustFactor** |Todas as versões |Controla a decisão de aumentar ou diminuir o intervalo de sondagem do sistema. Quanto maior o valor, menor a quantidade de erros que faz com que o intervalo de sondagem seja reduzido. O valor padrão em membros do domínio é **5**. O valor padrão em clientes e servidores autônomos é **5**. |
|**RequireSecureTimeSyncRequests** |Windows 8 e versões posteriores |Controla se o controlador de domínio responderá às solicitações de sincronização de hora que usam protocolos de autenticação mais antigos. Se habilitado (definido como **1**), o controlador de domínio não responderá às solicitações usando esses protocolos. Trata-se de uma configuração booliana, e o valor padrão é **0**. |
|**SpikeWatchPeriod** |Todas as versões |Especifica o tempo pelo qual uma diferença suspeita precisa persistir antes de ser aceita como correta (em segundos). O valor padrão em membros do domínio é **900**. O valor padrão em clientes e servidores autônomos e estações de trabalho é **900**. |
|**TimeJumpAuditOffset** |Todas as versões |Um inteiro sem sinal que indica o limite de auditoria de salto de tempo, em segundos. Se o serviço de horário ajustar o relógio local definindo diretamente o relógio e a correção de horário for maior que esse valor, o serviço de horário registrará um evento de auditoria em log. |
|**UpdateInterval** |Todas as versões |Especifica o número de tiques do relógio entre os ajustes de correção de fase. O valor padrão para controladores de domínio é **100**. O valor padrão para membros do domínio é **30.000**. O valor padrão para clientes e servidores autônomos é **360.000**.<p>**Observação**<br />Zero não é um valor válido para a entrada do Registro **UpdateInterval**. Em computadores que executam o Windows Server 2003, o Windows Server 2003 R2, o Windows Server 2008 e o Windows Server 2008 R2, se o valor for definido como **0**, o Serviço de Hora do Windows vai alterá-lo automaticamente para **1**.|
|**UtilizeSslTimeData** |Versões do Windows posteriores ao Windows 10 build 1511 |Um valor igual a **1** indica que o W32Time usa vários carimbos de data/hora SSL para propagar um relógio grosseiramente impreciso. |

### <a name="hklmsystemcurrentcontrolsetservicesw32timeparameters-subkey-entries"></a>Entradas da subchave <a id="parameters"></a>"HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters"

| Entrada de registro | Versões | Descrição |
| --- | --- | --- |
|**AllowNonstandardModeCombinations** |Todas as versões |Indica que as combinações de modo não padrão são permitidas na sincronização entre pares. O valor padrão para membros do domínio é **1**. O valor padrão para clientes e servidores autônomos é **1**. |
|**NtpServer** |Todas as versões |Especifica uma lista de pares delimitada por espaço da qual um computador obtém carimbos de data/hora, consistindo em um ou mais nomes DNS ou endereços IP por linha. Cada nome DNS ou endereço IP listado deve ser exclusivo. Os computadores conectados a um domínio devem sincronizar com uma fonte de horário mais confiável, como o horário oficial dos EUA.  <ul><li>0x01 SpecialInterval </li><li>0x02 UseAsFallbackOnly</li><li>0x04 SymmetricActive: Pra obter mais informações sobre esse modo, confira [Servidor de Hora do Windows: 3.3 Modos de operação](https://go.microsoft.com/fwlink/?LinkId=208012).</li><li>0x08 Cliente</li></ul><br />Não há valor padrão para essa entrada do Registro em membros do domínio. O valor padrão em clientes e servidores autônomos é time.windows.com,0x1.<p>**Observação**<br />Para obter mais informações sobre os servidores NTP disponíveis, confira o KB 262680, [Uma lista de servidores de horário do protocolo SNTP disponíveis na Internet](https://support.microsoft.com/help/262680/a-list-of-the-simple-network-time-protocol-sntp-time-servers-that-are) |
|**ServiceDll** |Todas as versões |Mantida pelo W32Time. Contém dados reservados usados pelo sistema operacional Windows. Qualquer alteração nessa configuração pode causar resultados imprevisíveis. A localização padrão para essa DLL em membros de domínio e clientes autônomos e servidores é %windir%\System32\W32Time.dll. |
|**ServiceMain** |Todas as versões |Mantida pelo W32Time. Contém dados reservados usados pelo sistema operacional Windows. Qualquer alteração nessa configuração pode causar resultados imprevisíveis. O valor padrão em membros do domínio é **SvchostEntry_W32Time**. O valor padrão em clientes e servidores autônomos é **SvchostEntry_W32Time**. |
|**Tipo** |Todas as versões |Indica de quais pares a sincronização será aceita:  <ul><li>**NoSync**. O serviço de horário não é sincronizado com outras fontes.</li><li>**NTP**. O serviço de horário é sincronizado dos servidores especificados no **NtpServer**. entrada do Registro.</li><li>**NT5DS**. O serviço de horário sincroniza da hierarquia de domínio.  </li><li>**AllSync**. O serviço de horário usa todos os mecanismos de sincronização disponíveis.  </li></ul>O valor padrão em membros do domínio é **NT5DS**. O valor padrão em clientes e servidores autônomos é **NTP**. |

### <a name="hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpclient-subkey-entries"></a>Entradas da subchave <a id="ntpclient"></a>"HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient"

|Entrada de registro |Versão |Descrição |
| --- | --- | --- |
|**AllowNonstandardModeCombinations** |Todas as versões |Indica que as combinações de modo não padrão são permitidas na sincronização entre pares. O valor padrão para membros do domínio é **1**. O valor padrão para clientes e servidores autônomos é **1**.|
|**CompatibilityFlags** |Todas as versões |Especifica os seguintes valores e sinalizadores de compatibilidade:<ul><li>**0x00000001**: DispersionInvalid</li><li>**0x00000002**: IgnoreFutureRefTimeStamp</li><li>**0x80000000**: AutodetectWin2K</li><li>**0x40000000**: AutodetectWin2KStage2</li></ul>O valor padrão para membros de domínio é **0x80000000**. O valor padrão para clientes e servidores autônomos é **0x80000000**. |
|**CrossSiteSyncFlags** |Todas as versões |Determina se o serviço escolhe os parceiros de sincronização fora do domínio do computador. As opções e os valores são:<ul><li>**0** – Nenhum</li><li>**1** – PdcOnly</li><li>**2** – Todos</li></ul>Esse valor será ignorado se o valor de NT5DS não estiver definido. O valor padrão para membros do domínio é **2**. O valor padrão para clientes e servidores autônomos é **2**. |
|**DllName** |Todas as versões |Especifica a localização da DLL para o provedor de horário.<p>A localização padrão dessa DLL em membros de domínio e clientes e servidores autônomos é **%windir%\System32\W32Time.dll**. |
|**Habilitada** |Todas as versões |Indica se o provedor NtpClient está habilitado no Serviço de Hora atual.<ul><li>**1** – Sim</li><li>**0** – Não</li></ul>O valor padrão em membros do domínio é **1**. O valor padrão em clientes e servidores autônomos é **1**.|
|**EventLogFlags** |Todas as versões |Especifica os eventos registrados em log pelo Serviço de Hora do Windows.<ul><li>**0x1**: alterações de acessibilidade</li><li>**0x2**: distorção de amostra grande (aplicável somente ao Windows Server 2003, ao Windows Server 2003 R2, ao Windows Server 2008 e ao Windows Server 2008 R2)</li></ul>O valor padrão em membros do domínio é **0x1**. O valor padrão em clientes e servidores autônomos é **0x1**.|
|**InputProvider** |Todas as versões |Indica se o NtpClient deve ser habilitado como um InputProvider, que obtém informações de hora do NtpServer. O NtpServer é um servidor de horário que responde às solicitações de tempo do cliente na rede retornando exemplos de horário que são úteis para sincronizar o relógio local. <ul><li>**1** – Sim</li><li>**0** – Não</li></ul>O valor padrão para membros do domínio e clientes autônomos é **1**. |
|**LargeSampleSkew** |Todas as versões |Especifica a distorção de amostra grande para o log, em segundos. Para estar em conformidade com as especificações da SEC (Security and Exchange Commission), isso deve ser definido como três segundos. Os eventos serão registrados em log para essa configuração somente quando **EventLogFlags** estiver explicitamente configurado para a distorção de amostra grande 0x2. O valor padrão em membros do domínio é 3. O valor padrão em clientes e servidores autônomos é 3. |
|**ResolvePeerBackOffMaxTimes** |Todas as versões |Especifica o número máximo de vezes a dobrar o intervalo de espera quando há tentativas repetidas de localizar um par com o qual foi feita uma sincronização com falha. Um valor de zero significa que o intervalo de espera é sempre o mínimo. O valor padrão em membros do domínio é **7**. O valor padrão em clientes e servidores autônomos é **7**. |
|**ResolvePeerBackoffMinutes** |Todas as versões |Especifica o intervalo inicial de espera, em minutos, antes da tentativa de localizar um par com o qual será feita a sincronização. O valor padrão em membros do domínio é **15**. O valor padrão em clientes e servidores autônomos é **15**.  |
|**SpecialPollInterval** |Todas as versões |Especifica o intervalo de sondagem especial, em segundos, para os pares manuais. Quando o sinalizador **SpecialInterval** 0x1 está habilitado, o W32Time usa esse intervalo de sondagem, em vez de um intervalo de sondagem determinado pelo sistema operacional. O valor padrão em membros do domínio é **3.600**. O valor padrão em clientes e servidores autônomos é **604.800**.<br/><br/>Uma novidade do build 1703, o **SpecialPollInterval** está contido nos valores do Registro de configuração **MinPollInterval** e **MaxPollInterval**.|
|**SpecialPollTimeRemaining** |Todas as versões |Mantida pelo W32Time. Contém dados reservados usados pelo sistema operacional Windows. Ele especifica o tempo, em segundos, antes que o W32Time seja ressincronizado após a reinicialização do computador. Qualquer alteração a essa configuração pode causar resultados imprevisíveis. O valor padrão em membros do domínio e em clientes e servidores autônomos é deixado em branco. |

### <a name="hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpserver-subkey-entries"></a>Entradas da subchave <a id="ntpserver"></a>"HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer"

|Entrada do Registro |Versões |Descrição |
| --- | --- | --- |
|**AllowNonstandardModeCombinations** |Todas as versões |Indica que as combinações de modo não padrão são permitidas na sincronização entre clientes e servidores. O valor padrão para membros do domínio é **1**. O valor padrão para clientes e servidores autônomos é **1**. |
|**DllName** |Todas as versões |Especifica a localização da DLL para o provedor de horário. A localização padrão dessa DLL em membros de domínio e clientes e servidores autônomos é **%windir%\System32\W32Time.dll**.  |
|**Habilitada** |Todas as versões |Indica se o provedor NtpServer está habilitado no Serviço de Hora atual. <ul><li>**1** – Sim</li><li>**0** – Não</li></ul>O valor padrão em membros do domínio é **1**. O valor padrão em clientes e servidores autônomos é **1**. |
|**InputProvider** |Todas as versões |Indica se o NtpClient deve ser habilitado como um InputProvider, que obtém informações de hora do NtpServer. O NtpServer é um servidor de horário que responde às solicitações de tempo do cliente na rede retornando exemplos de horário que são úteis para sincronizar o relógio local. <ul><li>**1** – Sim</li><li>**0** – Não = 0 </li></ul>Valor padrão para membros do domínio e clientes autônomos: 1 |

## <a name="reference-pre-set-values-for-the-windows-time-service-gpo-settings"></a>Referência: valores predefinidos para as configurações do GPO do Serviço de Hora do Windows  

A tabela a seguir lista as configurações de Política de Grupo globais associadas ao Serviço de Horário do Windows e o valor predefinido associado a cada configuração. Para obter mais informações sobre cada configuração, confira as entradas do Registro correspondentes em [Referência: entradas do Registro do Serviço de Hora do Windows](#reference-windows-time-service-registry-entries) anteriormente neste artigo. As configurações a seguir estão contidas em um GPO chamado **Definições de Configuração Global**.  

### <a name="pre-set-values-for-global-group-policy-settings"></a>Valores predefinidos para as configurações de "Política de Grupo Global"

|Configuração da Política de Grupo|Valor predefinido|  
| --- | --- |
|**AnnounceFlags**|**10**|  
|**EventLogFlags**|**2**|  
|**FrequencyCorrectRate**|**4**|  
|**HoldPeriod**|**5**|  
|**LargePhaseOffset**|**1.280.000**|  
|**LocalClockDispersion**|**10**|  
|**MaxAllowedPhaseOffset**|**300**|  
|**MaxNegPhaseCorrection**|**54.000** (15 horas)|  
|**MaxPollInterval**|**15**|  
|**MaxPosPhaseCorrection**|**54.000** (15 horas)|  
|**MinPollInterval**|**10**|  
|**PhaseCorrectRate**|**7**|  
|**PollAdjustFactor**|**5**|  
|**SpikeWatchPeriod**|**90**|  
|**UpdateInterval**|**100**|  

### <a name="pre-set-values-for-configure-windows-ntp-client-settings"></a>Valores predefinidos para as configurações de "Configurar Cliente NTP do Windows"

A tabela a seguir lista as configurações disponíveis para o GPO **Configurar o Windows NTP Client** e os valores predefinidos associados ao Serviço de Horário do Windows. Para obter mais informações sobre cada configuração, confira as entradas do Registro correspondentes em [Referência: entradas do Registro do Serviço de Hora do Windows](#reference-windows-time-service-registry-entries) anteriormente neste artigo.  

|Configuração da Política de Grupo|Valor predefinido|  
|------------------------|-----------------|  
|**NtpServer**|time.windows.com, **0x1**|  
|**Tipo**|Opções padrão:<ul><li>**NTP.** Use em computadores que não ingressaram em um domínio.</li><li>**NT5DS.** Use em computadores que ingressaram em um domínio.</li></ul>|  
|**CrossSiteSyncFlags**|**2**|  
|**ResolvePeerBackoffMinutes**|**15**|  
|**ResolvePeerBackoffMaxTimes**|**7**|  
|**SpecialPollInterval**|**3.600**|  
|**EventLogFlags**|**0**|  

## <a name="reference-network-ports-that-the-windows-time-service-uses"></a>Referência: portas de rede usadas pelo Serviço de Hora do Windows

O Horário do Windows segue a especificação NTP, que requer o uso da porta UDP 123 para todas as comunicações de sincronização de horário. Essa porta é reservada pelo Horário do Windows e permanece reservada sempre. Sempre que o computador sincroniza seu relógio ou fornece horário a outro computador, essa comunicação é executada na porta UDP 123.  

> [!NOTE]  
> Se você tiver um computador que tenha vários adaptadores de rede (também chamado de computador *multihomed*), não poderá habilitar seletivamente o Serviço de Hora do Windows com base no adaptador de rede.  

## <a name="related-information"></a>Informações relacionadas  

Os recursos a seguir contêm informações adicionais que são relevantes para esta seção.  

- RFC *1305* no Banco de Dados RFC IETF  
