---
title: Rede de alto desempenho
description: Este tópico fornece uma visão geral do descarregamento e tecnologias de otimização no Windows Server 2016 e inclui links para diretrizes adicionais sobre essas tecnologias.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: 09e18a41452baa0add8e055ceb22d2f5c2ad886e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815827"
---
## <a name="hardware-only-ho-features-and-technologies"></a>Tecnologias e recursos do hardware apenas (HO)

Esses acelerações de hardware melhorar o desempenho de rede em conjunto com o software, mas intimamente não fazem parte de qualquer recurso de software. Exemplos disso incluem descarregamento de soma de verificação do lado de recebimento IPv4, controle de fluxo e moderação de interrupção.

>[!TIP]
>SH e HO recursos estarão disponíveis se a NIC instalada dá suporte a ele. As descrições de recurso abaixo abordará como saber se a NIC suporta o recurso.

### <a name="address-checksum-offload"></a>Descarregamento de soma de verificação de endereço

Descarregamento de soma de verificação de endereço é um recurso NIC que descarrega o cálculo de endereço somas de verificação (IP, TCP, UDP) para o hardware NIC para de enviarem e receberam.

No caminho de recebimento, o descarregamento de soma de verificação calcula as somas de verificação nos cabeçalhos de IP, TCP e UDP (conforme apropriado) e indica para o sistema operacional, se as somas de verificação foi aprovada, falharam ou não verificada. Se a NIC declara que as somas de verificação são válidas, que o sistema operacional aceita o pacote totalmente. Se a NIC declara que as somas de verificação são inválidos ou não verificado, a pilha TCP/IP/UDP internamente calcula as somas de verificação novamente. Se a soma de verificação computada falhar, o pacote é descartado.

No caminho de envio, o descarregamento de soma de verificação calcula e insere as somas de verificação no cabeçalho IP, TCP ou UDP conforme apropriado.

Desabilitar o descarregamento de soma de verificação no caminho de envio não desabilita o cálculo de soma de verificação e a inserção para pacotes enviados para o driver de miniporta usando o recurso Enviar LSO (descarregamento grande).  Para desabilitar todos os cálculos de descarregamento de soma de verificação, o usuário também deve desabilitar LSO.

_**Gerenciar o descarregamento de soma de verificação de endereço**_

Nas propriedades avançadas, há várias propriedades distintas:

-   Descarregamento de soma de verificação de IPv4

-   Descarregamento de soma de verificação TCP (IPv4)

-   Descarregamento de soma de verificação TCP (IPv6)

-   Descarregamento de soma de verificação UDP (IPv4)

-   UDP Checksum Offload (IPv6)

Por padrão, esses são todos sempre habilitados. É recomendável sempre habilitar todos esses descarregamentos.

Os descarregamentos de soma de verificação podem ser gerenciados usando os cmdlets NetAdapterChecksumOffload Enable e Disable-NetAdapterChecksumOffload. Por exemplo, o cmdlet a seguir permite que os cálculos de soma de verificação UDP (IPv4) e TCP (IPv4):

```PowerShell
Enable-NetAdapterChecksumOffload –Name * -TcpIPv4 -UdpIPv4
```

_**Dicas sobre como usar descarregamentos de soma de verificação de endereço**_

Descarrega de soma de verificação de endereço deve estar sempre ativado, independentemente de qual carga de trabalho ou circunstância. Isso descarregar mais básico de todas as tecnologias sempre melhoram o desempenho de rede. Descarregamento de soma de verificação também é necessário para outros descarregamentos sem estado para funcionam, incluindo o receive side scaling (RSS), receber a união de segmentos (RSC) e descarregamento de envio grande (LSO).

### <a name="interrupt-moderation-im"></a>(IM) de moderação de interrupção

Mensagens Instantâneas armazena em buffer vários pacotes recebidos antes de interromper o sistema operacional. Quando uma NIC recebe um pacote, ele inicia um cronômetro. Quando o buffer estiver cheio ou o timer expira, o que vier primeiro, a NIC interrompe o sistema operacional. 

Número de NICs dão suporte a mais de apenas ativar/desativar para moderação de interrupção. A maioria das NICs dão suporte os conceitos de uma baixa, média e alta taxa de mensagem instantânea. As taxas de diferentes representam maiores ou menores de temporizadores e ajustes de tamanho do buffer apropriado para reduzir a latência (moderação de interrupção baixa) ou reduzir interrupções (moderação de interrupção alta).

Há um equilíbrio para ser ignorado entre a redução de interrupções e excessivamente atrasar a entrega de pacotes. Em geral, processamento de pacotes é mais eficiente com moderação de interrupção habilitado. Aplicativos de baixa latência ou alto desempenho, talvez seja necessário avaliar o impacto da desabilitação ou reduzindo a moderação de interrupção.

### <a name="jumbo-frames"></a>Quadros jumbo

Quadros jumbo é um recurso NIC e de rede que permite que um aplicativo enviar quadros que são muito maiores do que o padrão de 1.500 bytes. Normalmente o limite de quadros jumbo é cerca de 9.000 bytes, mas pode ser menor.

Não houve nenhuma alteração para suporte a quadros jumbo no Windows Server 2012 R2.

No Windows Server 2016, que há uma nova descarga: MTU_for_HNV. Esse novo descarregamento funciona com as configurações de quadros Jumbo para garantir que o tráfego encapsulado não exige a segmentação entre o host e o comutador adjacente. Esse novo recurso da pilha de SDN tem a NIC calcular automaticamente quais MTU para anunciar e quais MTU para usar durante a transmissão. Esses valores para MTU são diferentes se nenhum descarregamento da HNV está em uso. (Na tabela de compatibilidade de recurso, a tabela 1, MTU_for_HNV teria as mesmas interações conforme descarrega o HNVv2 descarregamentos têm, pois ele está diretamente relacionado ao HNVv2.)

### <a name="large-send-offload-lso"></a>LSO (Descarregamento de Envio Grande)

LSO permite que um aplicativo passar um grande bloco de dados para a NIC e as quebras de NIC os dados em pacotes que se encaixam dentro de transferir unidade máxima (MTU) da rede.

### <a name="receive-segment-coalescing-rsc"></a>Receive Segment Coalescing (RSC)

União de segmentos Receive, também conhecido como grandes receber descarregamento, é um recurso NIC que usa pacotes que fazem parte do mesmo fluxo que chega entre interrupções de rede e une-os em um único pacote antes de distribuí-los para o sistema operacional. A RSC não está disponível nas NICs que estão associadas ao comutador Virtual Hyper-V. Para obter mais informações, consulte [receber segmento de união (RSC)](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch).