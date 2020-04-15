---
title: Ativação do Microsoft Server
description: Como ativar o Windows Server 2016.
ms.prod: windows-server
ms.date: 09/19/2018
ms.technology: server-general
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a45d5cc7a54
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: fd9ea63785e8de313d2177113a466fa67c17410b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80826639"
---
# <a name="windows-server-2016-activation"></a>Ativação do Windows Server 2016

As informações a seguir descrevem as considerações iniciais de planejamento que você deve analisar para a ativação do KMS (Serviços de Gerenciamento de Chaves) envolvendo o Windows Server 2016. Para obter informações sobre a ativação do KMS envolvendo sistemas operacionais mais antigos do que os listados aqui, consulte [Etapa 1: Analisar e selecionar métodos de ativação](https://technet.microsoft.com/library/jj134256(WS.11).aspx).

O KMS usa um modelo cliente-servidor para ativar clientes. Os clientes KMS se conectam a um servidor KMS, chamado de host KMS, para ativação. O host KMS deve residir na rede local.

Hosts KMS não precisam ser servidores dedicados e o KMS pode ser hospedado juntamente com outros serviços. Um host KMS pode ser executado em qualquer sistema físico ou virtual que esteja executando o Windows 10, Windows Server 2016, Windows Server 2012 R2, Windows 8.1 ou Windows Server 2012.

Um host KMS em execução no Windows 10 ou no Windows 8.1 pode ativar apenas computadores que executem sistemas operacionais clientes.
A tabela a seguir resume os requisitos do cliente e host KMS para redes que incluam clientes Windows Server 2016 e Windows 10.

> [!NOTE]
> Podem ser necessárias atualizações no servidor KMS para dar suporte à ativação de qualquer um desses clientes mais recentes. Se você receber erros de ativação, verifique se tem as atualizações adequadas listadas abaixo desta tabela.

|Grupo da chave do produto (Product Key)|O KMS pode ser hospedado em|Edições do Windows ativadas por este host KMS|  
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|  
|Licença de volume para Windows Server 2016|Windows Server 2012<p>Windows Server 2012 R2<p>Windows Server 2016<p>|Windows Server Canal Semestral <br><br>Windows Server 2016 (todas as edições)<p>Windows 10 LTSB (2015 e 2016)<p>Windows 10 Professional<p>Windows 10 Enterprise<p>Windows 10 Pro for Workstations<br><br>Windows 10 Education<br><br>Windows Server 2012 R2 (todas as edições)<p>Windows 8.1 Professional<p>Windows 8.1 Enterprise<p>Windows Server 2012 (todas as edições)<p>Windows Server 2008 R2 (todas as edições)<p>Windows Server 2008 (todas as edições)<p>Windows 7 Professional<p>Windows 7 Enterprise| 
|Licença de volume para Windows 10|Windows 7<p>Windows 8.1<p> Windows 10|Windows 10 Professional<p> Windows 10 Professional N<p> Windows 10 Enterprise<p> Windows 10 Enterprise N<p> Windows 10 Education<p> Windows 10 Education N<p> Windows 10 Enterprise LTSB (2015)<p> Windows 10 Enterprise LTSB N (2015)<p> Windows 10 Pro for Workstations<br><br>Windows 8.1 Professional<p> Windows 8.1 Enterprise<p> Windows 7 Professional<p> Windows 7 Enterprise<p>|  
|Licença de volume para Windows Server 2012 R2 para Windows 10|Windows Server 2008 R2<p> Windows Server 2012 Standard<p> Windows Server 2012 Datacenter<p> Windows Server 2012 R2 Standard<p>Windows Server 2012 R2 Datacenter|Windows 10 Professional<p> Windows 10 Enterprise<p>Windows 10 Enterprise LTSB (2015)<br><br>Windows 10 Pro for Workstations<br><br>Windows 10 Education<br><br> Windows Server 2012 R2 (todas as edições)<p> Windows 8.1 Professional<p> Windows 8.1 Enterprise<p> Windows Server 2012 (todas as edições)<p> Windows Server 2008 R2 (todas as edições)<p>Windows Server 2008 (todas as edições)<p> Windows 7 Professional<p> Windows 7 Enterprise|

> [!NOTE]  
> Dependendo do sistema operacional no qual o servidor KMS está sendo executado e os sistemas operacionais que deseja ativar, talvez seja necessário instalar uma ou mais dessas atualizações:
> - As instalações do KMS no Windows 7 ou no Windows Server 2008 R2 devem ser atualizadas para dar suporte à ativação de clientes que executam o Windows 10. Para obter mais informações, consulte  [Atualização que permite que hosts KMS do Windows 7 e Windows Server 2008 R2 ativem o Windows 10](https://support.microsoft.com/help/3079821/update-that-enables-windows-7-and-windows-server-2008-r2-kms-hosts-to-activate-windows-10).  
> - As instalações do KMS no Windows Server 2012 devem ser atualizadas para dar suporte à ativação dos clientes que executam o Windows 10 e o Windows Server 2016, ou sistemas operacionais de cliente ou servidor mais recentes. Para obter mais informações, consulte  [Pacote cumulativo de atualizações de julho de 2016 para Windows Server 2012](https://support.microsoft.com/help/3172615/july-2016-update-rollup-for-windows-server-2012). 
> - As instalações do KMS no Windows 8.1 ou no Windows Server 2012 R2 devem ser atualizadas para dar suporte à ativação dos clientes que executam o Windows 10 e o Windows Server 2016, ou sistemas operacionais de cliente ou servidor mais recentes. Para obter mais informações, consulte  [Pacote cumulativo de atualizações de julho de 2016 para Windows 8.1 e Windows Server 2012 R2](https://support.microsoft.com/help/3172614/july-2016-update-rollup-for-windows-8.1-and-windows-server-2012-r2).  
> - O Windows Server 2008 R2 não pode ser atualizado para dar suporte à ativação de clientes que executam o Windows Server 2016 ou sistemas operacionais mais recentes. 

Um único host KMS pode dar suporte a um número ilimitado de clientes KMS. Se você tiver mais de 50 clientes, recomendamos que tenha pelo menos dois hosts KMS para o caso de um deles ficar indisponível. A maioria das organizações pode operar com um mínimo de dois hosts KMS para toda a infraestrutura.

## <a name="addressing-kms-operational-requirements"></a>Lidando com os requisitos operacionais do KMS
O KMS pode ativar computadores físicos e virtuais, mas, para se qualificar para uma ativação KMS, a rede deve ter um número mínimo de computadores físicos (o que chamamos de limite de ativação). Clientes KMS só são ativados depois que esse limite é atingido. Para garantir o cumprimento do limite de ativação, um host KMS conta o número de computadores que estão solicitando ativação na rede.

Os hosts KMS contam as conexões mais recentes. Quando um cliente ou servidor contata o host KMS, o host adiciona a ID do computador à sua contagem e, em seguida, retorna o valor atual da contagem em sua resposta. O cliente ou servidor será ativado se a contagem for alta o suficiente. Os clientes serão ativados se a contagem for 25 ou maior. Os servidores e as edições de volume dos produtos do Microsoft Office serão ativado se a contagem for cinco ou maior. O KMS conta apenas conexões únicas dos últimos 30 dias e armazena apenas os 50 contatos mais recentes.

As ativações KMS são válidas por 180 dias, um período conhecido como o intervalo de validade de ativação. Clientes KMS devem renovar suas ativações e, para isso, devem se conectar ao host KMS pelo menos uma vez a cada 180 dias para preservar a ativação. Por padrão, os computadores clientes KMS tentam renovar suas ativações a cada sete dias. Depois que a ativação de um cliente for renovada, o intervalo de validade da ativação será reiniciado.

## <a name="addressing-kms-functional-requirements"></a>Lidando com os requisitos funcionais do KMS

A ativação de KMS exige conectividade TCP/IP. Hosts e clientes KMS são configurados, por padrão, para usar DNS (Sistema de Nomes de Domínio). Por padrão, os hosts KMS usam a atualização dinâmica de DNS para publicar automaticamente as informações que os clientes KMS precisam localizar e conectar a eles. Você pode aceitar essas configurações padrão ou, caso tenha requisitos especiais de configuração de rede e segurança, pode configurar manualmente os hosts e clientes KMS.

Depois que o primeiro host KMS é ativado, é possível usar a chave KMS usada no primeiro host para ativar até cinco outros hosts KMS na rede. Após a ativação de um host KMS, os administradores podem usar a mesma chave para reativar o mesmo host nove vezes.

Se a organização precisar de mais de seis hosts KMS, você deverá solicitar ativações adicionais para a chave KMS da organização – por exemplo, se você tiver dez locais físicos em um contrato de licenciamento por volume e quiser que cada local tenha um host KMS local.

> [!NOTE] 
> Para solicitar essa exceção, contate o Call Center de Ativação. Para obter mais informações, consulte [Microsoft Volume Licensing]( https://www.microsoft.com/licensing).

Computadores que executam edições de licenciamento por volume do Windows 10, Windows Server 2016, Windows 8.1, Windows Server 2012 R2, Windows Server 2012, Windows 7, Windows Server 2008 R2 são, por padrão, clientes KMS sem necessidade de configuração adicional.

Caso esteja convertendo um computador por meio de um host KMS, MAK ou edição comercial do Windows em um cliente KMS, instale a Chave de Instalação de Cliente KMS. Para obter mais informações, consulte  [Chaves de instalação do cliente KMS](KMSclientkeys.md).
