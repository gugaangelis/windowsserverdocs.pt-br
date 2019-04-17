---
ms.assetid: 87bca912-b912-4bbe-9533-2c34a7abc52d
title: "Determinando o número de domínios necessários"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b65c792929a29c959244d1da83b4882f15fa2935
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="determining-the-number-of-domains-required"></a>Determinando o número de domínios necessários

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cada floresta começa com um único domínio. O número máximo de usuários que pode conter uma floresta de domínio único baseia-se no link mais lento que deve acomodar a replicação entre controladores de domínio e a largura de banda disponível que você deseja alocar os serviços de domínio do Active Directory (AD DS). A tabela a seguir lista o máximo recomendado o número de usuários que pode conter um domínio com base em uma floresta de domínio único, a velocidade do link mais lento e a porcentagem de largura de banda que você deseja reservar para replicação. Essas informações se aplicam ao florestas que contêm um máximo de 100.000 usuários e que têm uma conectividade de 28,8 quilobits por segundo (Kbps) ou superior. Para obter recomendações que se aplicam a florestas que contêm mais de 100.000 usuários ou conectividade de menos de 28,8 Kbps, consulte um designer experiente do Active Directory. Os valores na tabela a seguir são baseados no tráfego replicação gerado em um ambiente que tem as seguintes características:  
  
-   Novos usuários ingressar na floresta a uma taxa de 20 por cento por ano.  
  
-   Os usuários sair da floresta com uma taxa de 15% por ano.  
  
-   Cada usuário é um membro de cinco grupos globais e cinco grupos universais.  
  
-   A proporção de usuários em computadores é 1:1.  
  
-   Active Directory integrado sistema DNS (Domain Name) é usado.  
  
-   A eliminação de DNS é usado.  
  
> [!NOTE]  
> Os valores listados na tabela a seguir são aproximações. A quantidade de tráfego de replicação depende amplamente o número de alterações feitas no diretório em um determinado período de tempo. Confirme que sua rede pode acomodar o tráfego de replicação Testando a quantidade estimada e a taxa de alterações no design do seu em um laboratório antes de implantar seus domínios.  
  
|Link mais lento, conectando-se um controlador de domínio (Kbps)|Número máximo de usuários se a largura de banda de 1 por cento está disponível|Número máximo de usuários se a largura de banda de 5% está disponível|Número máximo de usuários se a largura de banda de 10% está disponível|  
|--------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------|-----------------------------------------------------------------|  
|28.8|10,000|25,000|40,000|  
|32|10,000|25,000|50,000|  
|56|10,000|50,000|100,000|  
|64|10,000|50,000|100,000|  
|128|25,000|100,000|100,000|  
|256|50,000|100,000|100,000|  
|512|80,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  
  
Para usar esta tabela:  
  
1.  No **link mais lento, conectando-se um controlador de domínio** coluna, localize o valor que corresponde a velocidade do link mais lento através da qual o AD DS replicará em seu domínio.  
  
2.  Na linha que corresponde à sua velocidade do link mais lenta, localize a coluna que representa a porcentagem de largura de banda que você quiser alocar no AD DS. O valor nesse local é o número máximo de usuários no domínio na floresta de um único domínio pode conter.  
  
Se você determinar que o número total de usuários na floresta é menor que o número máximo de usuários que pode conter seu domínio, você pode usar um único domínio. Certifique-se de acomodar crescimento futuro planejada quando tomar essa decisão. Se você determinar que o número total de usuários na floresta é maior do que o número máximo de usuários que pode conter seu domínio, você precisa para reservar um maior percentual da largura de banda para replicação, aumentar a velocidade do link ou dividir sua organização em domínios regionais.  
  
## <a name="dividing-the-organization-into-regional-domains"></a>Dividir a organização em domínios regionais  
Se você não poderá acomodar todos os seus usuários em um único domínio, você deve selecionar o modelo de domínio regional. Divida sua organização em regiões de forma que faça sentido para a organização e sua rede existente. Por exemplo, você pode criar regiões com base nos limites continentais.  
  
Observe que, porque você precisa criar um domínio do Active Directory para cada região que você estabelece, nós recomendamos que você reduza o número de regiões que você define para o AD DS. Embora seja possível incluir um número ilimitado de domínios em uma floresta, para o gerenciamento do recomendamos uma floresta incluir não mais do que 10 domínios. Você deve estabelecer o equilíbrio apropriado entre otimizando a largura de banda de replicação e reduzindo sua complexidade administrativa ao dividir sua organização em domínios regionais.  
  
Primeiro, determine o número máximo de usuários pode hospedar floresta. Baseie no link na floresta mais lento em replicarão quais controladores de domínio e a quantidade média de largura de banda que deve ser alocada para replicação do Active Directory. A tabela a seguir lista o máximo recomendado o número de usuários que pode conter uma floresta. Isso depende da velocidade do link mais lento e a porcentagem de largura de banda que você deseja reservar para replicação. Essas informações se aplicam ao florestas que contêm um máximo de 100.000 usuários e que têm uma conectividade de 28,8 Kbps ou superior. Os valores na tabela a seguir são baseados nas seguintes considerações:  
  
-   Todos os controladores de domínio são servidores de catálogo global.  
  
-   Novos usuários ingressar na floresta a uma taxa de 20 por cento por ano.  
  
-   Os usuários sair da floresta com uma taxa de 15% por ano.  
  
-   Os usuários são membros dos grupos globais cinco e cinco grupos universais.  
  
-   A proporção de usuários em computadores é 1:1.  
  
-   DNS do Active Directory integrado é usado.  
  
-   A eliminação de DNS é usado.  
  
> [!NOTE]  
> Os valores listados na tabela a seguir são aproximações. A quantidade de tráfego de replicação depende amplamente o número de alterações feitas no diretório em um determinado período de tempo. Confirme que sua rede pode acomodar o tráfego de replicação Testando a quantidade estimada e a taxa de alterações no design do seu em um laboratório antes de implantar seus domínios.  
  
|Link mais lento, conectando-se um controlador de domínio (Kbps)|Número máximo de usuários se a largura de banda de 1 por cento está disponível|Número máximo de usuários se a largura de banda de 5% está disponível|Número máximo de usuários se a largura de banda de 10% está disponível|  
|--------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------|-----------------------------------------------------------------|  
|28.8|10,000|50,000|75,000|  
|32|10,000|50,000|75,000|  
|56|10,000|75,000|100,000|  
|64|25,000|75,000|100,000|  
|128|50,000|100,000|100,000|  
|256|75,000|100,000|100,000|  
|512|100,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  
  
Para usar esta tabela:  
  
1.  No **link mais lento, conectando-se um controlador de domínio** coluna, localize o valor que corresponde a velocidade do link mais lento através da qual o AD DS replicará na floresta.  
  
2.  Na linha que corresponde à sua velocidade do link mais lenta, localize a coluna que representa a porcentagem de largura de banda que você quiser alocar no AD DS. O valor nesse local é o número máximo de usuários pode hospedar floresta.  
  
Se o número máximo de usuários pode hospedar floresta for maior que o número de usuários que você precisa para hospedar, uma única floresta funcionará para seu design. Se você precisar hospedar mais usuários do que o número máximo que você identificou, você precisa para aumentar a velocidade do link mínima, alocar um percentual da largura de banda maior para o AD DS, ou implantar florestas adicionais.  
  
Se você determinar que uma única floresta poderá acomodar o número de usuários que você precisa para hospedar, a próxima etapa é determinar o número máximo de usuários que pode dar suporte a cada região com base no link mais lento localizado na região. Divida floresta em regiões que façam sentido para você. Certifique-se de que você basear sua decisão sobre algo que não é a probabilidade de mudar. Por exemplo, use continentes em vez de regiões de vendas. Essas regiões serão a base de sua estrutura de domínio quando você ter identificado o número máximo de usuários.  
  
Determine o número de usuários que precisam ser hospedados em cada região e, em seguida, verifique se que eles não ultrapassem o máximo permitido com base na mais baixa velocidade do link e a largura de banda alocada no AD DS na região. A tabela a seguir lista o máximo recomendado o número de usuários que pode conter um domínio regional. Ele se baseia a velocidade do link mais lento e a porcentagem de largura de banda que desejar reservar para replicação. Essas informações se aplicam ao florestas que contêm um máximo de 100.000 usuários e que têm uma conectividade de 28,8 Kbps ou superior. Os valores na tabela a seguir são baseados nas seguintes considerações:  
  
-   Todos os controladores de domínio são servidores de catálogo global.  
  
-   Novos usuários ingressar na floresta a uma taxa de 20 por cento por ano.  
  
-   Os usuários sair da floresta com uma taxa de 15% por ano.  
  
-   Os usuários são membros dos grupos globais cinco e cinco grupos universais.  
  
-   A proporção de usuários em computadores é 1:1.  
  
-   DNS do Active Directory integrado é usado.  
  
-   A eliminação de DNS é usado.  
  
> [!NOTE]  
> Os valores listados na tabela a seguir são aproximações. A quantidade de tráfego de replicação depende amplamente o número de alterações feitas no diretório em um determinado período de tempo. Confirme que sua rede pode acomodar o tráfego de replicação Testando a quantidade estimada e a taxa de alterações no design do seu em um laboratório antes de implantar seus domínios.  
  
|Link mais lento, conectando-se um controlador de domínio (Kbps)|Número máximo de usuários se a largura de banda de 1 por cento está disponível|Número máximo de usuários se a largura de banda de 5% está disponível|Número máximo de usuários se a largura de banda de 10% está disponível|  
|--------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------|-----------------------------------------------------------------|  
|28.8|10,000|18,000|40,000|  
|32|10,000|20,000|50,000|  
|56|10,000|40,000|100,000|  
|64|10,000|50,000|100,000|  
|128|15,000|100,000|100,000|  
|256|30,000|100,000|100,000|  
|512|80,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  
  
Para usar esta tabela:  
  
1.  No **link mais lento, conectando-se um controlador de domínio** coluna, localize o valor que corresponde a velocidade do link mais lento através da qual o AD DS replicará em sua região.  
  
2.  Na linha que corresponde à sua velocidade do link mais lenta, localize a coluna que representa a porcentagem de largura de banda que você quiser alocar no AD DS. Esse valor representa o número máximo de usuários pode hospedar a região.  
  
Avalie cada região proposta e determinar se o número máximo de usuários em cada região é menor que o número máximo recomendado de usuários que pode conter um domínio. Se você determinar que a região pode hospedar o número de usuários que você precisa, você pode criar um domínio para essa região. Se você determinar que você não pode hospedar que muitos usuários, considere dividir seu design em regiões menores e recalcular o número máximo de usuários que podem ser hospedados em cada região. As outras alternativas são para alocar mais largura de banda ou aumentar a velocidade do link.  
  
Embora o número total de usuários que podem ser colocadas em um domínio em uma floresta de vários domínios é menor que o número de usuários no domínio na floresta de um único domínio, o número total de usuários na floresta de vários domínios pode ser maior. O menor número de usuários por domínio em uma floresta de vários domínios adequa a sobrecarga de replicação adicionais criada por manter o catálogo global nesse ambiente. Para obter recomendações que se aplicam a florestas que contêm mais de 100.000 usuários ou conectividade de menos de 28,8 Kbps, consulte um designer experiente do Active Directory.  
  
## <a name="documenting-the-regions-identified"></a>Documentar as regiões identificadas  
Depois que você divida sua organização em domínios regionais, documento representadas as regiões que você deseja e o número de usuários que existirá em cada região. Além disso, observe a velocidade dos links mais lentos em cada região que você usará para replicação do Active Directory. Essas informações são usadas para determinar se os domínios adicionais ou florestas são necessárias.  
  
Para uma planilha para ajudá-lo a documentar as regiões que você identificou, baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip).  
  


