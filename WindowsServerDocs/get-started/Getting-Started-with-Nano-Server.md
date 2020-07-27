---
title: Instalar o Nano Server
description: Nova instalação, atualização, migração e avaliação do Nano Server
ms.prod: windows-server
manager: dougkim
ms.technology: server-nano
ms.date: 09/06/2017
ms.topic: get-started-article
ms.assetid: 2c2fa45b-6f3b-4663-b421-2da6ecc463bf
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 4002126ee6d9919c0a7fbfb3c068587c9acbecef
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86953678"
---
# <a name="install-nano-server"></a>Instalar o Nano Server

>Aplica-se a: Windows Server 2016

> [!IMPORTANT]
> A partir do Windows Server, versão 1709, o Nano Server estará disponível somente como uma [imagem do sistema operacional de base de contêiner](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Confira [Mudanças no Nano Server](nano-in-semi-annual-channel.md) para saber o que isso significa. 

O Windows Server 2016 oferece uma nova opção de instalação: Nano Server. O Nano Server é um sistema operacional de servidor administrado remotamente e otimizado para data centers e nuvens privadas. É semelhante ao Windows Server no modo Server Core, mas significativamente menor, não tem nenhum recurso de logon local e só oferece suporte a agentes, ferramentas e aplicativos de 64 bits. Ele ocupa bem menos espaço em disco, configura consideravelmente mais rápido e exige muito menos atualizações e reinicializações que o Windows Server. Quando ele reinicia, é muito mais rápido. A opção de instalação Nano Server está disponível para as edições Standard e Datacenter do Windows Server 2016.  

O Nano Server é ideal para vários cenários:  
  
-   Como um host de computação para máquinas virtuais Hyper-V, em clusters ou não  
  
-   Como host de armazenamento no Servidor de Arquivos de Escalabilidade Horizontal.  
  
-   Como servidor DNS  
  
-   Como servidor Web executando os Serviços de Informações da Internet (IIS)  
  
-   Como um host para aplicativos desenvolvidos usando padrões de aplicativo em nuvem e em execução em um sistema operacional convidado de máquina virtual ou contêiner  
  
## <a name="important-differences-in-nano-server"></a>Diferenças importantes no Nano Server

Como o Nano Server é otimizado como um sistema operacional leve para execução de aplicativos nativos de nuvem com base em contêineres e microsserviços ou como um host de datacenter ágil e econômico com um volume consideravelmente menor, há diferenças importantes entre as instalações Nano Server e Server Core ou Servidor com Experiência Desktop:

- O Nano Server é sem periféricos; não há capacidade de logon local nem interface gráfica do usuário.
- Há suporte apenas para agentes, ferramentas e aplicativos de 64 bits.
- O Nano Server não pode servir como um controlador de domínio do Active Directory.
- Não há suporte para a Política de Grupo. No entanto, você pode usar a [Configuração de Estado Desejado](/previous-versions//dn387184(v=vs.85)) para aplicar as configurações em escala.
- O Nano Server não pode ser configurado para usar um servidor proxy para acessar a Internet.
- Não há suporte para o Agrupamento NIC (especificamente, balanceamento de carga e failover ou LBFO). Em vez disso, há suporte para o SET (agrupamento incorporado do comutador).
- Não há suporte para o Microsoft Endpoint Configuration Manager e o System Center Data Protection Manager.
- Não há suporte para cmdlets do BPA (Analisador de Práticas Recomendadas) nem para a integração do BPA ao Gerenciador do Servidor.
- O Nano Server não dá suporte a HBAs (adaptadores de barramento do host) virtuais.
- O Nano Server não precisa ser ativado com uma chave do produto (Product Key). Ao funcionar como um host do Hyper-V, o Nano Server não é compatível com a AVMA [(ativação automática de máquina virtual)](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11)). Máquinas virtuais em execução em um host do Nano Server podem ser ativadas usando o KMS [(Serviço de Gerenciamento de Chaves)](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj612867(v=ws.11)) com uma chave de licença de volume genérico ou usando a [ativação baseada no Active Directory](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502534(v=ws.11)).
- A versão do Windows PowerShell fornecida com o Nano Server tem diferenças importantes. Para obter detalhes, consulte [PowerShell on Nano Server](PowerShell-on-Nano-Server.md) (PowerShell on Nano Server).
- Há suporte para o Nano Server apenas no modelo CBB (Branch Atual para Negócios) – não há versão de LTSB (Branch de Manutenção em Longo Prazo) para o Nano Server neste momento. Consulte a subseção a seguir para obter mais informações.

### <a name="current-branch-for-business"></a>Branch Atual para Negócios
O Nano Server é atendido com um modelo mais ativo, chamado CBB (Branch Atual para Negócios) para oferecer suporte a clientes que estão migrando em uma cadência de nuvem, usando rápidos ciclos de desenvolvimento. Nesse modelo, espera-se versões de atualização de recurso do Nano Server de duas a três vezes por ano. Esse modelo requer [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default.aspx) para Nano Servers implantados e operados em produção. Para manter o suporte, os administradores devem usar não mais de duas versões anteriores do CBB. No entanto, essas versões não atualizam automaticamente implantações existentes; os administradores executam a instalação manual de uma nova versão do CBB conforme a conveniência. Para informações adicionais, consulte [Windows Server 2016 new Current Branch for Business servicing option](https://cloudblogs.microsoft.com/windowsserver/2016/07/12/windows-server-2016-new-current-branch-for-business-servicing-option/) (Nova opção de manutenção da Branch Atual para Negócios do Windows Server 2016).

As opções de instalação Server Core e Servidor com Experiência Desktop ainda são atendidas no [modelo LTSB (Branch de Manutenção de Longo Prazo)](https://support.microsoft.com/lifecycle#gp%2Fgp_msl_policy), que abrange cinco anos de suporte base e cinco anos de suporte estendido.

## <a name="installation-scenarios"></a>Cenários de instalação

### <a name="evaluation"></a>Avaliação
Você pode obter uma cópia de avaliação de 180 dias licenciada do Windows Server em [Avaliações do Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016). Para testar o Nano Server, escolha **Nano Server | Opção EXE de 64 bits** e, em seguida, volte para o [Início Rápido do Nano Server](Nano-Server-Quick-Start.md) ou [Implantar o Nano Server](Deploy-Nano-Server.md) para começar.

### <a name="clean-installation"></a>Instalação limpa
Como você instala o Nano Server configurando um VHD, uma instalação limpa é o método de implantação mais simples e rápido.

- Para começar rapidamente com uma implantação básica do Nano Server usando DHCP para obter um endereço IP, confira [Início Rápido do Nano Server](Nano-Server-Quick-Start.md) 
- Se você já estiver familiarizado com os conceitos básicos do Nano Server, os tópicos mais detalhados, começando por [Implantar o Nano Server](Deploy-Nano-Server.md), oferecem um conjunto completo de instruções para personalizar as imagens, trabalhar com domínios, instalar pacotes para funções de servidor, outros recursos online e offline e muito mais.

> [!IMPORTANT]  
> Após a conclusão da instalação e imediatamente depois de ter instalado todas as funções e recursos de servidor necessários, procure por e instale as atualizações disponíveis para o Windows Server 2016. Para o Nano Server, confira a seção Gerenciar atualizações no Nano Server de [Gerenciar o Nano Server](Manage-Nano-Server.md).

### <a name="upgrade"></a>Atualizar, Atualização (Upgrade)
Como o Nano Server é novo no Windows Server 2016, não existe um caminho de atualização de versões anteriores do sistema operacional para o Nano Server.

### <a name="migration"></a>Migração
Como o Nano Server é novo no Windows Server 2016, não existe um caminho de migração de versões anteriores do sistema operacional para o Nano Server.
  
-------------------------------------
Se você precisar de uma opção de instalação diferente, [volte à página principal do Windows Server 2016](windows-server-2016.md) 

  


 
