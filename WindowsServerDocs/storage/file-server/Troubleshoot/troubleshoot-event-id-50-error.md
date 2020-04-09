---
title: Solucionar problemas da mensagem de erro 50 do ID do evento
description: Descreve como solucionar problemas da mensagem de erro 50 do ID do evento
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 7ce3551b60450a3720c9350b5c55f396368490c1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815229"
---
# <a name="troubleshoot-the-event-id-50-error-message"></a>Solucionar problemas da mensagem de erro 50 do ID do evento

##  <a name="symptoms"></a>Sintomas

Quando as informações estão sendo gravadas no disco físico, as duas mensagens de evento a seguir podem ser registradas no log de eventos do sistema: 

```
Event ID: 50 
Event Type: Warning 
Event Source: Ftdisk 
Description: {Lost Delayed-Write Data} The system was attempting to transfer file data from buffers to \Device\HarddiskVolume4. The write operation failed, and only some of the data may have been written to the file.
Data: 
0000: 00 00 04 00 02 00 56 00 
0008: 00 00 00 00 32 00 04 80 
0010: 00 00 00 00 00 00 00 00 
0018: 00 00 00 00 00 00 00 00 
0020: 00 00 00 00 00 00 00 00 
0028: 11 00 00 80 
```

```
Event ID: 26 
Event Type: Information
Event Source: Application Popup
Description: Windows - Delayed Write Failed : Windows was unable to save all the data for the file \Device\HarddiskVolume4\Program Files\Microsoft SQL Server\MSSQL$INSTANCETWO\LOG\ERRORLOG. The data has been lost. This error may be caused by a failure of your computer hardware or network connection.

Please try to save this file elsewhere.
```

Essas mensagens de ID de evento significam exatamente a mesma coisa e são geradas pelos mesmos motivos. Para os fins deste artigo, somente a mensagem ID do evento 50 é descrita.

> [!NOTE] 
> O dispositivo e o caminho na descrição e os dados hexadecimais específicos irão variar. 

##  <a name="more-information"></a>Mais informações

Uma mensagem de ID de evento 50 será registrada se ocorrer um erro genérico quando o Windows estiver tentando gravar informações no disco. Esse erro ocorre quando o Windows está tentando confirmar dados do Gerenciador de cache do sistema de arquivos (não o cache de nível de hardware) para o disco físico. Esse comportamento faz parte do gerenciamento de memória do Windows. Por exemplo, se um programa envia uma solicitação de gravação, a solicitação de gravação é armazenada em cache pelo Gerenciador de cache e o programa é informado de que a gravação foi concluída com êxito. Em um momento posterior, o Gerenciador de cache tenta gravar os dados em tempo lento no disco físico. Quando o Gerenciador de cache tenta confirmar os dados no disco, ocorre um erro ao gravar os dados, e os dados são liberados do cache e descartados. O cache write-back melhora o desempenho do sistema, mas a perda de dados e a perda de integridade do volume podem ocorrer como resultado de falhas de gravação atrasadas perdidas.

É importante lembrar que nem toda e/s é e/s armazenada em buffer pelo Gerenciador de cache. Os programas podem definir um sinalizador de FILE_FLAG_NO_BUFFERING que ignora o Gerenciador de cache. Quando o SQL executa gravações críticas em um banco de dados, esse sinalizador é definido para garantir que a transação seja concluída diretamente no disco. Por exemplo, gravações não críticas em arquivos de log executam e/s em buffer para melhorar O desempenho geral. Uma mensagem de ID de evento 50 nunca resulta de e/s não armazenada em buffer.

Há várias fontes diferentes para uma mensagem de ID de evento 50. Por exemplo, uma mensagem de ID de evento 50 registrada de uma origem MRxSmb ocorrerá se houver um problema de conectividade de rede com o redirecionador. Para evitar a execução de etapas de solução de problemas incorretas, certifique-se de examinar a mensagem ID do evento 50 para confirmar se ela se refere a um problema de e/s de disco e se este artigo se aplica.

Uma mensagem de ID de evento 50 é semelhante a uma mensagem ID de evento 9 e a ID de evento 11. Embora o erro não seja tão sério quanto o erro indicado pela ID de evento 9 e uma mensagem de ID de evento 11, você pode usar as mesmas técnicas de solução de problemas para uma mensagem de ID de evento 50 como você faz para uma mensagem de identificação de evento 9 e de ID de evento 11. No entanto, lembre-se de que qualquer coisa na pilha pode causar gravações com atrasos perdidos, como drivers de filtro e drivers de mini portas. 

Você pode usar os dados binários associados a qualquer erro de "disco" que acompanha (indicado por uma mensagem de erro de ID de evento 9, 11, 51 ou outras mensagens) para ajudá-lo a identificar o problema.

###  <a name="how-to-decode-the-data-section-of-an-event-id-50-event-message"></a>Como decodificar a seção de dados de uma mensagem de evento de ID de evento 50 

Ao decodificar a seção de dados no exemplo de uma mensagem ID de evento 50 incluída na seção "Resumo", você verá que a tentativa de executar uma operação de gravação falhou porque o dispositivo estava ocupado e os dados foram perdidos. Esta seção descreve como decodificar essa mensagem de ID de evento 50. 

A tabela a seguir descreve o que cada deslocamento dessa mensagem representa: 

|OffsetLengthValues|Duração|Valores|
|-----------|------------|---------|
|0x00|2|Não usado|
|0x02|2|Tamanho de dados de despejo = 0x0004|
|0x04|2|Número de cadeias de caracteres = 0x0002|
|0x06|2|Deslocamento para as cadeias de caracteres|
|0x08|2|Categoria do Evento|
|0x0c|4|Código de erro NTSTATUS = 0x80040032 = IO_LOST_DELAYED_WRITE|
|0x10|8|Não usado|
|0x18|8|Não usado|
|0x20|8|Não usado|
|0x28|4|Código de erro de status do NT|

#### <a name="key-sections-to-decode"></a>Principais seções a serem decodificadas

**O código de erro**

No exemplo na seção "Resumo", o código de erro é listado na segunda linha. Essa linha começa com "0008:" e inclui os últimos quatro bytes nesta linha: 0008:00 00 00 00 32 00 04 80 nesse caso, o código de erro é 0x80040032. O código a seguir é o código para o erro 50 e é o mesmo para todas as mensagens da ID de evento 50: IO_LOST_DELAYED_WRITEWARNINGNote quando você estiver convertendo os dados hexadecimais na mensagem ID do evento para o código de status, lembre-se de que os valores são representados no formato little-endian.

**O disco de destino**

Você pode identificar o disco no qual a gravação estava sendo tentada usando o link simbólico listado para a unidade na seção "Descrição" da mensagem de ID do evento, por exemplo: \Device\HarddiskVolume4. Para obter informações adicionais sobre como identificar a unidade, clique no número de artigo a seguir para exibir o artigo na base de dados de conhecimento Microsoft: [159865](/EN-US/help/159865) como distinguir um dispositivo de disco físico de uma mensagem de evento

**O código de status final**

O código de status final é a informação mais importante em uma mensagem de ID de evento 50. Esse é o código de erro que é retornado quando a solicitação de e/s foi feita, e é a fonte de informações chave. No exemplo na seção "Resumo", o código de status final é listado em 0x28, a sexta linha, que começa com "0028:" e inclui os únicos quatro octetos nesta linha: 

```
0028: 11 00 00 80 
```

Nesse caso, o status final é igual a 0x80000011. Esse código de status é mapeado para STATUS_DEVICE_BUSY e implica que o dispositivo está ocupado no momento.

>[!NOTE] 
> Quando você estiver convertendo os dados hexadecimais na mensagem ID do evento 50 para o código de status, lembre-se de que os valores são representados no formato little-endian. Como o código de status é a única informação de que você está interessado, pode ser mais fácil exibir os dados em formato de palavras em vez de BYTES. Se você fizer isso, os bytes estarão no formato correto e os dados poderão ser mais fáceis de interpretar rapidamente.

Para fazer isso, clique em **palavras** na janela **Propriedades do evento** . Na exibição de palavras de dados, o exemplo na seção "sintomas" seria lido da seguinte maneira: dados: 

```
() Bytes (.) 
Words 0000: 00040000 00560002 00000000 80040032 0010: 00000000 00000000 00000000 00000000 0020: 00000000 00000000 80000011
```

Para obter uma lista de códigos de status do Windows NT, consulte NTSTATUS. H no SDK (Software Developers Kit) do Windows.