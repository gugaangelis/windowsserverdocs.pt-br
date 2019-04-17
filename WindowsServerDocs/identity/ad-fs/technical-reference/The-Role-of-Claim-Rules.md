---
ms.assetid: 65e474b5-3076-4ba3-809d-a09160f7c2bd
title: "A função de regras de declaração"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e47dbecdeee620d3a237ad8d8c41a550d3ef069c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-claim-rules"></a>A função de regras de declaração
A função geral do serviço de Federação em serviços de Federação do Active Directory \(AD FS\) serve para emitir um token que contém um conjunto de declarações. A decisão sobre quais declarações do AD FS aceita e depois, emite é regida pelas regras de declaração.  
  
## <a name="what-are-claim-rules"></a>Quais são as regras de declaração?  
Uma regra de declaração representa uma instância de lógica de negócios que vai levar uma ou mais declarações de entrada, aplicar condições a eles \ (se x y\ then) e produz uma ou mais declarações de saída com base nos parâmetros condição. Para obter mais informações sobre as declarações de entrada e saídas, veja [a função de declarações](The-Role-of-Claims.md).  
  
Usar regras de declaração quando você precisa implementar a lógica de negócios que irão controlar o fluxo de declarações através do pipeline de declarações. Enquanto o pipeline de declarações é mais um conceito lógico de declarações o processo de end\ to\ ponta para transferir as regras de declaração são um elemento administrativo real que você pode usar para personalizar o fluxo de declarações durante o processo de emissão de declarações.  
  
Para obter mais informações sobre o pipeline de declarações, consulte [a função do mecanismo requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Engine.md).  
  
Regras de declaração oferecem os seguintes benefícios:  
  
-   Fornecer um mecanismo para os administradores para aplicar a lógica de negócios do tempo de run\ para confiantes requerimentos judiciais ou Extrajudiciais de provedores de declarações  
  
-   Fornecer um mecanismo para que os administradores definam quais requerimentos judiciais ou Extrajudiciais forem lançados para partes confiantes  
  
-   Fornecer recursos de autorização com base em claims\ rica e detalhadas para administradores que desejam permitir ou negar o acesso a usuários específicos  
  
### <a name="how-claim-rules-are-processed"></a>Como as regras de declaração são processadas  
Declaração regras são processadas por meio do pipeline de requerimentos judiciais ou Extrajudiciais usando o *mecanismo requerimentos judiciais ou Extrajudiciais*. O mecanismo de declarações é um componente lógico do serviço de Federação examina o conjunto de declarações de entrada apresentada por um usuário e em seguida, dependendo da lógica em cada regra, produzirá um conjunto de saída de declarações.  
  
Juntos, as declarações regra mecanismo e o conjunto de regras associadas a uma determinada confiança federada determinam se as declarações de entrada devem ser passadas por meio de como são, filtradas para atender aos critérios de uma condição específica ou transformadas em um conjunto inteiramente novo de reivindicações antes que eles são emitidos como saídas requerimentos judiciais ou Extrajudiciais por seu serviço de federação de declaração.  
  
Para obter mais informações sobre esse processo, consulte [a função do mecanismo requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Engine.md).  
  
## <a name="what-are-claim-rule-templates"></a>Quais são os modelos de regra de declaração?  
AD FS inclui um conjunto predefinido de regra de declaração modelos que são projetados para ajudá-lo facilmente selecionar e criam as regras de reivindicação mais apropriadas para suas necessidades comerciais específico. Reivindica regra modelos só são usados durante o processo de criação de regras de declaração.  
  
No AD FS gerenciamento snap\-in, o regras só podem ser criadas usando modelos de regra de declaração. Depois de usar o snap\-in para selecionar um modelo de regra de declaração, os dados necessários para a lógica de regra de entrada e salvá-lo no banco de dados de configuração, ele será \ (a partir desse ponto forward\) chamado na interface do usuário como uma regra de declaração.  
  
### <a name="how-claim-rule-templates-work"></a>Como reivindicar o trabalho de modelos de regra  
A princípio, modelos de regra de declaração parecem ser formulários apenas entrados fornecidos pelo snap\-in para coletar dados e o processo de lógica específica em declarações de entrada. No entanto, em um nível muito mais detalhado, reivindica repositório de modelos de regra a necessária reivindicar estrutura de linguagem de regra que compõem a lógica básica necessária para criar rapidamente uma regra sem precisar saber o idioma intimamente.  
  
Cada modelo é fornecido na interface do usuário \(UI\) representa uma sintaxe de linguagem de regra reivindicação previamente preenchido, com base nas tarefas administrativas mais comumente necessárias. Há um modelo de regra no entanto, que é a exceção. Esse modelo é conhecido como o modelo de regra personalizada. Com este modelo, nenhuma sintaxe é previamente preenchida. Em vez disso, você deve criar diretamente a sintaxe da linguagem reivindicação regra no corpo de formulário de modelo de regras de declaração usando a sintaxe de linguagem de regra de declaração.  
  
Para obter mais informações sobre como usar a sintaxe da linguagem reivindicação regra, consulte [a função da linguagem de regra reivindicar](The-Role-of-the-Claim-Rule-Language.md) no guia de implantação do AD FS.  
  
> [!TIP]  
> Você pode exibir a linguagem de regra de declaração associada com uma regra a qualquer momento clicando o **idioma de regra de modo de exibição** botão nas propriedades de uma regra de declaração.  
  
### <a name="how-to-create-a-claim-rule"></a>Como criar uma regra de declaração  
Regras de declaração são criadas separadamente para cada relação de confiança federada dentro do serviço de Federação e não são compartilhadas entre várias relações de confiança. Você pode criar uma regra de um modelo de regra de declaração, começar do zero Criando a regra usando a linguagem de regra de declaração ou usar o Windows PowerShell para personalizar uma regra.  
  
Todas essas opções coexistam para lhe proporcionar a flexibilidade de escolher o método apropriado para um determinado cenário. Para obter mais informações sobre como criar uma regra de declaração, veja [Configurando regras reivindicar](https://technet.microsoft.com/library/ee913571.aspx) no guia de FSDeployment do AD.  
  
#### <a name="using-claim-rule-templates"></a>Usando modelos de regra de declaração  
Reivindica regra modelos só são usados durante o processo de criação de regras de declaração. Você pode usar qualquer um dos seguintes modelos para criar uma regra de declaração:  
  
-   Passagem ou filtrar uma declaração de entrada  
  
-   Transformar uma declaração de entrada  
  
-   Enviar atributos LDAP como requerimentos judiciais ou Extrajudiciais  
  
-   Enviar a associação ao grupo como uma reivindicação  
  
-   Enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada  
  
-   Permitir ou negar aos usuários com base em uma declaração de entrada  
  
-   Permitir que todos os usuários  
  
Para obter mais informações, que descreve cada um desses reivindicar modelos de regra, consulte [determinar o tipo de declaração regra modelo para uso](Determine-the-Type-of-Claim-Rule-Template-to-Use.md).  
  
#### <a name="using-the-claim-rule-language"></a>Usando a linguagem de regra de declaração  
Para as regras de negócios que estão além do escopo dos modelos de regra de declaração padrão, você pode usar um modelo de regra personalizada para expressar uma série de condições de uma lógica complexa usando a linguagem de regra de declaração. Para obter mais informações sobre como usar uma regra personalizada, veja [quando usar uma regra personalizada da reivindicação](When-to-Use-a-Custom-Claim-Rule.md).  
  
#### <a name="using-windows-powershell"></a>Usando o Windows PowerShell  
Você também pode usar o objeto do cmdlet ADFSClaimRuleSet com o Windows PowerShell para criar ou administrar regras no AD FS. Para saber mais sobre como você pode usar o Windows PowerShell com este cmdlet, consulte [administração do AD FS com o Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
## <a name="what-is-a-claim-rule-set"></a>O que é um conjunto de regras de declaração?  
Conforme mostrado na ilustração a seguir, um conjunto de regras de declaração é um agrupamento de uma ou mais regras para uma determinado confiança federada que definem como requerimentos judiciais ou Extrajudiciais serão processados pelo mecanismo de regras de declarações. Quando uma declaração de entrada é recebida pelo serviço de Federação o mecanismo de regras de declaração se aplica a lógica especificada pelo conjunto de regras de declarações apropriadas. É a soma final da lógica de cada regra no conjunto que irá determinar como requerimentos judiciais ou Extrajudiciais ser emitidas para uma determinado confiança em sua totalidade.  
  
![Funções do AD FS](media/adfs2_claimruleset.gif)  
  
Declaração regras são processadas pelo mecanismo de declarações em ordem cronológica dentro de um conjunto de regras de determinado. Essa ordem é importante, porque a saída de uma regra pode ser usada como a entrada para a próxima regra no conjunto.  
  
## <a name="what-are-claim-rule-set-types"></a>Quais são as regras de declaração definir tipos?  
Tipo de conjunto de regras uma declaração é um segmento lógico de uma confiança federada mostrada por categoria identifica se o conjunto de regras de declaração associado a relação de confiança será usado para declarações emissão, autorização ou aceitação. Cada confiança federada pode ter um ou mais regras reivindicação definir tipos associados a ele, dependendo do tipo de confiança que é usado.  
  
A tabela a seguir descreve os vários tipos de conjuntos de regra de declarações e explica sua relação com uma relação de confiança de provedor de declarações ou terceira confiança de terceiros.  
  
|Tipo de conjunto de regras de declaração|Descrição|Usado em|  
|-----------------------|---------------|-----------|  
|Conjunto de regras de transformação de aceitação|Um conjunto de regras de reivindicação que você usa em uma determinada declarações de confiança do provedor para especificar as declarações de entrada que serão aceitos da organização de provedor de declarações e as declarações de saída que serão enviadas para a terceira confiança de terceiros.<br /><br />As declarações de entrada que serão usadas para esse conjunto de regras de origem serão as declarações são a saída a regra de transformação de emissão definir conforme especificado na organização requerimentos judiciais ou Extrajudiciais provedor.<br /><br />Por padrão, o nó de confiança do provedor de declarações contém uma relação de confiança de provedor de declaração denominada **do Active Directory** que é usado para representar o armazenamento de atributo de origem para o conjunto de regras de transformação de aceitação. Esse objeto de confiança é usado para representar a conexão do seu serviço de federação para um banco de dados do Active Directory em sua rede. Esta relação de confiança padrão é o que processa declarações para usuários que foram autenticados pelo Active Directory e ele não pode ser excluído.|Relações de confiança de provedor de declarações|  
|Conjunto de regras de transformação de emissão|Um conjunto de regras de reivindicação que você usa em uma terceira confiança de terceiros para especificar as declarações ser emitidas para o terceiro.<br /><br />As declarações de entrada que serão usadas para esse conjunto de regras de origem será inicialmente as declarações são a saída as regras de transformação de aceitação.|Dependência relações de confiança de terceiros|  
|Conjunto de regras de autorização de emissão|Um conjunto de regras de reivindicação que você usa em uma terceira confiança de terceiros para especificar os usuários poderão receber um token para o terceiro.<br /><br />Essas regras determinar se um usuário pode receber reivindicações por um terceiro e, portanto, acesso à parte confiável.<br /><br />A menos que você especifique uma regra de autorização de emissão, todos os usuários terá o acesso negados por padrão.|Dependência relações de confiança de terceiros|  
|Conjunto de regras de autorização de delegação|Um conjunto de regras de reivindicação que você usa em uma terceira confiança de terceiros para especificar os usuários que serão permitidos para atuar como representantes de outros usuários para o terceiro.<br /><br />Essas regras determinar se o solicitante tem permissão para representar um usuário enquanto ainda identificar o solicitante no token que é enviado para o terceiro.<br /><br />A menos que você especifique uma regra de autorização de emissão, nenhum usuário pode atuar como representantes por padrão.|Dependência relações de confiança de terceiros|  
|Conjunto de regras de autorização de representação|Um conjunto de regras que você configurar usando o Windows PowerShell para determinar se um usuário podem totalmente de declaração representar outro usuário à parte confiável.<br /><br />Essas regras determinar se o solicitante tem permissão para representar um usuário sem identificar o solicitante no token que é enviado para o terceiro.<br /><br />Se passando por outro usuário dessa maneira é uma funcionalidade muito poderosa, pois o terceiro não saberá que o usuário está sendo representado.|Dependência de confiança de terceiros|  
  
Para obter mais informações sobre como selecionar as regras de declaração apropriado para usar em sua organização, consulte [determinar o tipo de declaração regra modelo para uso](Determine-the-Type-of-Claim-Rule-Template-to-Use.md).  
  

