---
title: Requisitos do Sistema
description: Quais são os requisitos mínimos de armazenamento, CPU, rede, memória e RAM em uma instalação limpa para cada opção de instalação.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 10/17/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a8b42d7-9fe5-4efe-9ea1-ace2131fe068
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 29183c62830cbe9e26cce4e0ce4543b554f0ed65
ms.sourcegitcommit: 3883eebbba70bfea0221e510863ee1a724a5f926
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5783738"
---
# Requisitos do sistema

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016 

Este tópico aborda os requisitos mínimos de sistema para executar o Windows Server&reg; 2016 ou o Windows Server, versão 1709.


> [!Note]  
> Nesta versão, apenas instalações limpas são recomendadas.  
>   

> [!NOTE]  
> Se, no momento da instalação, você optar por instalar com a opção Server Core, deverá estar ciente de que nenhum componente de GUI está instalado e você não poderá instalá-los ou desinstalá-los com o Gerenciador do Servidor. Se você precisar de recursos de GUI, escolha a opção "Servidor com Experiência Desktop" ao instalar o Windows Server 2016. Para obter mais informações, consulte [Install Nano Server](Getting-Started-with-Nano-Server.md) (Instalar o Nano Server)  


## Examine os requisitos de sistema  
A seguir apresentamos requisitos de sistema estimados para o Windows Server 2016. Se o seu computador não atender aos requisitos "mínimos", não será possível instalar este produto corretamente. Os requisitos reais variam conforme a configuração do sistema e dos aplicativos e recursos instalados.

Salvo indicação em contrário, esses requisitos mínimos de sistema aplicam-se a todas as opções de instalação (Server Core, Server com Experiência Desktop e Nano Server) e às edições Standard e Datacenter.  

> [!IMPORTANT]  
> O escopo muito diversificado de possíveis implantações inviabiliza a determinação de requisitos de sistema "recomendados" para aplicação geral. Consulte a documentação de cada função de servidor a ser implantada para obter mais detalhes sobre os recursos necessários a determinadas funções de servidor. Para obter os melhores resultados, realize implantações de teste para determinar os requisitos de sistema adequados aos cenários da sua implantação em particular.  


## Processador  
O desempenho do processador depende não apenas da frequência do relógio do processador, mas também do número de núcleos e do tamanho do cache do processador. Os requisitos de processador para este produto são:  

**Mínimo**:  
- Processador de 1,4 GHz e 64 bits  
- Compatível com conjunto de instruções de x64  
- Oferece suporte a NX e DEP  
- Oferece suporte a CMPXCHG16b, LAHF/SAHF e PrefetchW  
- Oferece suporte à Conversão de endereços de segundo nível (EPT ou NPT)  

[Coreinfo](https://technet.microsoft.com/sysinternals/cc835722.aspx) é uma ferramenta que você pode usar para confirmar quais desses recursos sua CPU possui.

## RAM  
Os requisitos estimados de RAM para este produto são:  

**Mínimo**:  
- 512 MB (2 GB para a opção de instalação Servidor com Experiência Desktop)
- Tipo ECC (código de correção de erro) ou tecnologias semelhantes  

> [!IMPORTANT]  
> Se você criar uma máquina virtual com os parâmetros mínimos de hardware com suporte (1 núcleo do processador e 512 MB de RAM) e, em seguida, tentar instalar esta versão na máquina virtual, a instalação falhará.  
>   
> Para evitar isso, siga um destes procedimentos:  
>   
> -   Aloque mais de 800 MB de RAM para a máquina virtual em que você pretende instalar esta versão. Uma vez que a instalação for sido concluída, você poderá alterar a alocação para até 512 MB de RAM, dependendo da configuração do servidor real.  
> -   Interrompa o processo de inicialização desta versão na máquina virtual com SHIFT+F10. No prompt de comando que será aberto, use Diskpart.exe para criar e formatar uma partição de instalação. Execute **Wpeutil createpagefile /path=C:\pf.sys** (presumindo que a partição de instalação que você criou tenha sido C:). Feche o prompt de comando e prossiga com a instalação.  

## Controlador de armazenamento e requisitos de espaço em disco  
Os computadores que executam o Windows Server 2016 devem incluir um adaptador de armazenamento compatível com a especificação de arquitetura PCI Express. Os dispositivos de armazenamento persistentes em servidores classificados como unidades de disco rígido não devem ser PATA. O Windows Server 2016 não permite ATA/PATA/IDE/EIDE para unidades de inicialização, paginação ou dados.  

Estes são os requisitos de espaço em disco **mínimos** estimados para a partição do sistema.  

**Mínimo**: 32 GB  

   > [!NOTE]  
    > Esteja ciente de que o valor 32 GB deve ser considerado um *valor mínimo absoluto* para uma instalação bem-sucedida. Este mínimo deve permitir que você instale o modo Server Core do Windows Server 2016, com a função de servidor Serviços Web (IIS). Um servidor no modo de Server Core é cerca de 4 GB menor que o mesmo servidor no modo de Servidor com GUI. 
    >   
    > A partição do sistema precisará de espaço extra em qualquer uma das seguintes circunstâncias:  
    >   
    > -   Se você instalar o sistema em uma rede.  
    > -   Computadores com mais de 16 GB de RAM precisarão de mais espaço em disco para arquivos de paginação, hibernação e despejo.  

## Requisitos do adaptador de rede  

Os adaptadores de rede usados com essa versão devem incluir estes recursos:  

**Mínimo**:  
- Um adaptador de Ethernet capaz de pelo menos produtividade de gigabit  
- Compatível com a especificação de arquitetura PCI Express.  
- Oferece suporte a PXE (Pre-Boot Execution Environment).  

Um adaptador de rede que oferece suporte à depuração de rede (KDNet) é útil, mas não é um requisito mínimo.   



## Outros requisitos  
Os computadores que executam essa versão também devem ter o seguinte:  


-   Unidade de DVD (caso pretenda instalar o sistema operacional usando mídia de DVD)  

Os itens a seguir não são rigorosamente exigidos, mas são necessários para determinados recursos:  

- Sistema UEFI baseado em 2.3.1c e firmware que oferece suporte à inicialização segura  
- Trusted Platform Module  

-   Dispositivo gráfico e monitor capaz de Super VGA (1024 x 768) ou resolução superior  

-   Teclado e mouse Microsoft&reg; (ou outro dispositivo apontador compatível)  

-   Acesso à Internet (tarifas podem ser aplicadas)  

>[!NOTE]  
> Um chip Trusted Platform Module (TPM) não é estritamente necessário para instalar esta versão, embora seja necessário para usar determinados recursos, como Criptografia de Unidade de Disco BitLocker. Se o computador usar TPM, ele deverá atender a estes requisitos:  
>  
>- Os TPMs baseados em hardware devem implementar a versão 2.0 da especificação de TPM.  
>- Os TPMs que implementam a versão 2.0 devem ter um certificado EK previamente provisionado ao TPM pelo fornecedor de hardware ou ser capazes de serem recuperados pelo dispositivo durante a primeira inicialização.  
>- Os TPMs que implementam a versão 2.0 devem acompanhar os bancos PCR SHA-256 e implemente os PCRs 0 a 23 para SHA-256. É aceitável enviar os TPMs com um único banco de PCR de alternância que pode ser usado para as medições de SHA-1 e SHA-256.  
>- Uma opção de UEFI para desativar o TPM não é um requisito.  

## Instalação do Nano Server  
Para ver etapas detalhadas de instalação do Windows Server 2016 como um Nano Server, consulte [Instalar o Nano Server](Getting-Started-with-Nano-Server.md).

## Recursos adicionais
- [Requisitos de processamento do Windows](https://docs.microsoft.com/windows-hardware/design/minimum/windows-processor-requirements)
- [Comparação das edições Standard e Datacenter do Windows Server 2016](https://docs.microsoft.com/windows-server/get-started/2016-edition-comparison)
- [Requisitos do sistema Windows 10 ](https://www.microsoft.com/windows/windows-10-specifications#system-specifications)
- [Baixe a folha de dados de licenciamento do Windows Server 2016](http://download.microsoft.com/download/7/2/9/7290EA05-DC56-4BED-9400-138C5701F174/WS2016LicensingDatasheet.pdf)
