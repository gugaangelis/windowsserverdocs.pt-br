---
title: Opções de atualização e conversão para o Windows Server 2016
description: Explica todos os caminhos de atualização com suporte para o Windows Server 2016.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.date: 01/18/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 74aa1da3-7076-4a1f-ad5b-9e17bd46dba2
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 2484363db661620844993d52914700cb8b6cdf56
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391587"
---
# <a name="upgrade-and-conversion-options-for-windows-server-2016"></a>Opções de atualização e conversão para o Windows Server 2016

>Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico inclui informações sobre como atualizar para o Windows Server® 2016 de uma variedade de sistemas operacionais anteriores usando uma série de métodos.

O processo de transição para o Windows Server 2016 pode variar muito, dependendo do sistema operacional com o qual você está começando e do caminho escolhido. Usamos os seguintes termos para distinguir diferentes ações, qualquer uma delas pode estar envolvida em uma nova implantação do Windows Server 2016.

- **Instalação** é o conceito básico de colocar o novo sistema operacional em seu hardware. Especificamente, uma **instalação limpa** requer a exclusão do sistema operacional anterior. Para obter informações sobre a instalação do Windows Server 2016, confira [Requisitos de sistema e informações de instalação do Windows Server 2016](https://technet.microsoft.com/windows-server-docs/get-started/system-requirements--and-installation). Para obter informações sobre a instalação de outras versões do Windows Server, confira [Instalação e Atualização do Windows Server](https://technet.microsoft.com//windowsserver/dn527667).

- **Migração** significa mudar seu sistema operacional existente para o Windows Server 2016 transferindo para um conjunto de hardware ou máquina virtual diferente. A migração, que pode variar consideravelmente dependendo das funções de servidor instaladas, é discutida em detalhes em [Instalação, atualização e migração do Windows Server](https://technet.microsoft.com/windowsserver/dn458795).

- **Atualização sem interrupção do sistema operacional do cluster** é um novo recurso no Windows Server 2016 que permite ao administrador atualizar o sistema operacional dos nós do cluster do Windows Server 2012 R2 para o Windows Server 2016 sem interromper o Hyper-V ou as cargas de trabalho de Servidor de Arquivos de Escalabilidade Horizontal. Esse recurso permite que você evite o tempo de inatividade que poderia afetar os Contratos de nível de serviço. Esse recurso novo é abordado com mais detalhes em [Atualização sem interrupção do sistema de operacional do cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

- **Conversão da licença** em algumas versões do sistema operacional – é possível converter uma edição específica da versão para outra edição da mesma versão em uma única etapa, com um simples comando e com a chave de licença adequada. Chamamos a isto “conversão da licença”. Por exemplo, se estiver executando o Windows Server 2016 Standard, será possível convertê-lo para o Windows Server 2016 Datacenter.

- **Atualizar** significa mudar do seu sistema operacional existente para uma versão mais recente, mantendo o mesmo hardware. (Às vezes, isso é chamado de atualização "in-loco") Por exemplo, se seu servidor estiver executando o Windows Server 2012, ou o Windows Server 2012 R2, você poderá atualizá-lo para o Windows Server 2016. É possível atualizar de uma versão de avaliação do sistema operacional para uma versão comercial, de uma versão comercial mais antiga para uma versão mais nova ou, em alguns casos, de uma edição com licença de volume do sistema operacional para uma edição comercial comum.

> [!IMPORTANT]  
> A atualização funciona melhor em máquinas virtuais, onde os drivers de hardware específicos do OEM não são necessários para uma atualização bem-sucedida.  

> [!IMPORTANT]  
> Para versões do Windows Server 2016 anteriores a 14393.0.161119-1705.RS1_REFRESH, **somente é possível executar essa conversão de avaliação para versão comercial** com o Windows Server 2016 que foi instalado usando a opção de Experiência Desktop (não a opção Server Core). A partir da versão 14393.0.161119-1705. RS1_REFRESH e versões posteriores, é possível converter edições de avaliação em versão comercial, independentemente da opção de instalação usada.

> [!IMPORTANT]  
> Se o seu servidor usa o Agrupamento NIC, desabilite o Agrupamento NIC antes da atualização e, depois, habilite-o novamente após a conclusão da atualização. confira [Visão geral do Agrupamento NIC](https://technet.microsoft.com/library/hh831648(v=ws.11).aspx) para obter detalhes.

## <a name="upgrading-previous-retail-versions-of-windows-server-to-windows-server-2016"></a>Atualizando versões comerciais anteriores do Windows Server para Windows Server 2016

A tabela a seguir traz um breve resumo sobre quais sistemas operacionais Windows **já licenciados** (isto é, que não sejam versões de avaliação) podem ser atualizados para as respectivas edições do Windows Server 2016.

Observe as seguintes diretrizes gerais para caminhos com suporte:

- Não há suporte para atualizações de arquiteturas de 32 bits para 64 bits. Todas as edições do Windows Server 2016 são apenas 64 bits.
- Não há suporte para atualizações de um idioma para outro.
- Se o servidor for um controlador de domínio, confira [Atualizar controladores de domínio para o Windows Server 2012 R2 e o Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx) para saber mais.
- Não há suporte para atualizações de versões de pré-lançamento (visualizações) do Windows Server 2016. Realize uma instalação limpa do Windows Server 2016.
- Não há suporte para atualizações que alternam da instalação Server Core para uma instalação de Servidor com Desktop (e vice-versa).
- Não há suporte para atualizações de uma instalação anterior do Windows Server para uma cópia de avaliação do Windows Server. Versões de avaliação devem ser instaladas como uma instalação limpa.

Se a sua versão atual não aparecer na coluna à esquerda, a atualização para esta versão do Windows Server 2016 não tem suporte.

Se aparecer mais de uma edição na coluna à direita, a atualização para **qualquer** edição da mesma versão inicial tem suporte.

|Se você está executando esta edição:|É possível atualizar para estas edições:|  
|-------------------|----------|  
|Windows Server 2012 Standard|Windows Server 2016 Standard ou Datacenter|
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard ou Datacenter|
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Grupo de trabalho do Windows Storage Server 2012|Grupo de trabalho do Windows Storage Server 2016|
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Grupo de trabalho do Windows Storage Server 2016|


## <a name="per-server-role-considerations-for-upgrading"></a>Considerações de atualização por função do servidor

Mesmo em caminhos de atualização de versões comerciais anteriores do Windows Server 2016 com suporte, certas funções do servidor que já estão instaladas podem necessitar de preparação ou de ações adicionais para que a função continue a funcionar após a atualização. confira os tópicos específicos da Biblioteca do TechNet para conhecer a função de cada servidor que deseja atualizar quanto aos detalhes das etapas adicionais que poderiam ser necessárias.

## <a name="converting-a-current-evaluation-version-to-a-current-retail-version"></a>Conversão de uma versão de avaliação atual para uma versão comercial atual

É possível converter a versão de avaliação do Windows Server 2016 Standard para o Windows Server 2016 Standard (comercial) ou para o Datacenter (comercial). De igual modo, é possível converter a versão de avaliação do Windows Server 2016 Datacenter para a versão comercial.

> [!IMPORTANT]  
> Para versões do Windows Server 2016 anteriores a 14393.0.161119-1705.RS1_REFRESH, somente é possível executar essa conversão de avaliação para versão comercial com o Windows Server 2016 que foi instalado usando a opção de Experiência Desktop (não a opção Server Core). A partir da versão 14393.0.161119-1705. RS1_REFRESH e versões posteriores, é possível converter edições de avaliação em versão comercial, independentemente da opção de instalação usada.

Antes de tentar converter de uma versão de avaliação para a versão comercial, verifique se o servidor está realmente executando uma versão de avaliação. Para isso, siga um destes procedimentos:

- Em um prompt de comando elevado, execute **slmgr.vbs /dlv**; as versões de avaliação incluirão “EVAL” na saída.

- Na tela Inicial, abra o **Painel de Controle**. Abra **Sistema e Segurança**e, em seguida, **Sistema**. Visualize o status de ativação do Windows na área de ativação do Windows da página **Sistema**. Clique em **Exibir detalhes** na ativação do Windows para obter mais informações sobre o status de ativação do Windows.

Caso você já tenha ativado o Windows, a Área de Trabalho mostrará o tempo restante do período de avaliação.

Se o servidor estiver executando uma versão comercial em vez da versão de avaliação, confira a seção "Atualizando versões comerciais anteriores do Windows Server para Windows Server 2016" deste documento para obter instruções sobre a atualização para o Windows Server 2016.

Para **Windows Server 2016 Essentials:** É possível realizar a conversão para a versão comercial completa inserindo uma chave comercial, licença de volume ou chave OEM no comando **slmgr.vbs**.

Se o servidor estiver executando uma versão de avaliação do Windows Server 2016 Standard ou do Windows Server 2016 Datacenter, será possível convertê-lo em uma versão comercial da seguinte maneira:

1.  Se o servidor for um **controlador de domínio**, não será possível convertê-lo em uma versão comercial. Neste caso, instale um controlador de domínio adicional em um servidor que execute uma versão de avaliação e remova o AD DS do controlador de domínio executado na versão de avaliação. Para saber mais, confira [Atualizar controladores de domínio para o Windows Server 2012 R2 e o Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx).
2.  Leia os termos de licença.
3.  Em um prompt de comando elevado, determine o nome da edição atual com o comando **DISM /online /Get-CurrentEdition**. Anote a ID da edição (uma forma abreviada do nome da edição). Em seguida, execute **DISM /online /Set-Edition:\<ID da edição\> /ProductKey:XXXXX-XXXXX-XXXXX-XXXXX-XXXXX /AcceptEula**, fornecendo a ID da edição e uma chave de produto comercial. O servidor será reiniciado duas vezes.

Para a versão de avaliação do Windows Server 2016 Standard, também é possível realizar a conversão na versão comercial do Windows Server 2016 Datacenter em uma etapa usando o mesmo comando e a chave do produto correta.

> [!TIP] 
> Para saber mais sobre o Dism.exe, consulte [Opções de linha de comando do DISM](https://go.microsoft.com/fwlink/?LinkId=192466).

## <a name="converting-a-current-retail-edition-to-a-different-current-retail-edition"></a>Conversão de uma edição comercial atual em outra edição comercial atual

A qualquer momento após a instalação do Windows Server 2016, será possível executar a Instalação para reparar a instalação (às vezes chamado de "reparo in-loco") ou, em alguns casos, realizar a conversão em uma edição diferente.
É possível executar a Instalação para realizar um "reparo in-loco" de qualquer edição do Windows Server 2016; o resultado será a mesma edição com a qual você começou.

No Windows Server 2016 Standard, é possível converter o sistema para o Windows Server 2016 Datacenter como a seguir: Em um prompt de comando elevado, determine o nome da edição atual com o comando **DISM /online /Get-CurrentEdition**. Para o Windows Server 2016 Standard, isso será `ServerStandard`. Execute o comando **DISM /online /Get-TargetEditions** para obter a ID da edição para a qual é possível atualizar. Anote essa ID da edição, uma forma abreviada do nome da edição. Em seguida, execute **DISM /online /Set-Edition:\<ID da edição\> /ProductKey:XXXXX-XXXXX-XXXXX-XXXXX-XXXXX /AcceptEula**, fornecendo a ID da edição de destino e a chave de produto comercial. O servidor será reiniciado duas vezes.

## <a name="converting-a-current-retail-version-to-a-current-volume-licensed-version"></a>Conversão de uma versão comercial atual em uma versão licenciada por volume atual

A qualquer momento após instalar o Windows Server 2016, será possível convertê-lo livremente entre uma versão comercial, uma versão licenciada por volume ou uma versão OEM. A edição permanece a mesma durante essa conversão. Se você estiver começando com uma versão de avaliação, faça a conversão dele para a primeira versão comercial e, depois, converta novamente conforme descrito aqui.

Para fazer isso, em um prompt de comandos com privilégios elevados, execute: **slmgr /ipk \<chave\>**

Em que \<chave\> é a chave de licença de volume, comercial ou chave do produto (Product Key) OEM adequada.
