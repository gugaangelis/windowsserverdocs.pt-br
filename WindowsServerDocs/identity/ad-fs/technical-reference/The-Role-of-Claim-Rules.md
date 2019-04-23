---
ms.assetid: 65e474b5-3076-4ba3-809d-a09160f7c2bd
title: A função das regras de declaração
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e47dbecdeee620d3a237ad8d8c41a550d3ef069c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860777"
---
>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-claim-rules"></a>A função das regras de declaração
A função geral do serviço de federação nos serviços de Federação do Active Directory \(do AD FS\) é emitir um token que contém um conjunto de declarações. A decisão sobre quais declarações do AD FS aceita e emite é regida pelas regras de declaração.  
  
## <a name="what-are-claim-rules"></a>Quais são as regras de declaração?  
Uma regra de declaração representa uma instância de lógica de negócios que irá levar um ou mais declarações de entrada, aplicar condições a elas \(se x e y\) e produzir um ou mais declarações de saída com base nos parâmetros da condição. Para obter mais informações sobre declarações de entrada e saídas, consulte [The Role of Claims](The-Role-of-Claims.md).  
  
Regras de declaração são usadas quando você precisa implementar uma lógica de negócios que controlará o fluxo de declarações por meio do pipeline de declarações. Embora o pipeline de declarações é mais um conceito lógico do fim\-para\-terminar processo de fluxo de declarações, as regras são um elemento administrativo real que você pode usar para personalizar o fluxo de declarações através do processo de emissão de declarações de declaração.  
  
Para obter mais informações sobre o pipeline de declarações, consulte [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
As regras de declaração oferecem os seguintes benefícios:  
  
-   Fornecer um mecanismo para os administradores apliquem a execução\-lógica de negócios de tempo para confiar em declarações de provedores de declarações  
  
-   Fornecer um mecanismo para que os administradores definam quais declarações são liberadas para as partes confiáveis  
  
-   Fornecer declarações e detalhadas\-com base em recursos de autorização para os administradores que desejam permitir ou negar acesso a usuários específicos  
  
### <a name="how-claim-rules-are-processed"></a>Como as regras de declaração são processadas  
As regras de declaração são processadas pelo pipeline de declarações usando o *mecanismo declarações*. O mecanismo de declarações é um componente lógico do serviço de federação que examina o conjunto de declarações de entrada apresentadas por um usuário e, em seguida, dependendo da lógica em cada regra, produzirá um conjunto de declarações de saída.  
  
Juntos, o mecanismo de regras de declarações e o conjunto de regras de declaração com uma determinada confiança federada determinam se as declarações de entrada devem ser transmitidas como estão, filtradas para atender aos critérios de uma condição específica ou transformadas em um conjunto inteiramente novo de declarações antes de serem emitidas como declarações de saída pelo Serviço de Federação.  
  
Para obter mais informações sobre esse processo, consulte [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
## <a name="what-are-claim-rule-templates"></a>Quais são os modelos de regra de declaração?  
O AD FS inclui um conjunto predefinido de regra de declaração de modelos que são projetados para ajudá-lo facilmente selecionar e criam as regras de declaração mais apropriadas para suas necessidades comerciais específicas. Os modelos de regra de declaração são usados somente durante o processo de criação de regra de declaração.  
  
No snap do gerenciamento do AD FS\-, regras só podem ser criadas usando modelos de regra de declaração. Depois de usar o snap\-para selecionar um modelo de regra de declaração, os dados necessários para a lógica da regra de entrada e salvá-lo no banco de dados de configuração, ele será \(a partir desse ponto\) mencionado na interface do usuário como uma regra de declaração.  
  
### <a name="how-claim-rule-templates-work"></a>Como funcionam os modelos de regra de declaração?  
Declaração à primeira vista, modelos de regra parecem ser apenas formulários de entrada fornecidos pelo snap\-para coletar dados e processar lógica específica em declarações de entrada. No entanto, em um nível muito mais detalhado, os modelos de regra de declaração armazenam a estrutura de linguagem de regra de declaração necessária que compõe a lógica básica necessária para criar rapidamente uma regra sem a necessidade de conhecer a linguagem intimamente.  
  
Cada modelo que é fornecido na interface do usuário \(interface do usuário\) representa uma sintaxe de linguagem da regra de declaração preenchida previamente, com base nas tarefas administrativas mais comuns. Há um modelo de regra, no entanto, que é a exceção. Esse modelo é chamado de modelo de regra personalizada. Com esse modelo, nenhuma sintaxe será preenchida previamente. Em vez disso, você deve criar diretamente a sintaxe de linguagem de regra de declaração no corpo do formulário de modelo de regra de declaração usando a sintaxe de linguagem da regra de declaração.  
  
Para obter mais informações sobre como usar a sintaxe de linguagem da regra de declaração, consulte [The Role of a linguagem da regra de declaração](The-Role-of-the-Claim-Rule-Language.md) no guia de implantação do AD FS.  
  
> [!TIP]  
> Você pode exibir a linguagem da regra de declaração associada a uma regra a qualquer momento clicando no botão **Exibir linguagem da regra** nas propriedades de uma regra de declaração.  
  
### <a name="how-to-create-a-claim-rule"></a>Como criar uma regra de declaração  
Regras de declaração são criadas separadamente para cada relação de confiança federada dentro do Serviço de Federação e não são compartilhadas entre várias relações de confiança. Você pode criar uma regra de um modelo de regra de declaração, começar do zero criando a regra usando a linguagem da regra de declaração ou usar o Windows PowerShell para personalizar uma regra.  
  
Todas essas opções coexistirem para fornecer a flexibilidade de escolher o método apropriado para um determinado cenário. Para obter mais informações sobre como criar uma regra de declaração, consulte [Configurando regras de declaração](https://technet.microsoft.com/library/ee913571.aspx) no guia de FSDeployment do AD.  
  
#### <a name="using-claim-rule-templates"></a>Usando modelos de regras de declaração  
Os modelos de regra de declaração são usados somente durante o processo de criação de regra de declaração. Você pode usar qualquer um dos seguintes modelos para criar uma regra de declaração:  
  
-   Passar ou filtrar uma declaração de entrada  
  
-   Transformar uma declaração de entrada  
  
-   Enviar atributos LDAP como declarações  
  
-   Enviar associação de grupo como uma declaração  
  
-   Enviar declarações usando uma regra personalizada  
  
-   Permitir ou negar usuários com base em uma declaração de entrada  
  
-   Permitir todos os usuários  
  
Para obter mais informações sobre cada um desses modelos de regras de declaração, consulte [determinar o tipo de declaração de modelo de regra para uso](Determine-the-Type-of-Claim-Rule-Template-to-Use.md).  
  
#### <a name="using-the-claim-rule-language"></a>Usando linguagem de regra de declaração  
Para regras de negócios que estão além do escopo dos modelos de regra de declaração padrão, você pode usar um modelo de regra personalizado para expressar uma série de condições de lógica complexas usando a linguagem da regra de declaração. Para obter mais informações sobre como usar uma regra personalizada, consulte [quando usar uma regra de declaração personalizada](When-to-Use-a-Custom-Claim-Rule.md).  
  
#### <a name="using-windowspowershell"></a>Usando o Windows PowerShell  
Você também pode usar o objeto de cmdlet ADFSClaimRuleSet com o Windows PowerShell para criar ou administrar as regras do AD FS. Para obter mais informações sobre como você pode usar o Windows PowerShell com esse cmdlet, consulte [administração do AD FS com o Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
## <a name="what-is-a-claim-rule-set"></a>O que é um conjunto de regras de declaração?  
Conforme mostrado na ilustração a seguir, um conjunto de regras de declaração é um agrupamento de uma ou mais regras para uma confiança federada determinada que definirá como as declarações serão processadas pelo mecanismo de regra de declarações. Quando uma declaração de entrada é recebida pelo Serviço de Federação, o mecanismo de regras de declaração aplica a lógica especificada pelo conjunto de regras de declaração adequado. É a soma final da lógica de cada regra no conjunto que determina como as declarações serão emitidas para uma determinada confiança em sua totalidade.  
  
![Funções do AD FS](media/adfs2_claimruleset.gif)  
  
As regras de declaração são processadas pelo mecanismo de declarações em ordem cronológica dentro de um determinado conjunto de regrar. Essa ordem é importante, porque a saída de uma regra pode ser usada como entrada para a próxima regra no conjunto.  
  
## <a name="what-are-claim-rule-set-types"></a>O que são os tipos de conjuntos de regras de declaração?  
Um tipo de conjunto de regra de declaração é um segmento lógico de uma confiança federada que identifica categoricamente se o conjunto de regras de declaração associado à relação de confiança será usado para a emissão, autorização ou aceitação de declarações. Cada relação de confiança federada pode ter um ou mais tipos de conjuntos de regras de declaração associados a ele, dependendo do tipo de relação de confiança que é usado.  
  
A tabela a seguir descreve os vários tipos de conjuntos de regras de declaração e explica sua relação com uma relação de confiança de provedor de declarações ou com o objeto de confiança de terceira parte confiável.  
  
|Tipo de conjunto de regras de declaração|Descrição|Usado em|  
|-----------------------|---------------|-----------|  
|Conjunto de regra de transformação de aceitação|Um conjunto de regras de declaração usado em uma determinada relação de confiança do provedor de declarações para especificar as declarações de entrada que serão aceitas da organização do provedor de declarações e as declarações de saída que serão enviadas para o objeto de confiança de terceira parte confiável.<br /><br />As declarações de entrada que serão usadas para fornecer esse conjunto de regras serão as declarações que são produzidas pelo conjunto de regras de transformação de emissão conforme especificado na organização do provedor de declarações.<br /><br />Por padrão, o nó de confiança do provedor de declarações contém uma relação de confiança do provedor de declarações chamada **Active Directory** que é usada para representar o armazenamento de atributos de fonte para o conjunto de regras de transformação de aceitação. Esse objeto de confiança é usado para representar a conexão do seu Serviço de Federação com um banco de dados do Active Directory em sua rede. Esta relação de confiança padrão é o que processa solicitações para usuários que foram autenticados pelo Active Directory e não pode ser excluída.|Relação de confiança do provedor de declarações|  
|Conjunto de regras de transformação de emissão|Um conjunto de regras de declaração que você usa em um objeto de confiança de terceira parte confiável para especificar as declarações que serão emitidas à terceira parte confiável.<br /><br />As declarações de entrada que serão usadas para fornecer esse conjunto de regras serão inicialmente as declarações que são produzidas pelas regras de transformação de aceitação.|Objetos de confiança de terceira parte confiável|  
|Conjunto de Regras de Autorização de Emissão|Um conjunto de regras de declaração que você usa em um objeto de confiança de terceira parte confiável para especificar os usuários que terão permissão para receber um token em nome da terceira parte confiável.<br /><br />Essas regras determinam se um usuário pode receber declarações em nome de uma parte confiável e, portanto, ter acesso à terceira parte confiável.<br /><br />A menos que você especifique uma regra de autorização de emissão, todos os usuários terão o acesso negado por padrão.|Objetos de confiança de terceira parte confiável|  
|Conjunto de Regras de Autorização de Delegação|Um conjunto de regras de declaração que você usa em um objeto de confiança de terceira parte confiável para especificar os usuários que terão permissão para agir como representantes de outros usuários da terceira parte confiável.<br /><br />Essas regras determinam se o solicitante tem permissão para representar um usuário enquanto ainda identifica o solicitante no token enviado à terceira parte confiável.<br /><br />A menos que você especifique uma regra de autorização de emissão, nenhum usuário poderá agir como representante por padrão.|Objetos de confiança de terceira parte confiável|  
|Conjunto de Regras de Autorização de Representação|Um conjunto de regras de declaração configurado usando o Windows PowerShell para determinar se um usuário pode representar totalmente outro usuário para a terceira parte confiável.<br /><br />Essas regras determinam se o solicitante tem permissão para representar um usuário sem identificar o solicitante no token enviado à terceira parte confiável.<br /><br />Representar outro usuário dessa maneira é um recurso muito poderoso, porque a terceira parte confiável não saberá que o usuário está sendo representado.|Objeto de confiança de terceira parte confiável|  
  
Para obter mais informações sobre como selecionar as regras de declaração adequado para usar em sua organização, consulte [determinar o tipo de declaração de modelo de regra para uso](Determine-the-Type-of-Claim-Rule-Template-to-Use.md).  
  

