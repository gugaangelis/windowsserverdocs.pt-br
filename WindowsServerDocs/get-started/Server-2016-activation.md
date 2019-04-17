---
title: Ativação dos servidores da Microsoft
description: Como ativar o Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 09/19/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a45d5cc7a54
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 79f15c4c9c635138ae74a61fe1c259b97717155e
ms.sourcegitcommit: d622f7af181ed0063d716b30278d41887a57db19
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/08/2019
ms.locfileid: "9150846"
---
# Ativação do Windows Server 2016

As informações a seguir descrevem as considerações de planejamento iniciais que você deve analisar para a ativação dos Serviços de Gerenciamento de Chaves (KMS) envolvendo o Windows Server 2016. Para obter informações sobre a ativação do KMS envolvendo os sistemas operacionais mais antigos do que os listados aqui, consulte [Etapa 1: Examine e selecione métodos de ativação](https://technet.microsoft.com/library/jj134256(WS.11).aspx).

O KMS usa um modelo cliente-servidor para ativar clientes. Os clientes KMS se conectam a um servidor KMS, chamado de host KMS, para ativação. O host KMS deve residir na rede local.

Hosts KMS não precisam ser servidores dedicados e o KMS pode ser hospedado juntamente com outros serviços. Um host KMS pode ser executado em qualquer sistema físico ou virtual que esteja executando o Windows 10, Windows Server 2016, Windows Server 2012 R2, Windows 8.1 ou Windows Server 2012.

Um host KMS em execução no Windows10 ou no Windows 8.1 pode ativar apenas computadores que executem sistemas operacionais clientes.
A tabela a seguir resume os requisitos do cliente e host KMS para redes que incluam clientes Windows Server 2016 e Windows 10.

>[!NOTE]
>**Observação:**  Podem ser necessárias atualizações no servidor KMS para dar suporte à ativação de qualquer um desses clientes mais recentes. Se você receber erros de ativação, verifique se tem as atualizações adequadas listadas abaixo desta tabela.

|Grupo da chave do produto (Product Key)|KMS pode ser hospedado em|Edições do Windows ativadas por este host KMS|  
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|  
|Licença de volume para o Windows Server 2016|Windows Server 2012<br /><br />Windows Server 2012 R2<br /><br />Windows Server 2016<br /><br />|Canal semestral do Windows Server <br><br>Windows Server 2016 (todas as edições)<br /><br />Windows 10 LTSB (2015 e 2016)<br /><br />Windows 10 Professional<br /><br />Windows 10 Enterprise<br /><br />Windows 10 Pro for Workstations<br><br>Windows 10 Education<br><br>Windows Server 2012 R2 (todas as edições)<br /><br />Windows 8.1 Professional<br /><br />Windows 8.1 Enterprise<br /><br />Windows Server 2012 (todas as edições)<br /><br />Windows Server 2008 R2 (todas as edições)<br /><br />Windows Server 2008 (todas as edições)<br /><br />Windows 7 Professional<br /><br />Windows 7 Enterprise| 
|Licença de volume para o Windows 10|Windows 7<br /><br />Windows 8.1<br /><br /> Windows 10|Windows 10 Professional<br /><br /> Windows 10 Professional N<br /><br /> Windows 10 Enterprise<br /><br /> Windows 10 Enterprise N<br /><br /> Windows 10 Education<br /><br /> Windows 10 Education N<br /><br /> Windows 10 Enterprise LTSB (2015)<br /><br /> Windows 10 Enterprise LTSB N (2015)<br /><br /> Windows 10 Pro for Workstations<br><br>Windows 8.1 Professional<br /><br /> Windows 8.1 Enterprise<br /><br /> Windows 7 Professional<br /><br /> Windows 7 Enterprise<br /><br />|  
|Licença de volume para “Windows Server 2012 R2 para Windows 10”|Windows Server 2008 R2<br /><br /> Windows Server 2012 Standard<br /><br /> Windows Server 2012 Datacenter<br /><br /> Windows Server 2012 R2 Standard<br /><br />Windows Server 2012 R2 Datacenter|Windows 10 Professional<br /><br /> Windows 10 Enterprise<br /><br />Windows 10 Enterprise LTSB (2015)<br><br>Windows 10 Pro for Workstations<br><br>Windows 10 Education<br><br> Windows Server 2012 R2 (todas as edições)<br /><br /> Windows 8.1 Professional<br /><br /> Windows 8.1 Enterprise<br /><br /> Windows Server 2012 (todas as edições)<br /><br /> Windows Server 2008 R2 (todas as edições)<br /><br />Windows Server 2008 (todas as edições)<br /><br /> Windows 7 Professional<br /><br /> Windows 7 Enterprise|

> [!NOTE]  
>Dependendo do sistema operacional no qual o servidor KMS está sendo executado e os sistemas operacionais que deseja ativar, talvez seja necessário instalar uma ou mais dessas atualizações:
>- A instalação do KMS no Windows 7 ou no Windows Server 2008 R2 deve ser atualizada para dar suporte à ativação de clientes que executem o Windows 10. Para obter mais informações, consulte[Update que permite que o Windows 7 e hosts KMS do Windows Server 2008 R2 ativem o Windows 10](https://support.microsoft.com/help/3079821/update-that-enables-windows-7-and-windows-server-2008-r2-kms-hosts-to-activate-windows-10).  
>- A instalação do KMS no Windows Server 2012 deve ser atualizada para dar suporte à ativação dos clientes que executam o Windows 10 e o Windows Server 2016, ou sistemas operacionais de cliente ou servidor mais recentes. Para obter mais informações, consulte[julho de 2016 cumulativo de atualizações para o Windows Server 2012](https://support.microsoft.com/help/3172615/july-2016-update-rollup-for-windows-server-2012). 
>- A instalação do KMS no Windows 8.1 ou Windows Server 2012 R2 deve ser atualizada para dar suporte à ativação dos clientes que executam o Windows 10 e o Windows Server 2016, ou sistemas operacionais de cliente ou servidor mais recentes. Para obter mais informações, consulte[julho de 2016 cumulativo de atualizações para Windows 8.1 e Windows Server 2012 R2](https://support.microsoft.com/help/3172614/july-2016-update-rollup-for-windows-8.1-and-windows-server-2012-r2).  
>- O Windows Server2008 R2 não pode ser atualizado para dar suporte à ativação de clientes que executam o Windows Server 2016, ou sistemas operacionais mais recentes. 

Um único host KMS pode dar suporte a um número ilimitado de clientes KMS. Se tiver mais de 50 clientes, recomendamos que você tenha pelo menos dois hosts KMS, para o caso de um deles ficar indisponível. A maioria das organizações pode operar com um mínimo de dois hosts KMS para toda a infraestrutura.

# Lidando com os requisitos operacionais do KMS
O KMS pode ativar computadores físicos e virtuais, mas, para se qualificar para uma ativação KMS, a rede deve ter um número mínimo de computadores físicos (o que chamamos de limite de ativação). Clientes KMS só são ativados depois que esse limite é atingido. Para garantir o cumprimento do limite de ativação, um host KMS conta o número de computadores que estão solicitando ativação na rede.

Os hosts KMS contam as conexões mais recentes. Quando um cliente ou servidor contata o host KMS, o host adiciona a ID do computador à sua contagem e, em seguida, retorna o valor atual da contagem em sua resposta. O cliente ou servidor será ativado se a contagem for alta o suficiente. Os clientes serão ativados se a contagem for 25 ou maior. Os servidores e as edições de volume dos produtos do Microsoft Office serão ativados se a contagem for cinco ou maior. O KMS conta apenas conexões únicas dos últimos 30 dias e armazena apenas os 50 contatos mais recentes.

As ativações KMS são válidas por 180 dias, um período conhecido como o intervalo de validade de ativação. Clientes KMS devem renovar suas ativações e, para isso, devem se conectar ao host KMS pelo menos uma vez a cada 180 dias para preservar a ativação. Por padrão, os computadores clientes KMS tentam renovar suas ativações a cada sete dias. Depois de renovada a ativação de um cliente, o intervalo de validade da ativação é reiniciado.

# Lidando com os requisitos funcionais do KMS

A ativação de KMS exige conectividade TCP/IP. Hosts e clientes KMS são configurados, por padrão, para usar DNS (Sistema de Nomes de Domínio). Por padrão, os hosts KMS usam a atualização dinâmica de DNS para publicar automaticamente as informações de que os clientes KMS precisam para localizar e se conectar. Você pode aceitar essas configurações padrão ou, caso tenha requisitos especiais de configuração de rede e segurança, pode configurar manualmente os hosts e clientes KMS.

Depois que o primeiro host KMS é ativado, é possível usar a chave KMS usada no primeiro host para ativar até cinco outros hosts KMS na rede. Após a ativação de um host KMS, os administradores podem usar a mesma chave para reativar o mesmo host nove vezes.

Se a organização precisar de mais de seis hosts KMS, solicite ativações adicionais para a chave KMS da organização – por exemplo, quando dez locais físicos são regidos por um único contrato de licenciamento por volume e você quer um host KMS local em cada um deles.

>[!NOTE] 
>Para solicitar essa exceção, contate o Call Center de Ativação. Para obter mais informações, consulte [Licenciamento por Volume da Microsoft]( https://www.microsoft.com/licensing).

Computadores que executam edições de licenciamento por volume do Windows 10, Windows Server 2016, Windows 8.1, Windows Server 2012 R2, Windows Server 2012, Windows 7, Windows Server 2008 R2 são, por padrão, clientes KMS sem a necessidade de configuração adicional.

Caso esteja convertendo um computador por meio de um host KMS, MAK ou edição comercial do Windows em um cliente KMS, instale a Chave de Instalação de Cliente KMS. Para obter mais informações, consulte as[Chaves de instalação de cliente KMS](KMSclientkeys.md). 
