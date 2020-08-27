---
ms.assetid: 87bca912-b912-4bbe-9533-2c34a7abc52d
title: Determinando o número de domínios necessários
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/08/2018
ms.topic: article
ms.openlocfilehash: 8fe9e6d50bef530d50ba8e7a33432fe2613b871b
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939346"
---
# <a name="determining-the-number-of-domains-required"></a>Determinando o número de domínios necessários

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cada floresta começa com um único domínio. O número máximo de usuários que uma única floresta de domínio pode conter baseia-se no link mais lento que deve acomodar a replicação entre os controladores de domínio e a largura de banda disponível que você deseja alocar para Active Directory Domain Services (AD DS). A tabela a seguir lista o número máximo recomendado de usuários que um domínio pode conter com base em uma floresta de domínio único, a velocidade do link mais lento e a porcentagem de largura de banda que você deseja reservar para replicação. Essas informações se aplicam a florestas que contêm um máximo de 100.000 usuários e que têm uma conectividade de 28,8 kilobits por segundo (Kbps) ou mais. Para obter recomendações que se aplicam a florestas que contêm mais de 100.000 usuários ou conectividade de menos de 28,8 Kbps, consulte um designer de Active Directory experiente. Os valores na tabela a seguir se baseiam no tráfego de replicação gerado em um ambiente com as seguintes características:

- Novos usuários ingressam na floresta a uma taxa de 20% por ano.
- Os usuários deixam a floresta a uma taxa de 15% por ano.
- Cada usuário é membro de cinco grupos globais e cinco grupos universais.
- A taxa de usuários para computadores é 1:1.
- O DNS (sistema de nomes de domínio) integrado Active Directory é usado.
- A limpeza de DNS é usada.

> [!NOTE]
> As figuras listadas na tabela a seguir são as aproximações. A quantidade de tráfego de replicação depende muito do número de alterações feitas no diretório em um determinado período de tempo. Confirme se sua rede pode acomodar o tráfego de replicação testando a quantidade estimada e a taxa de alterações em seu design em um laboratório antes de implantar seus domínios.

|Link mais lento conectando um controlador de domínio (Kbps)|Número máximo de usuários se a largura de banda de 1% estiver disponível|Número máximo de usuários se a largura de banda de 5% estiver disponível|Número máximo de usuários se a largura de banda de 10% estiver disponível|
| --- | --- | --- | --- |
|28,8|10.000|25.000|40.000|
|32|10.000|25.000|50.000|
|56|10.000|50.000|100.000|
|64|10.000|50.000|100.000|
|128|25.000|100.000|100.000|
|256|50.000|100.000|100.000|
|512|80.000|100.000|100.000|
|1\.500|100.000|100.000|100.000|

Para usar esta tabela:

1. No **link mais lento conectando uma coluna de controlador de domínio** , localize o valor que corresponde à velocidade do link mais lento entre o qual AD DS será replicado em seu domínio.

2. Na linha que corresponde à sua velocidade de vínculo mais lenta, localize a coluna que representa a largura de banda percentual que você deseja alocar para AD DS. O valor nesse local é o número máximo de usuários que o domínio em uma floresta de domínio único pode conter.

Se você determinar que o número total de usuários em sua floresta é menor que o número máximo de usuários que seu domínio pode conter, você poderá usar um único domínio. Certifique-se de acomodar o crescimento planejado futuro ao fazer essa determinação. Se você determinar que o número total de usuários em sua floresta é maior que o número máximo de usuários que seu domínio pode conter, será necessário reservar um percentual mais alto de largura de banda para replicação, aumentar a velocidade do link ou dividir sua organização em domínios regionais.

## <a name="dividing-the-organization-into-regional-domains"></a>Dividindo a organização em domínios regionais

Se você não puder acomodar todos os seus usuários em um único domínio, deverá selecionar o modelo de domínio regional. Divida sua organização em regiões de uma maneira que faça sentido para sua organização e sua rede existente. Por exemplo, você pode criar regiões com base em limites continental.

Observe que, como você precisa criar um domínio Active Directory para cada região que você estabelecer, recomendamos minimizar o número de regiões que você define para AD DS. Embora seja possível incluir um número ilimitado de domínios em uma floresta, para capacidade de gerenciamento, recomendamos que uma floresta não contenha mais de 10 domínios. Você deve estabelecer o equilíbrio apropriado entre otimizar sua largura de banda de replicação e minimizar sua complexidade administrativa ao dividir sua organização em domínios regionais.

Primeiro, determine o número máximo de usuários que sua floresta pode hospedar. Baseie isso no link mais lento na floresta em que os controladores de domínio serão replicados e a quantidade média de largura de banda que você deseja alocar para Active Directory replicação. A tabela a seguir lista o número máximo recomendado de usuários que uma floresta pode conter. Isso se baseia na velocidade do link mais lento e na largura de banda percentual que você deseja reservar para replicação. Essas informações se aplicam a florestas que contêm um máximo de 100.000 usuários e que têm uma conectividade de 28,8 kbps ou superior. Os valores na tabela a seguir se baseiam nas seguintes suposições:

- Todos os controladores de domínio são servidores de catálogo global.
- Novos usuários ingressam na floresta a uma taxa de 20% por ano.
- Os usuários deixam a floresta a uma taxa de 15% por ano.
- Os usuários são membros de cinco grupos globais e cinco grupos universais.
- A taxa de usuários para computadores é 1:1.
- O DNS integrado ao Active Directory é usado.
- A limpeza de DNS é usada.

> [!NOTE]
> As figuras listadas na tabela a seguir são as aproximações. A quantidade de tráfego de replicação depende muito do número de alterações feitas no diretório em um determinado período de tempo. Confirme se sua rede pode acomodar o tráfego de replicação testando a quantidade estimada e a taxa de alterações em seu design em um laboratório antes de implantar seus domínios.

|Link mais lento conectando um controlador de domínio (Kbps)|Número máximo de usuários se a largura de banda de 1% estiver disponível|Número máximo de usuários se a largura de banda de 5% estiver disponível|Número máximo de usuários se a largura de banda de 10% estiver disponível|
| --- | --- | --- | --- |
|28,8|10.000|50.000|75.000|
|32|10.000|50.000|75.000|
|56|10.000|75.000|100.000|
|64|25.000|75.000|100.000|
|128|50.000|100.000|100.000|
|256|75.000|100.000|100.000|
|512|100.000|100.000|100.000|
|1\.500|100.000|100.000|100.000|

Para usar esta tabela:

1. No **link mais lento conectando uma coluna de controlador de domínio** , localize o valor que corresponde à velocidade do link mais lento entre o qual AD DS será replicado na floresta.

2. Na linha que corresponde à sua velocidade de vínculo mais lenta, localize a coluna que representa a largura de banda percentual que você deseja alocar para AD DS. O valor nesse local é o número máximo de usuários que sua floresta pode hospedar.

Se o número máximo de usuários que sua floresta pode hospedar for maior que o número de usuários que você precisa hospedar, uma única floresta funcionará para seu design. Se você precisar hospedar mais usuários do que o número máximo identificado, será necessário aumentar a velocidade mínima do link, alocar um percentual maior de largura de banda para AD DS ou implantar florestas adicionais.

Se você determinar que uma única floresta acomodará o número de usuários que precisa hospedar, a próxima etapa será determinar o número máximo de usuários aos quais cada região pode dar suporte com base no link mais lento localizado nessa região. Divida sua floresta em regiões que fazem sentido para você. Certifique-se de que você baseie sua decisão em algo que não é provavelmente alterado. Por exemplo, use continentes em vez de regiões de vendas. Essas regiões serão a base da sua estrutura de domínio quando você tiver identificado o número máximo de usuários.

Determine o número de usuários que precisam ser hospedados em cada região e, em seguida, verifique se eles não excedem o máximo permitido com base na velocidade de vínculo mais lenta e a largura de banda alocada para AD DS nessa região. A tabela a seguir lista o número máximo recomendado de usuários que um domínio regional pode conter. Ele se baseia na velocidade do link mais lento e na porcentagem de largura de banda que você deseja reservar para replicação. Essas informações se aplicam a florestas que contêm um máximo de 100.000 usuários e que têm uma conectividade de 28,8 kbps ou superior. Os valores na tabela a seguir se baseiam nas seguintes suposições:

- Todos os controladores de domínio são servidores de catálogo global.
- Novos usuários ingressam na floresta a uma taxa de 20% por ano.
- Os usuários deixam a floresta a uma taxa de 15% por ano.
- Os usuários são membros de cinco grupos globais e cinco grupos universais.
- A taxa de usuários para computadores é 1:1.
- O DNS integrado ao Active Directory é usado.
- A limpeza de DNS é usada.

> [!NOTE]
> As figuras listadas na tabela a seguir são as aproximações. A quantidade de tráfego de replicação depende muito do número de alterações feitas no diretório em um determinado período de tempo. Confirme se sua rede pode acomodar o tráfego de replicação testando a quantidade estimada e a taxa de alterações em seu design em um laboratório antes de implantar seus domínios.

|Link mais lento conectando um controlador de domínio (Kbps)|Número máximo de usuários se a largura de banda de 1% estiver disponível|Número máximo de usuários se a largura de banda de 5% estiver disponível|Número máximo de usuários se a largura de banda de 10% estiver disponível|
| --- | --- | --- | --- |
|28,8|10.000|18.000|40.000|
|32|10.000|20,000|50.000|
|56|10.000|40.000|100.000|
|64|10.000|50.000|100.000|
|128|15,000|100.000|100.000|
|256|30,000|100.000|100.000|
|512|80.000|100.000|100.000|
|1\.500|100.000|100.000|100.000|

Para usar esta tabela:

1. No **link mais lento conectando uma coluna de controlador de domínio** , localize o valor que corresponde à velocidade do link mais lento no qual AD DS será replicado em sua região.

2. Na linha que corresponde à sua velocidade de vínculo mais lenta, localize a coluna que representa a largura de banda percentual que você deseja alocar para AD DS. Esse valor representa o número máximo de usuários que a região pode hospedar.

Avalie cada região proposta e determine se o número máximo de usuários em cada região é menor do que o número máximo recomendado de usuários que um domínio pode conter. Se você determinar que a região pode hospedar o número de usuários que você precisa, poderá criar um domínio para essa região. Se você determinar que não pode hospedar muitos usuários, considere dividir seu design em regiões menores e recalcular o número máximo de usuários que podem ser hospedados em cada região. As outras alternativas são alocar mais largura de banda ou aumentar a velocidade do link.

Embora o número total de usuários que você pode colocar em um domínio em uma floresta de vários domínios seja menor do que o número de usuários no domínio em uma floresta de domínio único, o número total de usuários na floresta de vários domínios pode ser maior. O número menor de usuários por domínio em uma floresta de vários domínios acomoda a sobrecarga de replicação adicional criada mantendo o catálogo global nesse ambiente. Para obter recomendações que se aplicam a florestas que contêm mais de 100.000 usuários ou conectividade de menos de 28,8 Kbps, consulte um designer de Active Directory experiente.

## <a name="documenting-the-regions-identified"></a>Documentando as regiões identificadas

Depois de dividir sua organização em domínios regionais, documente as regiões que você deseja que sejam representadas e o número de usuários que existirão em cada região. Além disso, observe a velocidade dos links mais lentos em cada região que será usada para Active Directory replicação. Essas informações são usadas para determinar se domínios ou florestas adicionais são necessários.

Para uma planilha para ajudá-lo a documentar as regiões identificadas, baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de [ajudas de trabalho para o Windows Server 2003 Deployment Kit](https://microsoft.com/download/details.aspx?id=9608) e abra "identificando regiões" (DSSLOGI_4.doc).
