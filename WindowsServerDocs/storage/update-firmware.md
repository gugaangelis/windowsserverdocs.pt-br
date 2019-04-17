---
ms.assetid: e5945bae-4a33-487c-a019-92a69db8cf6c
title: Atualizar o firmware da unidade no Windows Server 2016
ms.prod: windows-server-threshold
ms.author: toklima
ms.manager: dmoss
ms.technology: storage-spaces
ms.topic: article
author: toklima
ms.date: 10/04/2016
ms.openlocfilehash: 90019ed8425d72d30059be5d47458785cac34c73
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="updating-drive-firmware-in-windows-server-2016"></a>Atualizando o firmware da unidade no Windows Server 2016
>Aplicável a: Windows 10, Windows Server (canal semestral), Windows Server 2016

A atualização do firmware para unidades historicamente tem sido uma tarefa difícil com potencial para tempo de inatividade. Por isso, estamos fazendo aprimoramentos nos Espaços de Armazenamento, no Windows Server, Windows 10, versão 1703 e mais recente. Se você tiver unidades que dão suporte ao novo mecanismo de atualização de firmware incluído no Windows, poderá atualizar o firmware da unidade de unidades em produção sem tempo de inatividade. No entanto, se você pretende atualizar o firmware de uma unidade de produção, leia nossas dicas sobre como minimizar o risco ao usar essa nova e poderosa funcionalidade.

  > [!Warning]
  > Atualizações de firmware são uma operação de manutenção potencialmente perigosa e você só deve aplicá-las após um teste completo da nova imagem do firmware. É possível que esse novo firmware em hardware sem suporte afete negativamente a estabilidade e a confiabilidade ou até mesmo cause perda de dados. Os administradores devem ler as notas de versão que acompanham uma determinada atualização para determinar o impacto e a aplicabilidade.

## <a name="drive-compatibility"></a>Compatibilidade de unidade

Para usar o Windows Server para atualizar o firmware da unidade, você deverá ter unidades compatíveis. Para garantir o comportamento comum do dispositivo, começamos definindo requisitos novos e - para Windows 10 e Windows Server 2016 - opcionais de Hardware Lab Kit (HLK) para dispositivos SAS, SATA e NVMe. Esses requisitos descrevem a quais comandos um dispositivo NVMe, SAS ou SATA devem oferecer suporte para o firmware atualizável usando esses cmdlets do PowerShell novos e nativos do Windows. Para oferecer suporte a esses requisitos, há um novo teste de HLK para verificar se os produtos do fornecedor dão suporte aos comandos certos e para fazê-los serem implementados em revisões futuras. 

Para obter informações sobre se o hardware oferece suporte a atualização do firmware da unidade do Windows, entre em contato com o fornecedor da solução.
Aqui estão links para os diversos requisitos:

-   SATA: [Device.Storage.Hd.Sata](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/device-storage#devicestoragehdsata) - na seção **[If Implemented\] Firmware Download & Activate**
    
-   SAS: [Device.Storage.Hd.Sas](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/device-storage#devicestoragehdsas) - na seção **[If Implemented\] Firmware Download & Activate**

-   NVMe: [Device.Storage.ControllerDrive.NVMe](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/device-storage#devicestoragecontrollerdrivenvme) - nas seções **5.7** e **5.8**.

## <a name="powershell-cmdlets"></a>Cmdlets do PowerShell

Os dois cmdlets adicionados ao Windows são:

-   Get-StorageFirmwareInformation
-   Update-StorageFirmware

O primeiro cmdlet fornece informações detalhadas sobre os recursos do dispositivo, as imagens de firmware e as revisões. Nesse caso, a máquina contém apenas um único SSD SATA com 1 slot de firmware. Aqui está um exemplo:

   ```powershell
   Get-PhysicalDisk | Get-StorageFirmwareInformation

   SupportsUpdate        : True
   NumberOfSlots         : 1
   ActiveSlotNumber      : 0
   SlotNumber            : {0}
   IsSlotWritable        : {True}
   FirmwareVersionInSlot : {J3E16101}
   ```

Observe que os dispositivos SAS sempre informam "SupportsUpdate" como "True", porque não há nenhuma maneira de consultar explicitamente o dispositivo para oferecer suporte a esses comandos.

O segundo cmdlet, Update-StorageFirmware, permite aos administradores atualizarem o firmware da unidade com um arquivo de imagem, se a unidade aceitar o novo mecanismo de atualização de firmware. Você deve obter esse arquivo de imagem diretamente do fornecedor OEM ou unidade.

  > [!Note]
  > Antes de atualizar qualquer hardware de produção, teste a imagem do firmware específico em hardware idêntico em um laboratório.

A unidade primeiro carregará a nova imagem de firmware para uma área de preparo interna. Enquanto isso acontece, E/S continua normalmente. A imagem é ativada após o download. Durante esse tempo, a unidade não poderá responder a comandos de E/S porque ocorre uma reinicialização interna. Isso significa que essa unidade não serve dados durante a ativação. Um aplicativo que acessa dados nessa unidade precisaria aguardar uma resposta até que a ativação de firmware tivesse sido concluída. Aqui está um exemplo do cmdlet em ação:

   ```powershell 
   $pd | Update-StorageFirmware -ImagePath C:\Firmware\J3E160@3.enc -SlotNumber 0
   $pd | Get-StorageFirmwareInformation

   SupportsUpdate        : True
   NumberOfSlots         : 1
   ActiveSlotNumber      : 0
   SlotNumber            : {0}
   IsSlotWritable        : {True}
   FirmwareVersionInSlot : {J3E160@3}
   ```

As unidades normalmente não concluem solicitações de E/S quando ativam uma nova imagem de firmware. O tempo que uma unidade demora para ativar depende de seu design e do tipo de firmware que você atualiza. Observamos que o tempo de atualização varia de menos de 5 segundos a mais de 30 segundos.

Essa unidade realizou a atualização do firmware em ~5,8 segundos, como mostrado aqui:

```powershell 
Measure-Command {$pd | Update-StorageFirmware -ImagePath C:\\Firmware\\J3E16101.enc -SlotNumber 0}

 Days : 0
 Hours : 0
 Minutes : 0
 Seconds : 5
 Milliseconds : 791
 Ticks : 57913910
 TotalDays : 6.70299884259259E-05
 TotalHours : 0.00160871972222222
 TotalMinutes : 0.0965231833333333
 TotalSeconds : 5.791391
 TotalMilliseconds : 5791.391
 ```

## <a name="updating-drives-in-production"></a>Atualizando unidades em produção

Antes de colocar um servidor em produção, é altamente recomendável atualizar o firmware de suas unidades para o firmware recomendado pelo fornecedor de hardware ou OEM que vende e oferece suporte à sua solução (compartimentos de armazenamento, unidades e servidores).

Quando um servidor estiver em produção, é uma boa ideia fazer poucas alterações no servidor, por praticidade. No entanto, algumas vezes o fornecedor da solução pode avisar que há uma atualização de firmware extremamente importante para as unidades. Se isso ocorrer, aqui estão algumas práticas recomendadas a seguir antes de aplicar atualizações de firmware da unidade:

1. Revise as notas de versão de firmware e confirme que a atualização corrige problemas que poderiam afetar seu ambiente e que o firmware não contém os problemas conhecidos que podem prejudicá-lo.

2. Instale o firmware em um servidor em seu laboratório com unidades idênticas (incluindo a revisão da unidade, se houver várias revisões da mesma unidade) e teste a unidade sob carga com o novo firmware. Para obter informações sobre como fazer o teste de carga sintética, consulte [Testar o desempenho dos Espaços de Armazenamento usando cargas de trabalho sintéticas](https://technet.microsoft.com/en-us/library/dn894707.aspx).

## <a name="automated-firmware-updates-with-storage-spaces-direct"></a>Atualizações de firmware automatizadas com Espaços de Armazenamento Diretos

O Windows Server 2016 inclui um serviço de integridade para implantações de Espaços de Armazenamento Diretos (inclusive soluções de pilha do Microsoft Azure). O objetivo principal do serviço de integridade é facilitar o monitoramento e o gerenciamento da implantação do hardware. Como parte de suas funções de gerenciamento, ele tem a capacidade de distribuição de firmware da unidade em um cluster inteiro sem receber cargas de trabalho offline ou incorrer em tempo de inatividade. Esse recurso é orientado por políticas, com o controle nas mãos do administrador.

Usar o serviço de integridade para distribuir firmware em um cluster é muito simples e envolve as seguintes etapas:

-   Identificar quais unidades de discos rígidos e SSD você pretende que façam parte do seu cluster de Espaços de Armazenamento Diretos e se as unidades dão suporte às atualizações de firmware realizadas pelo Windows
-   Listar as unidades no arquivo xml de componentes com suporte
-   Identificar as versões de firmware que você espera que essas unidades tenham no xml de componentes com suporte (incluindo caminhos de local das imagens de firmware)
-   Carregar o arquivo xml no banco de dados do cluster

Neste ponto, o serviço de integridade inspecionará e analisará o xml e identificará as unidades que não têm a versão do firmware desejada implantada. Ele continuará para redirecionar E/S para fora das unidades afetadas – indo nós por nó – e para atualizar o firmware nelas. Um cluster de Espaços de Armazenamento Diretos atinge resiliência distribuindo dados em vários nós de servidor; é possível para o serviço de integridade isolar um nó inteiro de unidades para atualizações. Uma vez que o nó é atualizado, ele iniciará um reparo nos Espaços de Armazenamento, trazendo todas as cópias de dados no cluster de volta para sincronizar com os outros, antes de passar para o próximo nó. É esperado e normal que os Espaços de Armazenamento mudem para um modo de operação "degradado" enquanto o firmware é distribuído.

Para garantir uma distribuição estável e tempo suficiente de validação de uma nova imagem de firmware, existe um atraso significativo entre as atualizações de vários servidores. Por padrão, o serviço de integridade aguardará 7 dias antes de atualizar o 2<sup>o</sup> servidor. Os servidores subsequentes (3<sup>o</sup>, 4<sup>o</sup>, ...) atualizam com 1 dia de atraso. Se um administrador considerar que o firmware está instável ou de alguma maneira indesejável, ele pode parar a distribuição pelo serviço de integridade a qualquer momento. Se o firmware tiver sido validado anteriormente e uma distribuição mais rápida for desejada, esses valores padrão poderão ser modificados de dias para horas ou minutos.

Veja aqui um exemplo de xml de componentes com suporte para um cluster genérico de Espaços de Armazenamento Diretos:

```xml
 <Components>
     <Disks>
        <Disk>
            <Manufacturer>Contoso</Manufacturer>
            <Model>XYZ9000</Model>
            <AllowedFirmware>
              <Version>2.0</Version>
              <Version>2.1>/Version>
              <Version>2.2</Version>
            </AllowedFirmware>
            <TargetFirmware>
              <Version>2.2</Version>
              <BinaryPath>\\path\to\image.bin</BinaryPath>
            </TargetFirmware>
        </Disk>
        ...
        ...
    </Disks>
 </Components>
```

Para fazer a distribuição do novo firmware iniciar neste cluster de Espaços de Armazenamento Diretos, basta carregar o .xml para o banco de dados do cluster:

```powershell
$SpacesDirect = Get-StorageSubSystem Clus*

$CurrentDoc = $SpacesDirect | Get-StorageHealtHealth Service etting -Name "System.Storage.SupportedComponents.Document"

$CurrentDoc.Value | Out-File <Path>
```

Edite o arquivo no seu editor favorito, como o Visual Studio Code ou bloco de notas e, em seguida, salve.

```powershell
$NewDoc = Get-Content <Path> | Out-String

$SpacesDirect | Set-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document" -Value $NewDoc
```

Se você quiser ver o serviço de integridade em ação e saber mais sobre seu mecanismo de distribuição, veja este vídeo: https://channel9.msdn.com/Blogs/windowsserver/Update-Drive-Firmware-Without-Downtime-in-Storage-Spaces-Direct

## <a name="frequently-asked-questions"></a>Perguntas frequentes

Consulte também [Solução de problemas de atualizações de firmware de unidade](troubleshoot-firmware-update.md).

### <a name="will-this-work-on-any-storage-device"></a>Isso funcionará em qualquer dispositivo de armazenamento

Isso funcionará em dispositivos de armazenamento que implementam os comandos corretos no firmware. O cmdlet Get-StorageFirmwareInformation mostrará se firmware da unidade, de fato, oferece suporte os comandos corretos (para SATA/NVMe) e o teste HLK permite que os fornecedores e os OEMs testem esse comportamento.

### <a name="after-i-update-a-sata-drive-it-reports-to-no-longer-support-the-update-mechanism-is-something-wrong-with-the-drive"></a>Depois de atualizar uma unidade SATA, ele relata que não oferece mais suporte ao mecanismo de atualização. Há algo de errado com a unidade

Não, a unidade está bem, a menos que o novo firmware não permita mais atualizações. Você está atingindo um problema conhecido no qual uma versão em cache dos recursos da unidade está incorreta. Executar “Update-StorageProviderCache -DiscoveryLevel Full” enumerará novamente os recursos de unidade e atualizará a cópia armazenada em cache. Como uma solução alternativa, recomendamos executar o comando acima uma vez antes de iniciar uma atualização de firmware ou completar a distribuição em um cluster de Espaços Diretos.

### <a name="can-i-update-firmware-on-my-san-through-this-mechanism"></a>Posso atualizar o firmware em minha SAN por meio desse mecanismo
Não. SANs geralmente têm seus próprios utilitários e suas próprias interfaces para essas operações de manutenção. Esse novo mecanismo é para armazenamento com conexão direta, como dispositivos NVMe, SAS ou SATA.

### <a name="from-where-do-i-get-the-firmware-image"></a>Onde posso obter a imagem do firmware

Você deve sempre obter firmware diretamente do seu OEM, fornecedor da solução ou fornecedor da unidade e não baixá-lo de terceiros. O Windows fornece o mecanismo para obter a imagem para a unidade, mas não verifica sua integridade.

### <a name="will-this-work-on-clustered-drives"></a>Isso funcionará em unidades de cluster

Os cmdlets também podem executar sua função em unidades de cluster, mas tenha em mente que a orquestração do serviço de integridade reduz o impacto de E/S em cargas de trabalho em execução. Se os cmdlets forem usados diretamente em unidades de cluster, E/S provavelmente paralisará. Em geral, é uma prática recomendada executar atualizações de firmware de unidade quando não houver nenhuma ou apenas uma carga de trabalho mínima nas unidades subjacentes.

### <a name="what-happens-when-i-update-firmware-on-storage-spaces"></a>O que acontece quando eu atualizo o firmware em Espaços de Armazenamento

No Windows Server 2016, com o serviço de integridade implantados em Espaços de Armazenamento Diretos, você pode executar esta operação sem colocar suas cargas de trabalho offline, supondo que as unidades deem suporte ao Windows Server que está atualizando o firmware.

### <a name="what-happens-if-the-update-fails"></a>O que acontece se a atualização falhar

A atualização pode falhar por várias razões, algumas delas são: 1) a unidade não oferece suporte aos comandos corretos para o Windows atualizar seu firmware. Nesse caso, a nova imagem de firmware nunca é ativada e a unidade continuará funcionando com a imagem antiga. 2) A imagem não pode baixar ou ser aplicada a essa unidade (incompatibilidade de versão, imagem corrompida, ...). Nesse caso, ocorre falha no comando de ativação da unidade. Novamente, a imagem antiga do firmware continuará funcionando.

Se a unidade não responder após uma atualização de firmware, você provavelmente estará atingindo um bug no próprio firmware da unidade. Teste todas as atualizações de firmware em um ambiente de laboratório antes de colocá-las em produção. A única correção poderá ser substituir a unidade.

Para obter mais informações, consulte [Solução de problemas de atualizações de firmware de unidade](troubleshoot-firmware-update.md).

### <a name="how-do-i-stop-an-in-progress-firmware-roll-out"></a>Como posso interromper a distribuição de um firmware em andamento

Desabilite a distribuição no PowerShell por meio de:
```powershell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.Enabled" -Value false
```

### <a name="i-am-seeing-an-access-denied-or-path-not-found-error-during-roll-out-how-do-i-fix-this"></a>Vejo um acesso negado ou erro de caminho não encontrado durante a distribuição. Como faço para corrigir isso

Verifique se a imagem do firmware que você deseja usar para a atualização está acessível por todos os nós do cluster. A maneira mais fácil de garantir isso é colocá-lo em um volume compartilhado do cluster.
