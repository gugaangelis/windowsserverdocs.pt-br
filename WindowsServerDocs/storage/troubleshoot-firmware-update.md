---
ms.assetid: 13210461-1e92-48a1-91a2-c251957ba256
title: Solução de problemas de atualizações de firmware de unidade
ms.prod: windows-server-threshold
ms.author: toklima
ms.manager: masriniv
ms.technology: storage
ms.topic: article
author: toklima
ms.date: 04/18/2017
ms.openlocfilehash: 8283b87e9505b1d3f47ddc823016fbcc7c0c29e6
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867052"
---
# <a name="troubleshooting-drive-firmware-updates"></a>Solução de problemas de atualizações de firmware de unidade

>Aplica-se a: Windows 10, Windows Server (canal semestral),

O Windows 10, versão 1703, e o novo Windows Server (canal semestral) incluem a capacidade de atualizar o firmware de HDDs e SSDs que foram certificados com o AQ (Qualificador Adicional) de upgrade de firmware por meio do PowerShell.

Você pode encontrar mais informações sobre esse recurso aqui:

- [Atualizando o firmware da unidade no Windows Server 2016](update-firmware.md)
- [Atualizar o firmware da unidade sem tempo de inatividade no Espaços de Armazenamento Diretos](https://channel9.msdn.com/Blogs/windowsserver/Update-Drive-Firmware-Without-Downtime-in-Storage-Spaces-Direct)

As atualizações de firmware podem falhar por vários motivos. O objetivo deste artigo é ajudar na solução de problemas avançada.

  > [!Note]
  > As informações contidas neste artigo, dependendo do problema, podem não ser suficientes para depurar todos os casos de falha possíveis.

## <a name="common-issues"></a>Problemas comuns
Em termos de arquitetura, esse novo recurso depende de APIs implementadas na pilha de armazenamento do Windows, em que o PowerShell é chamado. A pilha de armazenamento depende de drivers e hardware para implementar comandos definidos pelo setor adequadamente. Isso resulta em vários pontos em que podem ocorrer falhas. Os problemas mais comumente observados são:

1.  Uma determinada unidade não implementa corretamente os comandos padrão do setor (não tem o AQ)
2.  As APIs necessárias para executar a atualização não são implementadas ou estão com defeito (se drivers de terceiros forem usados)
3.  As APIs funcionam, mas há um problema com o firmware em si (imagem inválida/corrompida…)

As seções a seguir descrevem informações de solução de problemas, dependendo se drivers da Microsoft ou de terceiros são usados.

## <a name="identifying-inappropriate-hardware"></a>Identificando hardware inadequado
A maneira mais rápida para identificar se um dispositivo dá suporte ao conjunto correto de comandos é simplesmente iniciar o PowerShell e passar um objeto do disco que representa PhysicalDisk para o cmdlet Get-StorageFirmwareInfo. Veja um exemplo:

```powershell
Get-PhysicalDisk -SerialNumber 15140F55976D | Get-StorageFirmwareInformation
```
Veja também um exemplo de saída:

```
PhysicalDisk          : MSFT_PhysicalDisk (ObjectId = "{1}\\TOKLIMA-DL380\root/Microsoft/Windo...)
SupportsUpdate        : True
NumberOfSlots         : 1
ActiveSlotNumber      : 0
SlotNumber            : {0}
IsSlotWritable        : {True}
FirmwareVersionInSlot : {0013}
```

O campo SupportsUpdate, pelo menos para dispositivos SATA e NVMe, indicará se a funcionalidade interna do PowerShell pode ser usada para atualizar o firmware.

O campo SupportsUpdate sempre relatará "True" para dispositivos conectados a SAS, pois não é possível consultar o suporte de comando apropriado com comandos padrão do setor.

Para validar se um dispositivo SAS aceita o conjunto de comandos necessários, existem duas opções:
1.  Testar via cmdlet Update-StorageFirmware com uma imagem do firmware apropriada, ou
2.  Consulte o catálogo do Windows Server para identificar quais dispositivos SAS obtiveram com êxito a atualização do FW AQ (https://www.windowsservercatalog.com/)

### <a name="remediation-options"></a>Opções de correção
Se um determinado dispositivo que você está testando não der suporte ao conjunto de comandos apropriado, consulte seu fornecedor para ver se um firmware atualizado que fornece o conjunto de comandos necessário está disponível ou consulte o catálogo do Windows Server para identificar dispositivos de origem que implementam o conjunto de comandos apropriado.

## <a name="troubleshooting-with-3rd-party-drivers-sas"></a>Solução de problemas com drivers de terceiros (SAS)
Os componentes de software que melhor interagem com hardware são drivers de miniporta na pilha de armazenamento do Windows. Para alguns protocolos de armazenamento, como SATA e NVMe, a Microsoft fornece drivers nativos do Windows. Esses drivers permitem obter informações adicionais de depuração. No entanto, fornecedores de hardware e software de terceiros são livres para criar seus próprios drivers de miniporta para seus dispositivos e seu nível de suporte para obter informações de depuração pode variar.

Para identificar o que aconteceu com o download de firmware e ativar as APIs enviadas para a pilha de armazenamento, independentemente do driver de miniporta, consulte o canal de log de eventos a seguir:

Visualizador de Eventos - Logs de aplicativos e serviços - Microsoft - Windows - StorDiag - **Microsoft-Windows-Storage-ClassPnP/Operational**

Esses canal registra as informações sobre as APIs do Windows enviadas para os drivers de miniporta e suas respostas. Por exemplo, a condição de erro mostrada diretamente abaixo é apresentada ao tentar fazer download de uma imagem de firmware para um dispositivo SATA, que é conectado por meio de um HBA SAS que não implementa corretamente a conversão necessária de SAS em SATA:

```powershell
Get-PhysicalDisk -SerialNumber 44GS103UT5EW | Update-StorageFirmware -ImagePath C:\Firmware\J3E160@3.enc -SlotNumber 0
```
Eis um exemplo da saída:

```
Update-StorageFirmware : Failed

Extended information:
A warning or error has been encountered during storage firmware update.
Incorrect function.

Activity ID: {1224482b-2315-4a38-81eb-27bb7de19c00}
At line:1 char:47
+ ... S103UT5EW | Update-StorageFirmware -ImagePath C:\Firmware\J3E160@3.en ...
+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : NotSpecified: (:) [Update-StorageFirmware], CimException
+ FullyQualifiedErrorId : StorageWMI 4,Microsoft.Management.Infrastructure.CimCmdlets.InvokeCimMethodCommand,Update-StorageFirmware
```
    
O PowerShell gerará um erro e receberá informações indicando que a função chamada (ou seja, API de Kernel) estava incorreta. Isso pode indicar que a API não foi implementada pelo driver de miniporta SAS de terceiros (verdadeiro nesse caso) ou que a API falhou por outro motivo, como um alinhamento incorreto de segmentos de download.

```
EventData
DeviceGUID  {132EDB55-6BAC-A3A0-C2D5-203C7551D700}
DeviceNumber    1
Vendor  ATA 
Model   TOSHIBA THNSNJ12
FirmwareVersion 6101
SerialNumber    44GS103UT5EW
DownLevelIrpStatus  0xc0000185
SrbStatus   132
ScsiStatus  2
SenseKey    5
AdditionalSenseCode 36
AdditionalSenseCodeQualifier    0
CdbByteCount    10
CdbBytes    3B0E0000000001000000
NumberOfRetriesDone 0
```

O evento 507 do ETW do canal mostra que uma solicitação SRB SCSI falhou e fornece as informações adicionais que SenseKey era ' 5 ' (solicitação inválida) e que AdditionalSense Information era ' 36 ' (campo inválido em CDB).

   > [!Note]
   > Essas informações são fornecidas diretamente pela miniporta em questão e a precisão dessas informações depende da implementação e da sofisticação do driver de miniporta.

É possível que condições de erro diferentes exibam os mesmos códigos de erro, se o driver de miniporta não diferenciá-las. Por exemplo, tentar fazer download de uma imagem de firmware inválida por meio de um HBA SAS para um dispositivo SATA (que deve falhar) pode ser convertido nos mesmos códigos de falha.

Em casos onde protocolos são misturados e podem ocorrer conversões, ou seja, SATA por trás de SAS, é melhor testar o dispositivo SATA diretamente conectado a um controlador de SATA para descartá-lo como um problema em potencial.

### <a name="remediation-options"></a>Opções de correção
Se o driver de terceiros for identificado como não implementando as APIs ou conversões necessárias, será possível usar as alternativas fornecidas pela Microsoft para SATA (StorAHCI.sys) e NVMe (StorNVMe.sys) ou entrar em contato com o fornecedor OEM ou HBA que forneceu o driver SAS e consultar se existe uma versão mais recente com o suporte correto.

## <a name="additional-troubleshooting-with-microsoft-drivers-satanvme"></a>Solução de problemas adicional com drivers da Microsoft (SATA/NVMe)
Quando os drivers nativos do Windows, como StorAHCI.sys ou StorNVMe.sys, são usados para dispositivos de armazenamento de energia, é possível obter informações adicionais sobre casos de falha possíveis durante operações de atualização de firmware.

Além do canal operacional ClassPnP, StorAHCI e StorNVMe registrarão em log os códigos de retorno específicos do protocolo do dispositivo no seguinte canal do ETW:

Visualizador de Eventos - Logs de aplicativos e serviços - Microsoft - Windows - StorDiag - **Microsoft-Windows-Storage-StorPort/Diagnose**

Os logs de diagnóstico não são mostrados por padrão e podem ser ativados/mostrados clicando em "Exibir" no Visualizador de Eventos e selecionando "Mostrar Logs Analíticos e de Depuração" no menu suspenso.

Para coletar essas entradas avançadas do log, habilite o log, reproduza a falha de atualização de firmware e salve o log de diagnóstico.

Aqui está um exemplo de uma atualização de firmware em um dispositivo SATA falhando, porque a imagem a ser baixada era inválida (ID do evento: 258):

``` 
EventData
MiniportName    storahci
MiniportEventId 19
MiniportEventDescription    Firmware Activate Completion
PortNumber  0
Bus 2
Target  0
LUN 0
Irp 0xffff8c84cd45aca0
Srb 0xffffab0024030bc0
Parameter1Name  SrbStatus
Parameter1Value 130
Parameter2Name  ReturnCode
Parameter2Value 0
Parameter3Name  FeaturesReg
Parameter3Value 15
Parameter4Name  SectorCountReg
Parameter4Value 0
Parameter5Name  DriveHeadReg
Parameter5Value 160
Parameter6Name  CommandReg
Parameter6Value 146
Parameter7Name  NULL
Parameter7Value 0
Parameter8Name  NULL
Parameter8Value 0
```

O evento acima contém informações detalhadas de dispositivo nos valores de parâmetro 2 a 6. Aqui estamos olhando vários valores do registro ATA. A especificação ATA ACS pode ser usada para decodificar os valores abaixo pela falha de um comando Download Microcode:
- Código de retorno: 0 (0000 0000) (N/A-sem sentido porque nenhuma carga foi transferida)
- Recursos: 15 (0000 1111) (bit 1 é definido como "1" e indica "Abort")
- SectorCount: 0 (0000 0000) (N/A)
- DriveHead: 160 (1010 0000) (N/A – somente bits obsoletos são definidos)
- Linha 146 (1001 0010) (bit 1 é definido como "1" indicando a disponibilidade dos dados de detecção)

Isso informa que a operação de atualização de firmware foi cancelada pelo dispositivo.

Um nível semelhante de informações de depuração está disponível neste canal ao usar dispositivos NVMe com o driver NVMe nativo do Windows (StorNVMe.sys).
