---
title: Gerenciar remotamente os hosts Hyper-V
description: Descreve a compatibilidade de versão entre hosts Hyper-V e o Gerenciador do Hyper-V e como se conectar a hosts remotos em ambientes diferentes, incluindo o domínio cruzado e autônomo.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d34e98c-6134-479b-8000-3eb360b8b8a3
author: KBDAzure
ms.author: kathydav
ms.date: 12/06/2016
ms.openlocfilehash: d4d9f2dd3727e196bb6893fd5041fa3f08c30796
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453184"
---
# <a name="remotely-manage-hyper-v-hosts-with-hyper-v-manager"></a>Gerenciar remotamente os hosts do Hyper-V com o Gerenciador do Hyper-V

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows 10, Windows 8.1

Este artigo lista as combinações suportadas de hosts Hyper-V e as versões do Hyper-V Manager e descreve como conectar-se aos hosts do Hyper-V locais e remotos para que você possa gerenciá-los. 

Gerenciador do Hyper-V permite gerenciar um pequeno número de hosts do Hyper-V, locais e remotos. Ele é instalado quando você instala as ferramentas de gerenciamento do Hyper-V, que pode ser feito por meio de uma completa de instalação do Hyper-V ou uma instalação somente ferramentas. Fazendo um meio de instalação somente ferramentas você pode usar as ferramentas em computadores que não atendem os requisitos de hardware para o host do Hyper-V. Para obter detalhes sobre o hardware para hosts Hyper-V, consulte [requisitos de sistema](../System-requirements-for-Hyper-V-on-Windows.md).

Se o Gerenciador do Hyper-V não estiver instalado, consulte o [instruções](#install-hyper-v-manager) abaixo.

## <a name="supported-combinations-of-hyper-v-manager-and-hyper-v-host-versions"></a>Combinações compatíveis do Gerenciador do Hyper-V e as versões de host do Hyper-V

Em alguns casos você pode usar uma versão diferente do Gerenciador do Hyper-V que a versão do Hyper-V no host, conforme mostrado na tabela. Quando você fizer isso, o Gerenciador do Hyper-V fornece os recursos disponíveis para a versão do Hyper-V no host que você está gerenciando. Por exemplo, se você usar a versão do Gerenciador do Hyper-V no Windows Server 2012 R2 para gerenciar remotamente um host que executa o Hyper-V no Windows Server 2012, você não poderá usar os recursos disponíveis no Windows Server 2012 R2 no host Hyper-V.

A tabela a seguir mostra quais versões de um host Hyper-V que você pode gerenciar em uma versão específica do Gerenciador do Hyper-V. Somente sistema operacional com suporte as versões são listadas. Para obter detalhes sobre o status de suporte de uma versão de sistema operacional específico, use o **ciclo de vida de produto de pesquisa** botão a [Lifecycle Policy Microsoft](https://support.microsoft.com/lifecycle) página. Em geral, as versões mais antigas do Gerenciador do Hyper-V só podem gerenciar um host Hyper-V executando a mesma versão ou versão do Windows Server comparável.

|Versão do Gerenciador do Hyper-V | Versão do host do Hyper-V|
|---|---|
|Windows 2016, Windows 10|-Windows Server 2016 — todas as edições e opções de instalação, incluindo o Nano Server e a versão correspondente do Hyper-V Server <br>-Windows Server 2012 R2 — todas as edições e opções de instalação e a versão correspondente do Hyper-V Server <br>-Windows Server 2012 — todas as edições e opções de instalação e a versão correspondente do Hyper-V Server <br> - Windows 10 <br> - Windows 8.1  |
| Windows Server 2012 R2, Windows 8.1 | -Windows Server 2012 R2 — todas as edições e opções de instalação e a versão correspondente do Hyper-V Server <br>-Windows Server 2012 — todas as edições e opções de instalação e a versão correspondente do Hyper-V Server <br>- Windows 8.1
| Windows Server 2012 | -Windows Server 2012 — todas as edições e opções de instalação e a versão correspondente do Hyper-V Server
| Windows Server 2008 R2 Service Pack 1, Windows 7 Service Pack 1 | – Windows Server 2008 R2 — todas as edições e opções de instalação e a versão correspondente do Hyper-V Server
| Windows Server 2008, Windows Vista Service Pack 2 | -Windows Server 2008 — todas as edições e opções de instalação e a versão correspondente do Hyper-V Server

>[!NOTE]
>Suporte ao Service pack foi encerrado para o Windows 8 em 12 de janeiro de 2016. Para obter mais informações, consulte o [perguntas frequentes sobre o Windows 8.1](https://support.microsoft.com/help/18581).

## <a name="connect-to-a-hyper-v-host"></a>Conectar a um host Hyper-V

Para se conectar a um host Hyper-V do Gerenciador do Hyper-V, clique com botão direito **Gerenciador do Hyper-V** no painel esquerdo e, em seguida, clique **conectar ao servidor**.

## <a name="manage-hyper-v-on-a-local-computer"></a>Gerenciar o Hyper-V em um computador local

Gerenciador do Hyper-V não lista todos os computadores que hospedam o Hyper-V até que você adicione o computador, incluindo um computador local. Para fazer isso:

1. No painel esquerdo, clique com botão direito **Gerenciador do Hyper-V**.
2. Clique em **se conectar ao servidor**.
3. Partir **Selecionar computador**, clique em **computador Local** e, em seguida, clique em **Okey**.

Se você não pode se conectar:

* É possível que apenas as ferramentas de Hyper-V estão instaladas. Para verificar se a plataforma Hyper-V está instalada, procure o serviço de gerenciamento de máquinas virtuais. / (Abra o aplicativo de serviços da área de trabalho: clique **Start**, clique o **Iniciar pesquisa** , digite **Services. msc**e, em seguida, pressione **Enter**. Se o serviço de gerenciamento de máquina Virtual não estiver listado, instale a plataforma Hyper-V, seguindo as instruções em [instalar o Hyper-V](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).
* Verifique se o seu hardware cumpre os requisitos. Ver [requisitos de sistema](../System-requirements-for-Hyper-V-on-Windows.md).
* Verifique se sua conta de usuário pertence ao grupo Administradores ou do grupo de administradores do Hyper-V.

## <a name="manage-hyper-v-hosts-remotely"></a>Gerenciar hosts Hyper-V remotamente  

Para gerenciar hosts remotos do Hyper-V, habilite o gerenciamento remoto no computador local e no host remoto.

No Windows Server, abra o Gerenciador do servidor \> **servidor Local** \> **gerenciamento remoto** e, em seguida, clique em **permitir conexões remotas com este computador**. 

Ou, de qualquer sistema operacional, abra o Windows PowerShell como administrador e execute: 

```
Enable-PSRemoting
```

### <a name="connect-to-hosts-in-the-same-domain"></a>Conectar-se aos hosts no mesmo domínio

Para Windows 8.1 e versões anteriores, o gerenciamento remoto funciona somente quando o host está no mesmo domínio e também é sua conta de usuário local no host remoto.

Para adicionar um host remoto do Hyper-V ao Gerenciador do Hyper-V, selecione **outro computador** na **Selecionar computador** caixa de diálogo e digite o nome do host do host remoto, nome NetBIOS ou o nome de domínio totalmente qualificado \(FQDN\).

Gerenciador do Hyper-V no Windows Server 2016 e Windows 10 oferece mais tipos de conexão remota que as versões anteriores, descritos nas seções a seguir.  

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-as-a-different-user"></a>Conectar-se a um host remoto do Windows 2016 ou Windows 10 como um usuário diferente

Isso permite que você se conectar ao host do Hyper-V quando você não estiver executando no computador local como um usuário que seja membro do grupo Administradores do Hyper-V ou o grupo de administradores no host Hyper-V. Para fazer isso:

1. No painel esquerdo, clique com botão direito **Gerenciador do Hyper-V**.
1. Clique em **se conectar ao servidor**.
1. Selecione **conectar como outro usuário** na **Selecionar computador** caixa de diálogo.
1. Selecione **definir usuário**.

>[!NOTE]
> Isso só funcionará para o Windows Server 2016 ou Windows 10 **remoto** hosts.

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-using-ip-address"></a>Conectar a um host remoto Windows 2016 ou Windows 10 usando o endereço IP

Para fazer isso:

1. No painel esquerdo, clique com botão direito **Gerenciador do Hyper-V**.
1. Clique em **se conectar ao servidor**.
1. Digite o endereço IP para o **outro computador** campo de texto.

>[!NOTE]
> Isso só funcionará para o Windows Server 2016 ou Windows 10 **remoto** hosts.

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-outside-your-domain-or-with-no-domain"></a>Conectar-se a um Windows 2016 ou Windows 10 host remoto fora de seu domínio ou sem domínio

Para fazer isso:

1. No host do Hyper-V a serem gerenciados, abra uma sessão do Windows PowerShell como administrador.

1. Crie as regras de firewall necessárias para zonas da rede privada:   
   
   ```
   Enable-PSRemoting
   ```

2. Para permitir acesso remoto em zonas públicas, habilite as regras de firewall para CredSSP e WinRM:
  
   ```
   Enable-WSManCredSSP -Role server
   ```

    Para obter detalhes, consulte [Enable-PSRemoting](https://technet.microsoft.com/library/hh849694.aspx) e [Enable-WSManCredSSP](https://technet.microsoft.com/library/hh849872.aspx).

Em seguida, configure o computador que você usará para gerenciar o host Hyper-V.

1. Abra uma sessão do Windows PowerShell como administrador.
1. Execute estes comandos:

     ```
     Set-Item WSMan:\localhost\Client\TrustedHosts -Value "fqdn-of-hyper-v-host"
     ```
     ```
     Enable-WSManCredSSP -Role client -DelegateComputer "fqdn-of-hyper-v-host"
     ```
1. Talvez você também precise configurar a política de grupo a seguir: 
    * **Configuração do computador** \> **modelos administrativos** \> **sistema** \> **adelegaçãodecredenciais** \> **Permitir delegação de novas credenciais com autenticação de servidor somente NTLM**
    * Clique em **habilitar** e adicione *wsman/fqdn-de-hyper-v-host*.
1. Abra **Gerenciador do Hyper-V**.
1. No painel esquerdo, clique com botão direito **Gerenciador do Hyper-V**.
1. Clique em **se conectar ao servidor**.

>[!NOTE]
> Isso só funcionará para o Windows Server 2016 ou Windows 10 **remoto** hosts.

Para obter detalhes do cmdlet, consulte [Set-Item](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/set-item) e [Enable-WSManCredSSP](https://technet.microsoft.com/library/hh849872.aspx).

## <a name="install-hyper-v-manager"></a>Instalar o Gerenciador do Hyper-V

Para usar uma ferramenta de interface do usuário, escolha o apropriado para o sistema operacional no computador onde você executará o Gerenciador do Hyper-V:

No Windows Server, abra o Gerenciador do servidor \> **gerenciar** \> **adicionar funções e recursos**. Mover para o **recursos** da página e expanda **ferramentas de administração de servidor remoto** \> **ferramentas de administração de função** \>  **Ferramentas de gerenciamento do Hyper-V**. 

No Windows, o Gerenciador do Hyper-V está disponível no [qualquer sistema operacional Windows que inclui o Hyper-V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_compatibility).

1. Na área de trabalho do Windows, clique no botão Iniciar e comece a digitar **programas e recursos**. 
1. Nos resultados da pesquisa, clique em **programas e recursos**.
1. No painel esquerdo, clique em **ativar ou desativar recursos do Windows ativar**.
1. Expanda a pasta do Hyper-V, e **clique em ferramentas de gerenciamento do Hyper-V**.
1. Para instalar o Gerenciador do Hyper-V, clique em **as ferramentas de gerenciamento do Hyper-V**. Se você quiser instalar também o módulo do Hyper-V, clique em que a opção.

Para usar o Windows PowerShell, execute o comando a seguir como administrador:

```
add-windowsfeature rsat-hyper-v-tools
```

## <a name="see-also"></a>Consulte também  
 
[Instalar o Hyper-V](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md) 

