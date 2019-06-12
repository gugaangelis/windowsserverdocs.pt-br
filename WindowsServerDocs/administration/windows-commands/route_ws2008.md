---
title: route_ws2008
description: Saiba como modificar e exibir as entradas na tabela de roteamento IP local.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afcd666c-0cef-47c2-9bcc-02d202b983b3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 30843fe94ac7a4dc60092adcede60120bc9e627f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441763"
---
# <a name="routews2008"></a>route_ws2008

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe e modifica as entradas na tabela de roteamento IP local. Usado sem parâmetros, **rota** exibe a Ajuda.   

## <a name="syntax"></a>Sintaxe  
```  
route [/f] [/p] [<Command> [<Destination>] [mask <Netmask>] [<Gateway>] [metric <Metric>]] [if <Interface>]]  
```  

### <a name="parameters"></a>Parâmetros  

|Parâmetro|Descrição|  
|-------|--------|  
|/f|Limpa a tabela de roteamento de todas as entradas que não são rotas de host (rotas com uma máscara de rede de 255.255.255.255), a rota de rede de loopback (rotas com um destino de 127.0.0.0 e uma máscara de rede de 255.0.0.0) ou uma rota de multicast (rotas com um destino de 224.0.0.0 e uma máscara de rede de 240.0.0.0). Se isso for usado em conjunto com um dos comandos (tais como adicionar, alterar ou excluir), a tabela é limpo antes de executar o comando.|  
|/p|Quando usado com o comando Adicionar, a rota especificada é adicionada ao registro e é usada para inicializar a tabela de roteamento IP sempre que o protocolo TCP/IP é iniciado. Por padrão, as rotas adicionadas não são preservadas quando o protocolo TCP/IP é iniciado. Quando usado com o comando print, a lista de rotas persistentes é exibida. Esse parâmetro é ignorado para todos os outros comandos. Rotas persistentes são armazenadas no local de registro **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\PersistentRoutes**.|  
|\<Comando >|Especifica o comando que você deseja executar. A tabela a seguir lista os comandos válidos:<br /><br />-   **Adicionar:** adiciona uma rota.<br />-   **alterar:** modifica uma rota existente.<br />-   **Excluir:** exclui um ou mais rotas.<br />-   **Imprimir:** imprime uma ou mais rotas.|  
|\<Destino >|Especifica o destino de rede da rota. O destino pode ser um endereço de rede IP (onde os bits de host do endereço de rede são definidos como 0), um endereço IP para uma rota de host ou 0.0.0.0 para a rota padrão.|  
|mask \<Netmask>|Especifica o destino de rede da rota. O destino pode ser um endereço de rede IP (onde os bits de host do endereço de rede são definidos como 0), um endereço IP para uma rota de host ou 0.0.0.0 para a rota padrão.|  
|\<Gateway>|Especifica o endereço IP de salto encaminhamento ou Avançar sobre o qual o conjunto de endereços definidos pela máscara de destino e a sub-rede de rede está acessível. Para rotas de sub-rede conectada localmente, o endereço do gateway é o endereço IP atribuído à interface que está conectada à sub-rede. Para rotas remotas, disponíveis em um ou mais roteadores, o endereço do gateway é um endereço IP diretamente acessível que é atribuído a um roteador vizinho.|  
|métrica \<métrica >|Especifica uma medida de custo inteiro (variando de 1 a 9999) para a rota, que é usada ao escolher entre várias rotas na tabela de roteamento que melhor corresponder ao endereço de destino de um pacote que está sendo encaminhado. A rota com a métrica mais baixa será escolhida. A métrica pode refletir o número de saltos, a velocidade do caminho, confiabilidade, taxa de transferência do caminho ou propriedades administrativas.|  
|if \<Interface>|Especifica o índice de interface para a interface em que o destino está acessível. Para obter uma lista de interfaces e os índices correspondentes, use a exibição do comando route print. Você pode usar valores decimais ou hexadecimais para o índice de interface. Para valores hexadecimais, preceda o número hexadecimal com 0x. Quando o se o parâmetro for omitido, a interface é determinada do endereço de gateway.|  
|/?|Exibe a ajuda no prompt de comando.|  

## <a name="remarks"></a>Comentários  
- Valores grandes na **métrica** coluna da tabela de roteamento são o resultado de permitir que o TCP/IP determinar automaticamente a métrica de rotas na tabela de roteamento com base na configuração do endereço IP, máscara de sub-rede e gateway padrão para cada interface de LAN. Determinação automática da métrica da interface, habilitada por padrão, determina a velocidade de cada interface e ajusta as métricas de rotas para cada interface para que a interface mais rápida cria as rotas com a métrica mais baixa. Para remover as métricas de grandes, desative o cálculo automático da métrica de interface das propriedades avançadas do protocolo TCP/IP para cada conexão de rede local.  
- Nomes podem ser usados para *destino* se houver uma entrada apropriada no arquivo redes local armazenado na <strong>systemroot\System32\Drivers\\</strong>pasta Etc. Nomes podem ser usados para o *gateway* desde que eles podem ser resolvidos para um endereço IP por meio de técnicas de resolução de nome de host padrão, como consultas de sistema de nome de domínio (DNS), usar o arquivo de Hosts local armazenado no  <strong>systemroot\system32\drivers\\</strong>pasta etc e a resolução de nomes NetBIOS.  
- Se o comando **imprimir** ou **excluir**, o *Gateway* parâmetro pode ser omitido e curingas podem ser usados para o destino e o gateway. O *destino* valor pode ser um valor de curinga especificado por um asterisco (*). Se o destino especificado contém um asterisco (\*) ou um ponto de interrogação (?), ele será tratado como um caractere curinga e somente rotas de destino correspondente são impressos ou excluídas. O asterisco corresponde a qualquer cadeia de caracteres e o ponto de interrogação corresponde a qualquer caractere único. Por exemplo, 10. \*.1, 192.168. \*, 127. \*, e \*224\* são usos válidos do curinga asterisco.  
- Usar uma combinação inválida de um valor de máscara (máscara de rede) de destino e a sub-rede exibe uma "rota: máscara de rede de endereço de gateway incorreto" mensagem de erro. Essa mensagem de erro aparece quando o destino contém um ou mais bits definidos como 1 em locais de bit em que o bit correspondente de máscara de sub-rede é definido como 0. Para testar essa condição, expressam a destino e máscara de sub-rede usando notação binária. A máscara de sub-rede na notação binária consiste em uma série de bits 1, que representa a parte do endereço de rede de destino e uma série de bits 0, que representa a parte do endereço de host do destino. Verifique se há bits no destino que são definidos como 1 para a parte de destino que é o endereço do host (conforme definido pela máscara de sub-rede).  
- O **/p** parâmetro só é compatível com o comando de rota para o Windows NT 4.0, Windows 2000, Windows Millennium edition, Windows XP e Windows Server 2003. Não há suporte para esse parâmetro o **rota** comando para o Windows 95 ou Windows 98.  
- Esse comando está disponível somente se o protocolo de Internet Protocol (TCP/IP) é instalado como um componente nas propriedades de um adaptador de rede em conexões de rede.  

## <a name="BKMK_Examples"></a>Exemplos  
Para exibir todo o conteúdo da tabela de roteamento IP, digite:  
```  
route print  
```  
Para exibir as rotas na tabela de roteamento IP que começam com 10, digite:  
```  
route print 10.*  
```  
Para adicionar uma rota padrão com o endereço de gateway padrão de 192.168.12.1, digite:  
```  
route add 0.0.0.0 mask 0.0.0.0 192.168.12.1  
```  
Para adicionar uma rota para o destino 10.41.0.0 com a máscara de sub-rede de 255.255.0.0 e o endereço do próximo salto de 10.27.0.1, digite:  
```  
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1  
```  
Para adicionar uma rota persistente ao destino 10.41.0.0 com a máscara de sub-rede de 255.255.0.0 e o endereço do próximo salto de 10.27.0.1, digite:  
```  
route /p add 10.41.0.0 mask 255.255.0.0 10.27.0.1  
```  
Para adicionar uma rota para o destino 10.41.0.0 com a máscara de sub-rede de 255.255.0.0, o endereço do próximo salto de 10.27.0.1 e a métrica de custo de 7, digite:  
```  
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1 metric 7  
```  
Para adicionar uma rota para o destino 10.41.0.0 com a máscara de sub-rede 255.255.0.0, o endereço do próximo salto de 10.27.0.1 e o índice de interface 0x3, digite:  
```  
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1 if 0x3  
```  
Para excluir a rota para o destino 10.41.0.0 com a máscara de sub-rede 255.255.0.0, digite:  
```  
route delete 10.41.0.0 mask 255.255.0.0  
```  
Para excluir todas as rotas na tabela de roteamento de IP que começam com 10, digite:  
```  
route delete 10.*  
```  
Para alterar o endereço do próximo salto da rota com o destino de 10.41.0.0 e a máscara de sub-rede 255.255.0.0 de 10.27.0.1 para 10.27.0.25, digite:  
```  
route change 10.41.0.0 mask 255.255.0.0 10.27.0.25  
```  

## <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
