---
ms.assetid: 2bab6bf6-90e7-46a7-b917-14a7a8f55366
title: Gerenciamento de integridade de memória de classe de armazenamento (NVDIMM-N) no Windows
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: JasonGerend
ms.date: 08/24/2016
ms.localizationpriority: medium
ms.openlocfilehash: 0c39d704056c4ae6935f3be9c521c12ca1014820
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870547"
---
# <a name="storage-class-memory-nvdimm-n-health-management-in-windows"></a>Gerenciamento de integridade de memória de classe de armazenamento (NVDIMM-N) no Windows
> Aplica-se a: Windows Server 2016, Windows 10 (versão 1607)

Este artigo fornece aos administradores de sistema e profissionais de TI informações sobre o gerenciamento da integridade e o tratamento de erros específicos para dispositivos de memória de classe de armazenamento (NVDIMM-N) no Windows, destacando as diferenças entre a memória de classe de armazenamento e dispositivos de armazenamento tradicionais.

Se você não estiver familiarizado com o suporte do Windows para dispositivos de memória de classe de armazenamento, esses vídeos curtos fornecerão uma visão geral:
- [Usando memória não volátil (NVDIMM-N) como armazenamento em bloco no Windows Server 2016](https://channel9.msdn.com/Events/Build/2016/P466)
- [Usando memória não volátil (NVDIMM-N) como armazenamento endereçável por Byte no Windows Server 2016](https://channel9.msdn.com/Events/Build/2016/P470)
- [Acelerando o desempenho do SQL Server 2016 com memória persistente no Windows Server 2016](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-Windows-Server-2016-SCM--FAST)

Dispositivos de memória de classe de armazenamento NVDIMM-N compatíveis com JEDEC têm suporte no Windows com drivers nativos do Windows Server 2016 e do Windows 10 (versão 1607). Embora esses dispositivos se comportem como outros discos (HDDs e SSDs), há algumas diferenças.

Todas as condições listadas aqui devem ser ocorrências raras, mas dependem das condições em que o hardware é usado.

Os vários casos abaixo podem se referir a configurações de espaços de armazenamento. A configuração de maior interesse é quando dois dispositivos NVDIMM-N são utilizados como um cache de write-back espelhado em um espaço de armazenamento. Para definir essa configuração, confira [Configurar espaços de armazenamento com um cache de write-back NVDIMM-N](https://msdn.microsoft.com/library/mt650885.aspx).

No Windows Server 2016, a GUI de Espaços de Armazenamento mostra o tipo de barramento NVDIMM N como DESCONHECIDO. Ele não tem qualquer perda de funcionalidade ou incapacidade na criação do Pool, Armazenamento VD. Você pode verificar o tipo de barramento executando o seguinte comando:

```powershell
PS C:\>Get-PhysicalDisk | fl
```
O parâmetro BusType na saída do cmdlet mostrará corretamente o tipo de barramento como "SCM"

## <a name="checking-the-health-of-storage-class-memory"></a>Verificar a integridade da memória de classe do armazenamento
Para consultar a integridade da memória de classe de armazenamento, use os seguintes comandos em uma sessão do Windows PowerShell.

```powershell
PS C:\> Get-PhysicalDisk | where BusType -eq "SCM" | select SerialNumber, HealthStatus, OperationalStatus, OperationalDetails
```

Isso produz esta saída de exemplo:

|SerialNumber|HealthStatus|OperationalStatus|OperationalDetails|
|---|---|---|---|
|802c-01-1602-117cb5fc|Íntegro|OK||
|802c-01-1602-117cb64f|Aviso|Falha preditiva|{Limite excedido, erro de NVDIMM\_N}|

> [!NOTE]
> Para encontrar a localização física de um dispositivo NVDIMM N especificado em um evento, na guia **Detalhes** do evento no Visualizador de Eventos, acesse **EventData** > **Local**. Observe que o Windows Server 2016 lista o local incorreto dos dispositivos de NVDIMM-N, mas isso foi corrigido no Windows Server, versão 1709.

Para ajudar a entender as várias condições de integridade, confira as seções a seguir.

## <a name="warning-health-status"></a>Status de integridade de "Aviso"

Esta condição é quando você verifica a integridade de um dispositivo de memória de classe de armazenamento e vê que o status de integridade dele está listado como **Aviso**, conforme mostrado nesta saída de exemplo:

|SerialNumber|HealthStatus|OperationalStatus|OperationalDetails|
|---|---|---|---|
|802c-01-1602-117cb5fc|Íntegro|OK||
|802c-01-1602-117cb64f|Aviso|Falha preditiva|{Limite excedido, erro de NVDIMM\_N}|

A tabela a seguir lista algumas informações sobre essa condição.

||Descrição|
|---|---|
|Condição provável|Violação do limite de aviso de NVDIMM-N|
|Causa raiz|Os dispositivos NVDIMM-N controlam vários limites, como temperatura, tempo de vida de NVM e/ou tempo de vida de fonte de energia. Quando um desses limites é excedido, o sistema operacional é notificado.|
|Comportamento geral|O dispositivo permanece totalmente operacional. Este é um aviso, não um erro.|
|Comportamento dos Espaços de Armazenamento|O dispositivo permanece totalmente operacional. Este é um aviso, não um erro.|
|Mais informações|Campo OperationalStatus do objeto PhysicalDisk. EventLog – Microsoft-Windows-ScmDisk0101/Operational|
|O que fazer|Dependendo do limite de aviso violado, pode ser prudente substituir todo ou algumas partes do NVDIMM-N. Por exemplo, se o limite de tempo de vida NVM for ultrapassado, faz sentido substituir o NVDIMM-N.|

## <a name="writes-to-an-nvdimm-n-fail"></a>Falha ao gravar um NVDIMM-N

Esta condição ocorre quando você verifica a integridade de um dispositivo de memória de classe de armazenamento e vê que o status de integridade está listado como **Não íntegro** e o status operacional menciona um **Erro de E/S**, conforme mostrado nesta saída de exemplo:

|SerialNumber|HealthStatus|OperationalStatus|OperationalDetails|
|---|---|---|---|
|802c-01-1602-117cb5fc|Íntegro|OK||
|802c-01-1602-117cb64f|Unhealthy|{Metadados obsoletos, erro de E/S, erro temporário}|{Perda da persistência de dados, perda de dados, NV...}|

A tabela a seguir lista algumas informações sobre essa condição.

||Descrição|
|---|---|
|Condição provável|Perda de persistência/alimentação de backup|
|Causa raiz|Os dispositivos NVDIMM-N dependem de uma fonte de alimentação de backup para sua persistência, normalmente uma bateria ou supercapacitor. Se essa fonte de alimentação de backup não estiver disponível ou o dispositivo não puder executar um backup por algum motivo (erro de controlador/Flash), os dados estarão em risco e o Windows impedirá gravações adicionais nos dispositivos afetados. Ainda é possível realizar leituras para remover dados.|
|Comportamento geral|O volume NTFS será desmontado.<br>O campo de status de integridade do PhysicalDisk mostrará "Não íntegro" para todos os dispositivos NVDIMM-N afetados.|
|Comportamento dos Espaços de Armazenamento|O Espaço de Armazenamento permanecerá operacional contanto que apenas um NVDIMM-N seja afetado. Se vários dispositivos forem afetados, haverá falha nas gravações no Espaço de Armazenamento. <br>O campo de status de integridade do PhysicalDisk mostrará "Não íntegro" para todos os dispositivos NVDIMM-N afetados.|
|Mais informações|Campo OperationalStatus do objeto PhysicalDisk.<br>EventLog – Microsoft-Windows-ScmDisk0101/Operational|
|O que fazer|É recomendável fazer o backup dos dados afetados do NVDIMM-N. Para obter acesso de leitura, você pode manualmente colocar o disco online (a sua superfície será como um volume NTFS somente leitura).<br><br>Para limpar totalmente essa condição, a causa raiz deverá ser resolvida (ou seja, ligar a fonte de alimentação ou substituir o NVDIMM-N, dependendo do problema) e o volume no NVDIMM-N deverá ser colocado offline e online novamente ou o sistema deverá ser reiniciado.<br><br>Para tornar o NVDIMM-N utilizável novamente em Espaços de Armazenamento, use o cmdlet **Reset-PhysicalDisk**, que reintegra o dispositivo e inicia o processo de reparo.|

## <a name="nvdimm-n-is-shown-with-a-capacity-of-0-bytes-or-as-a-generic-physical-disk"></a>O NVDIMM-N é mostrado com uma capacidade de '0' bytes ou como um "Disco físico genérico"

Esta condição é quando um dispositivo de memória de classe de armazenamento é mostrado com uma capacidade de 0 bytes e não pode ser inicializado ou é exposto como um objeto de "Disco físico genérico" com um status operacional de **Comunicação Perdida**, conforme mostrado nesta saída de exemplo:

|SerialNumber|HealthStatus|OperationalStatus|OperationalDetails|
|---|---|---|---|
|802c-01-1602-117cb5fc|Íntegro|OK||
||Aviso|Comunicação perdida||

A tabela a seguir lista algumas informações sobre essa condição.

||Descrição|
|---|---|
|Condição provável|A BIOS não expôs o NVDIMM-N para o sistema operacional|
|Causa raiz|Os dispositivos NVDIMM-N são baseados em DRAM. Quando um endereço DRAM corrompido é referenciado, a maioria das CPUs iniciará uma verificação de máquina e reiniciará o servidor. Algumas plataformas de servidor em seguida mapeiam o NVDIMM, impedindo que o sistema operacional o acesse e possivelmente causando outra verificação de máquina. Isso também pode ocorrer se a BIOS detectar que o NVDIMM-N falhou e precisa ser substituído.|
|Comportamento geral|O NVDIMM-N é mostrado como não inicializado, com uma capacidade de 0 bytes e não pode ser lido ou gravado.|
|Comportamento dos Espaços de Armazenamento|O Espaço de Armazenamento permanece operacional (desde que apenas um NVDIMM-N seja afetado).<br>O Objeto PhysicalDisk do NVDIMM-N é mostrado com um Status de Integridade de Aviso e como um "Disco físico geral"|
|Mais informações|Campo OperationalStatus do objeto PhysicalDisk. <br>EventLog – Microsoft-Windows-ScmDisk0101/Operational|
|O que fazer|O dispositivo NVDIMM-N deve ser substituído ou limpo, de forma que a plataforma de servidor o exponha para o sistema operacional de host novamente. Recomenda-se substituir o dispositivo, pois poderão ocorrer erros incorrigíveis adicionais. Pode-se adicionar um dispositivo de substituição a uma configuração de espaços de armazenamento com o cmdlet **Add-Physicaldisk**.|

## <a name="nvdimm-n-is-shown-as-a-raw-or-empty-disk-after-a-reboot"></a>O NVDIMM-N é mostrado como RAW ou disco vazio após uma reinicialização

Esta condição é quando você verifica a integridade de um dispositivo de memória de classe de armazenamento e vê o status de integridade como **Não Íntegro** e o status operacional de **Metadados Não Reconhecidos**, conforme mostrado nesta saída de exemplo:

|SerialNumber|HealthStatus|OperationalStatus|OperationalDetails|
|---|---|---|---|
|802c-01-1602-117cb5fc|Íntegro|OK|{Desconhecido}|
|802c-01-1602-117cb64f|Unhealthy|{Metadados não reconhecidos, metadados obsoletos}|{Desconhecido}|

A tabela a seguir lista algumas informações sobre essa condição.

||Descrição|
|---|---|
|Condição provável|Falha de backup/restauração|
|Causa raiz|Uma falha no procedimento de backup ou restauração provavelmente resultará na perda de todos os dados do NVDIMM-N. Quando o sistema operacional for carregado, ele será exibido como um novo NVDIMM-N sem uma partição ou sistema de arquivos e uma superfície como RAW, o que significa que ele não tem um sistema de arquivos.|
|Comportamento geral|O NVDIMM-N estará em modo somente leitura. É necessária uma ação explícita do usuário para começar a usá-lo novamente.|
|Comportamento dos Espaços de Armazenamento|Os Espaços de Armazenamento permanecem operacionais se apenas um NVDIMM for afetado.<br>O objeto de disco físico NVDIMM-N será mostrado com o Status de Integridade "Não íntegro" e não é usado por Espaços de Armazenamento.|
|Mais informações|Campo OperationalStatus do objeto PhysicalDisk.<br>EventLog – Microsoft-Windows-ScmDisk0101/Operational|
|O que fazer|Se o usuário não quiser substituir o dispositivo afetado, ele poderá usar o cmdlet **Reset-PhysicalDisk** para limpar a condição somente leitura no NVDIMM-N afetado. Em ambientes de Espaços de Armazenamento, isso também tentará reintegrar o NVDIMM-N ao Espaço de Armazenamento e iniciar o processo de reparo.|

## <a name="interleaved-sets"></a>Conjuntos intercalados

Os conjuntos intercalados com frequência podem ser criados na BIOS de uma plataforma para fazer vários dispositivos NVDIMM-N aparecerem como um único dispositivo para o sistema operacional do host.

O Windows Server 2016 e o Windows 10 Anniversary Edition não oferecem suporte a conjuntos intercalados de NVDIMM-Ns.

No momento da criação deste artigo, não há nenhum mecanismo para o sistema operacional do host identificar corretamente NVDIMM-Ns individuais nesses conjuntos e comunicar claramente ao usuário que determinado dispositivo pode ter causado um erro ou precisa ser reparado.


