---
title: Rede de alto desempenho
description: Este tópico fornece uma visão geral das tecnologias de descarregamento e otimização no Windows Server 2016 e inclui links para diretrizes adicionais sobre essas tecnologias.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: 5cf6a5057151c696bc1c29a1dcf6fc18c776605a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405746"
---
## <a name="hardware-only-ho-features-and-technologies"></a>Tecnologias e recursos do hardware apenas (HO)

Essas acelerações de hardware melhoram o desempenho de rede em conjunto com o software, mas não fazem parte profunda de qualquer recurso de software. Exemplos desses incluem moderação de interrupção, controle de fluxo e descarregamento de soma de verificação de IPv4 no lado do recebimento.

>[!TIP]
>Os recursos SH e HO estarão disponíveis se a NIC instalada der suporte a ele. As descrições de recurso a seguir abordarão como saber se a NIC dá suporte ao recurso.

### <a name="address-checksum-offload"></a>Descarregamento de soma de verificação de endereço

Os descarregamentos de soma de verificação de endereço são um recurso NIC que descarrega o cálculo das somas de verificação de endereço (IP, TCP, UDP) para o hardware NIC para enviar e receber.

No caminho de recebimento, o descarregamento de soma de verificação calcula as somas de verificação nos cabeçalhos IP, TCP e UDP (conforme apropriado) e indica ao sistema operacional se as somas de verificação passaram, falharam ou não foram verificadas. Se a NIC afirmar que as somas de verificação são válidas, o sistema operacional aceita o pacote não desafiado. Se a NIC afirmar que as somas de verificação são inválidas ou não marcadas, a pilha de IP/TCP/UDP calcula internamente as somas de verificação novamente. Se a soma de verificação computada falhar, o pacote será Descartado.

No caminho de envio, o descarregamento de soma de verificação calcula e insere as somas de verificação no cabeçalho IP, TCP ou UDP, conforme apropriado.

Desabilitar descarregamentos de soma de verificação no caminho de envio não desabilita o cálculo da soma de verificação e a inserção de pacotes enviados para o driver de miniporta usando o recurso LSO (descarregamento de envio grande).  Para desabilitar todos os cálculos de descarregamento de soma de verificação, o usuário também deve desabilitar o LSO.

_**Gerenciar descarregamentos de soma de verificação de endereço**_

Nas propriedades avançadas, há várias propriedades distintas:

-   Descarregamento de soma de verificação IPv4

-   Descarregamento de soma de verificação TCP (IPv4)

-   Descarregamento de soma de verificação TCP (IPv6)

-   Descarregamento de soma de verificação UDP (IPv4)

-   Descarregamento de soma de verificação UDP (IPv6)

Por padrão, todos estão sempre habilitados. Recomendamos sempre habilitar todos esses descarregamentos.

Os descarregamentos de soma de verificação podem ser gerenciados usando os cmdlets Enable-NetAdapterChecksumOffload e Disable-NetAdapterChecksumOffload. Por exemplo, o cmdlet a seguir habilita os cálculos de soma de verificação TCP (IPv4) e UDP (IPv4):

```PowerShell
Enable-NetAdapterChecksumOffload –Name * -TcpIPv4 -UdpIPv4
```

_**Dicas sobre como usar descarregamentos de soma de verificação de endereço**_

Os descarregamentos de soma de verificação de endereço sempre devem ser habilitados, independentemente da carga de trabalho ou circunstância. Esse mais básico de todas as tecnologias de descarregamento sempre melhora o desempenho da rede. O descarregamento de soma de verificação também é necessário para que outros descarregamentos sem monitoração de estado funcionem, incluindo o recebimento de escala lateral (RSS), a União de segmento de recebimento (RSC) e o LSO (carregamento de envio grande).

### <a name="interrupt-moderation-im"></a>Moderação de interrupção (IM)

O IM armazena em buffer vários pacotes recebidos antes de interromper o sistema operacional. Quando uma NIC recebe um pacote, ele inicia um temporizador. Quando o buffer estiver cheio ou o temporizador expirar, o que vier primeiro, a NIC interromperá o sistema operacional. 

Muitas NICs dão suporte a mais do que apenas ligado/desligado para moderação da interrupção. A maioria das NICs dá suporte aos conceitos de uma taxa baixa, média e alta para mensagens instantâneas. As diferentes taxas representam temporizadores mais curtos ou longos e ajustes de tamanho de buffer apropriados para reduzir a latência (moderação de baixa interrupção) ou reduzir interrupções (alta moderação de interrupção).

Há um equilíbrio a ser riscado entre a redução de interrupções e o atraso excessivo da entrega de pacotes. Geralmente, o processamento de pacotes é mais eficiente com a moderação de interrupção habilitada. Aplicativos de alto desempenho ou de baixa latência podem precisar avaliar o impacto de desabilitar ou reduzir a moderação da interrupção.

### <a name="jumbo-frames"></a>Quadros jumbo

Os quadros jumbo são um recurso de NIC e rede que permite que um aplicativo envie quadros muito maiores do que os 1500 bytes padrão. Normalmente, o limite de quadros Jumbo é de cerca de 9000 bytes, mas pode ser menor.

Não houve alterações no suporte a quadros Jumbo no Windows Server 2012 R2.

No Windows Server 2016, há um novo descarregamento: MTU_for_HNV. Esse novo descarregamento funciona com configurações de quadro Jumbo para garantir que o tráfego encapsulado não exija segmentação entre o host e a opção adjacente. Esse novo recurso da pilha SDN tem a NIC que calcula automaticamente qual MTU deve ser anunciado e qual MTU usar na conexão. Esses valores para MTU serão diferentes se qualquer descarregamento HNV estiver em uso. (Na tabela de compatibilidade de recursos, a tabela 1, MTU_for_HNV teria as mesmas interações que os descarregamentos de HNVv2, já que ele está diretamente relacionado aos descarregamentos de HNVv2.)

### <a name="large-send-offload-lso"></a>LSO (Descarregamento de Envio Grande)

O LSO permite que um aplicativo transmita um grande bloco de dados para a NIC, e a NIC quebra os dados em pacotes que se encaixam na MTU (unidade máxima de transferência) da rede.

### <a name="receive-segment-coalescing-rsc"></a>Receive Segment Coalescing (RSC)

A União de segmento de recebimento, também conhecida como descarga de recebimento grande, é um recurso NIC que usa pacotes que fazem parte do mesmo fluxo que chega entre interrupções de rede e os une em um único pacote antes de fornecê-los ao sistema operacional. O RSC não está disponível em NICs associadas ao comutador virtual Hyper-V. Para obter mais informações, consulte [RSC (União de segmento de recebimento)](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch).