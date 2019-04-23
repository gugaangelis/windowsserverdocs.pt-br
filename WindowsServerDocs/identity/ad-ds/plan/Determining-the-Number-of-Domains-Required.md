---
ms.assetid: 87bca912-b912-4bbe-9533-2c34a7abc52d
title: Determinando o número de domínios necessários
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e5d6369ca91c1d48375c22238d1e706576df7fbf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843517"
---
# <a name="determining-the-number-of-domains-required"></a>Determinando o número de domínios necessários

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Todas as florestas começa com um único domínio. O número máximo de usuários que uma floresta de domínio único pode conter baseia-se no link mais lento que deve se adaptar a replicação entre controladores de domínio e a largura de banda disponível que você deseja alocar para os serviços de domínio do Active Directory (AD DS). A tabela a seguir lista o número máximo recomendado de usuários que contém um domínio com base em uma floresta de domínio único, a velocidade do link mais lenta e a porcentagem de largura de banda que você deseja reservar para a replicação. Essas informações se aplicam às florestas que contêm um máximo de 100.000 usuários e que têm uma conectividade de 28,8 Kbps por segundo (Kbps) ou superior. Para obter recomendações que se aplicam às florestas que contêm mais de 100.000 usuários ou conectividade de menos de 28,8 Kbps, consulte um designer experiente do Active Directory. Os valores na tabela a seguir se baseiam no tráfego de replicação gerado em um ambiente que tem as seguintes características:  
  
- Novos usuários ingressar na floresta em uma taxa de 20 por cento por ano.  
- Os usuários deixam a floresta em uma taxa de 15 por cento por ano.  
- Cada usuário é um membro dos grupos globais de cinco e cinco grupos universais.  
- A proporção de usuários de computadores é 1:1.  
- Active Directory integrado domínio nome DNS (sistema) é usado.  
- Eliminação de DNS é usado.  

> [!NOTE]  
> Os valores listados na tabela a seguir são aproximações. A quantidade de tráfego de replicação depende em grande parte o número de alterações feitas no diretório em uma determinada quantidade de tempo. Confirme se sua rede pode acomodar o tráfego de replicação Testando a quantidade prevista e a taxa de alterações em seu design em um laboratório antes de implantar seus domínios.  
  
|Link mais lento se conectar a um controlador de domínio (Kbps)|Número máximo de usuários, se houver largura de banda de 1 por cento|Número máximo de usuários, se houver largura de banda de 5 por cento|Número máximo de usuários, se houver largura de banda de 10 por cento|  
| --- | --- | --- | --- |  
|28.8|10,000|25,000|40,000|  
|32|10,000|25,000|50,000|  
|56|10,000|50,000|100,000|  
|64|10,000|50,000|100,000|  
|128|25,000|100,000|100,000|  
|256|50,000|100,000|100,000|  
|512|80,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  

Para usar esta tabela:  

1. No **link mais lento se conectar a um controlador de domínio** coluna, localize o valor que corresponde a velocidade do link mais lenta em que o AD DS serão replicadas em seu domínio.  

2. Na linha que corresponde à sua velocidade mais lenta do link, localize a coluna que representa a porcentagem de largura de banda que deseja alocar para o AD DS. O valor nesse local é o número máximo de usuários que o domínio em uma floresta de domínio único pode conter.  

Se você determinar que o número total de usuários em sua floresta é menor que o número máximo de usuários que contém seu domínio, você pode usar um único domínio. Certifique-se acomodar o crescimento futuro planejado quando você faz essa determinação. Se você determinar que o número total de usuários em sua floresta é maior que o número máximo de usuários que contém seu domínio, você precisa reservar uma porcentagem maior de largura de banda para replicação, aumentar a velocidade do link ou dividir sua organização em domínios regionais.  
  
## <a name="dividing-the-organization-into-regional-domains"></a>Dividindo a organização em domínios regionais

Se você não puder acomodar todos os seus usuários em um único domínio, você deve selecionar o modelo de domínio regional. Divida a sua organização em regiões de forma que faça sentido para sua organização e sua rede existente. Por exemplo, você pode criar regiões com base em limites continental.  
  
Observe que como você precisa criar um domínio do Active Directory para cada região que você estabelecer, recomendamos que você minimize o número de regiões que você define para o AD DS. Embora seja possível incluir um número ilimitado de domínios em uma floresta, para capacidade de gerenciamento é recomendável que uma floresta incluir domínios não mais do que 10. Você deve estabelecer o equilíbrio apropriado entre otimizando a largura de banda de replicação e minimizando a complexidade administrativa ao dividir a sua organização em domínios regionais.  
  
Primeiro, determine o número máximo de usuários que pode hospedar a sua floresta. Base no link mais lento na floresta em quais controladores de domínio serão replicadas e a quantidade média de largura de banda que você deseja alocar para replicação do Active Directory. A tabela a seguir lista o número máximo recomendado de usuários que uma floresta pode conter. Isso se baseia na velocidade do link mais lento e a porcentagem de largura de banda que você deseja reservar para a replicação. Essas informações se aplicam às florestas que contêm um máximo de 100.000 usuários e que têm uma conectividade de 28,8 Kbps ou superior. Os valores na tabela a seguir baseiam-se nas seguintes suposições:  

- Todos os controladores de domínio são servidores de catálogo global.  
- Novos usuários ingressar na floresta em uma taxa de 20 por cento por ano.  
- Os usuários deixam a floresta em uma taxa de 15 por cento por ano.  
- Os usuários são membros de grupos globais de cinco e cinco grupos universais.  
- A proporção de usuários de computadores é 1:1.  
- DNS integrado ao Active Directory é usado.  
- Eliminação de DNS é usado.  

> [!NOTE]  
> Os valores listados na tabela a seguir são aproximações. A quantidade de tráfego de replicação depende em grande parte o número de alterações feitas no diretório em uma determinada quantidade de tempo. Confirme se sua rede pode acomodar o tráfego de replicação Testando a quantidade prevista e a taxa de alterações em seu design em um laboratório antes de implantar seus domínios.  
  
|Link mais lento se conectar a um controlador de domínio (Kbps)|Número máximo de usuários, se houver largura de banda de 1 por cento|Número máximo de usuários, se houver largura de banda de 5 por cento|Número máximo de usuários, se houver largura de banda de 10 por cento|  
| --- | --- | --- | --- |  
|28.8|10,000|50,000|75,000|  
|32|10,000|50,000|75,000|  
|56|10,000|75,000|100,000|  
|64|25,000|75,000|100,000|  
|128|50,000|100,000|100,000|  
|256|75,000|100,000|100,000|  
|512|100,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  

Para usar esta tabela:  

1. No **link mais lento se conectar a um controlador de domínio** coluna, localize o valor que corresponde a velocidade do link mais lenta em que o AD DS serão replicadas na sua floresta.  

2. Na linha que corresponde à sua velocidade mais lenta do link, localize a coluna que representa a porcentagem de largura de banda que você deseja alocar para o AD DS. O valor nesse local é o número máximo de usuários que pode hospedar a sua floresta.  

Se o número máximo de usuários que pode hospedar a sua floresta for maior que o número de usuários que você precisa para hospedar, uma única floresta funcionará para seu design. Se você precisa para hospedar mais usuários do que o número máximo que você identificou, você precisa para aumentar a velocidade mínima do link, aloque um percentual maior largura de banda para o AD DS, ou implantar florestas adicionais.  

Se você determinar que uma única floresta poderá acomodar o número de usuários que você precisa para hospedar, a próxima etapa é determinar o número máximo de usuários que pode dar suporte a cada região com base no link mais lento localizado nessa região. Divida sua floresta em regiões que façam sentido para você. Certifique-se de que você baseie sua decisão sobre algo que não provavelmente será alterado. Por exemplo, use continentes em vez de regiões de vendas. Essas regiões será a base para a estrutura de domínio quando tiver identificado o número máximo de usuários.  

Determine o número de usuários que precisam ser hospedados em cada região e, em seguida, verifique se que eles não excedam o máximo permitido com base na velocidade do link mais lenta e a largura de banda alocada para o AD DS nessa região. A tabela a seguir lista o número máximo recomendado de usuários que um domínio regional pode conter. Ele se baseia a velocidade do link mais lenta e a porcentagem de largura de banda que você deseja reservar para replicação. Essas informações se aplicam às florestas que contêm um máximo de 100.000 usuários e que têm uma conectividade de 28,8 Kbps ou superior. Os valores na tabela a seguir baseiam-se nas seguintes suposições:  

- Todos os controladores de domínio são servidores de catálogo global.  
- Novos usuários ingressar na floresta em uma taxa de 20 por cento por ano.  
- Os usuários deixam a floresta em uma taxa de 15 por cento por ano.  
- Os usuários são membros de grupos globais de cinco e cinco grupos universais.  
- A proporção de usuários de computadores é 1:1.  
- DNS integrado ao Active Directory é usado.  
- Eliminação de DNS é usado.  
  
> [!NOTE]  
> Os valores listados na tabela a seguir são aproximações. A quantidade de tráfego de replicação depende em grande parte o número de alterações feitas no diretório em uma determinada quantidade de tempo. Confirme se sua rede pode acomodar o tráfego de replicação Testando a quantidade prevista e a taxa de alterações em seu design em um laboratório antes de implantar seus domínios.  
  
|Link mais lento se conectar a um controlador de domínio (Kbps)|Número máximo de usuários, se houver largura de banda de 1 por cento|Número máximo de usuários, se houver largura de banda de 5 por cento|Número máximo de usuários, se houver largura de banda de 10 por cento|  
| --- | --- | --- | --- |  
|28.8|10,000|18,000|40,000|  
|32|10,000|20,000|50,000|  
|56|10,000|40,000|100,000|  
|64|10,000|50,000|100,000|  
|128|15,000|100,000|100,000|  
|256|30,000|100,000|100,000|  
|512|80,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  

Para usar esta tabela:  

1. No **link mais lento se conectar a um controlador de domínio** coluna, localize o valor que corresponde a velocidade do link mais lenta em que o AD DS serão replicadas em sua região.  

2. Na linha que corresponde à sua velocidade mais lenta do link, localize a coluna que representa a porcentagem de largura de banda que você deseja alocar para o AD DS. Esse valor representa o número máximo de usuários que a região pode hospedar.  

Avalie cada região proposto e determinar se o número máximo de usuários em cada região é menor que o número máximo recomendado de usuários que um domínio pode conter. Se você determinar que a região pode hospedar o número de usuários que você precisa, você pode criar um domínio para essa região. Se você determinar que não é possível hospedar que muitos usuários, considere dividir seu design em regiões menores e recalcular o número máximo de usuários que podem ser hospedados em cada região. As outras alternativas são para alocar mais largura de banda ou aumentar a velocidade do link.  

Embora o número total de usuários que você pode colocar em um domínio em uma floresta de vários domínios é menor do que o número de usuários no domínio em uma floresta de domínio único, o número total de usuários na floresta de vários domínios pode ser maior. O menor número de usuários por domínio em uma floresta de vários domínios acomoda a sobrecarga de replicação adicional criada pela manutenção de catálogo global nesse ambiente. Para obter recomendações que se aplicam às florestas que contêm mais de 100.000 usuários ou conectividade de menos de 28,8 Kbps, consulte um designer experiente do Active Directory.  
  
## <a name="documenting-the-regions-identified"></a>Documentar as regiões identificadas

Depois que você divida sua organização em domínios regionais, documento representadas as regiões que você deseja e o número de usuários que existirá em cada região. Além disso, observe a velocidade dos vínculos mais lentas em cada região em que você usará para replicação do Active Directory. Essa informação é usada para determinar se as florestas ou domínios adicionais são necessárias.  

Para uma planilha ajudar a documentar as regiões que você identificou, baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip da [trabalho auxílios para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558) e abra " Identificação de regiões"(DSSLOGI_4.doc).  
