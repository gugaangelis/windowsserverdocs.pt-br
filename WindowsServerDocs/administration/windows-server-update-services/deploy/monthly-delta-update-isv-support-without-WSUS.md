---
title: Suporte ao ISV de atualização Delta mensal sem WSUS
description: Tópico do Windows Server Update Service (WSUS) – como ISVs (fornecedores independentes de software) podem usar temporariamente a atualização Delta mensal em vez da entrega de atualizações do WSUS Express para reduzir o tamanho do pacote
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: dougkim
ms.date: 10/16/2017
ms.openlocfilehash: 9077cb87d1d0f6d59ad037c93f5608d3b698feaa
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868717"
---
# <a name="monthly-delta-update-isv-support-without-wsus"></a>Suporte ao ISV de atualização Delta mensal sem WSUS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10

Os downloads de atualização do Windows 10 podem ser grandes porque cada pacote contém todas as correções lançadas anteriormente para garantir a consistência e a simplicidade.  

Desde a versão 7, o Windows conseguiu reduzir o tamanho dos downloads de Windows Update com um recurso chamado [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2)e, embora os dispositivos do consumidor ofereçam suporte a ele por padrão, os dispositivos Windows 10 Enterprise exigem que o Windows Server Update Services (WSUS) execute vantagem do Express. Se você tiver o WSUS disponível, consulte [suporte ao ISV de entrega de atualização expressa](express-update-delivery-ISV-support.md). É recomendável usá-lo para habilitar a entrega de atualização expressa. 

Se você não tiver o WSUS instalado no momento, mas precisar de tamanhos de pacote de atualização menores no interim, poderá usar a atualização Delta mensal. A atualização Delta reduz substancialmente os tamanhos de pacote, mas não tanto quanto a entrega de atualizações do WSUS Express. Recomendamos que você implante a atualização do WSUS Express sempre que possível para maior redução nos tamanhos dos pacotes. Veja a seguir um gráfico que compara os tamanhos de download Delta, cumulativo e expresso para o Windows 10 versão 1607:

![Comparação do tamanho do download](../../media/express-update-delivery-isv-support/delta-1.png)

## <a name="what-is-monthly-delta-update"></a>O que é a atualização Delta mensal?

Há duas variantes da atualização de segurança mensal: Delta e cumulativo.

A atualização Delta mensal é nova e uma solução provisória para ISVs que não têm o WSUS disponível para ajudar a reduzir tamanhos de pacote de atualização.

>[!IMPORTANT]
>**A atualização Delta está disponível para manutenção do Windows 10, versão 1607 (atualização de aniversário), versão 1703 (atualização para criadores) e versão 1709 (atualização para criadores do outono).** Para versões posteriores à versão 1709, você precisará implementar uma infraestrutura de implantação que ofereça suporte à [entrega de atualização expressa](express-update-delivery-ISV-support.md) para continuar aproveitando as atualizações incrementais.

Usando a atualização Delta mensal, os pacotes conterão apenas as atualizações de um mês. O cumulativo mensal contém todas as atualizações até essa versão de atualização, resultando em um arquivo grande que cresce a cada mês. As atualizações Delta e mensal são lançadas na segunda terça-feira de cada mês, também conhecida como "atualização de terça-feira". A tabela a seguir compara as atualizações Delta e cumulativa:

|                    | Atualização **Delta** mensal                                                                                                                                                                                                       | Atualização **cumulativa** mensal                                                                                                                                                                                             |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Escopo**          | Atualização única com **apenas novas correções para esse mês**                                                                                                                                                                           | Atualização única com todas as novas correções para esse mês e todos os meses anteriores                                                                                                                                                   |
| **Aplicativo**    | Só poderá ser aplicado se a atualização do mês anterior tiver sido aplicada (cumulativa ou Delta)                                                                                                                                           | Pode ser aplicado a qualquer momento                                                                                                                                                                                                |
| **Entrega**       | **Publicado somente para Windows Update catálogo** , onde ele pode ser baixado para uso com outras ferramentas ou processos. Não oferecido aos computadores conectados ao Windows Update                                                         | Publicado em Windows Update (onde todos os PCs do consumidor irão instalá-lo), WSUS e o catálogo de Windows Update                                                                                                                |

Delta e cumulativo têm o mesmo número de KB, com a mesma classificação e liberação ao mesmo tempo. As atualizações podem ser diferenciadas pelo título da atualização no catálogo ou pelo nome do MSU:

- 2017-02 *\*atualizaçãoDelta\*paraWindows 10*versão1607parasistemasbaseados em x64 (KB1234567)
-  *\*atualizaçãocumulativa2017-02\*para*Windows10versão1607para sistemas baseados em x86 (KB1234567)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      

### <a name="when-to-use-monthly-delta-update"></a>Quando usar a atualização Delta mensal

Se o tamanho da atualização para o dispositivo cliente for uma preocupação, é recomendável usar a atualização Delta nos dispositivos que têm a atualização do mês anterior e a atualização cumulativa nos dispositivos que estão atrás. Dessa forma, todos os dispositivos exigem apenas uma única atualização para atualizá-los. Isso requer um pequeno ajuste no processo geral de gerenciamento de atualizações, pois você precisará implantar atualizações diferentes com base em quão atualizadas os dispositivos estão na organização:

![Comparação do tamanho do download](../../media/express-update-delivery-isv-support/delta-2.png)

### <a name="prevent-deployment-of-delta-and-cumulative-updates-in-the-same-month"></a>Impedir a implantação de atualizações Delta e cumulativas no mesmo mês

Como a atualização Delta e a atualização cumulativa estão disponíveis ao mesmo tempo, é importante entender o que acontece se você implantar ambas as atualizações no mesmo mês.

Se você aprovar e implantar a mesma versão do Delta e da atualização cumulativa, não só irá gerar tráfego de rede adicional, já que ambos serão baixados para o PC, mas talvez você não consiga reinicializar o computador para o Windows após a reinicialização.

Se as atualizações Delta e cumulativa forem instaladas inadvertidamente e o computador não estiver mais sendo inicializado, você poderá se recuperar com as seguintes etapas:

1. Inicialize no prompt de comando do WinRE
2. Listar os pacotes em um estado pendente:

    `x:\windows\system32\dism.exe /image:<drive letter for windows directory> /Get-Packages >> <path to text file>`
 
    > **Exemplo**:` x:\windows\system32\dism.exe /image:c:\ /Get-Packages >> c:\temp\packages.txt`
 
3. Abra o arquivo de texto no qual você pôde canalizar **Get-Packages**. Se você vir qualquer patch de instalação pendente, execute **Remove-Package** para cada nome de pacote:
 
   `dism.exe /image:<drive letter for windows directory> /remove-package /packagename:<package name>`
 
    > **Exemplo**:`x:\windows\system32\dism.exe /image:c:\ /remove-package /packagename:Package_for_KB4014329~31bf3856ad364e35~amd64~~10.0.1.0`
 
    >[!NOTE]
    >Não remova os patches pendentes de desinstalação.

>[!IMPORTANT]
>**A atualização Delta está disponível para manutenção do Windows 10, versão 1607 (atualização de aniversário), versão 1703 (atualização para criadores) e versão 1709 (atualização para criadores do outono).** Para versões posteriores à versão 1709, você precisará implementar uma infraestrutura de implantação que ofereça suporte à [entrega de atualização expressa](express-update-delivery-ISV-support.md) para continuar aproveitando as atualizações incrementais.
