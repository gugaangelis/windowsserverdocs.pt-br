---
title: Gerenciar remotamente hosts do Hyper-V
description: Descreve a compatibilidade de versão entre hosts Hyper-V e o Gerenciador do Hyper-V e como se conectar a hosts remotos em ambientes diferentes, incluindo entre domínios e autônomos.
ms.topic: article
ms.assetid: 2d34e98c-6134-479b-8000-3eb360b8b8a3
ms.author: benarm
author: BenjaminArmstrong
ms.date: 12/06/2016
ms.openlocfilehash: 2b0a7c93f5a6c6be7340c8d5b0a5bd93d21f1cec
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746651"
---
# <a name="remotely-manage-hyper-v-hosts-with-hyper-v-manager"></a>Gerenciar hosts do Hyper-V remotamente com o Gerenciador do Hyper-V

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows 10, Windows 8.1

Este artigo lista as combinações com suporte de hosts Hyper-V e versões do Gerenciador do Hyper-V e descreve como se conectar a hosts Hyper-V remotos e locais para que você possa gerenciá-los.

O Gerenciador do Hyper-V permite que você gerencie um pequeno número de hosts Hyper-V, remotos e locais. Ele é instalado quando você instala as ferramentas de gerenciamento do Hyper-V, que podem ser efetuadas por meio de uma instalação completa do Hyper-V ou de uma instalação somente de ferramentas. Fazer uma instalação somente de ferramentas significa que você pode usar as ferramentas em computadores que não atendem aos requisitos de hardware para hospedar o Hyper-V. Para obter detalhes sobre o hardware para hosts Hyper-V, consulte [requisitos do sistema](../System-requirements-for-Hyper-V-on-Windows.md).

Se o Gerenciador do Hyper-V não estiver instalado, consulte as [instruções](#install-hyper-v-manager) abaixo.

## <a name="supported-combinations-of-hyper-v-manager-and-hyper-v-host-versions"></a>Combinações com suporte do Gerenciador do Hyper-V e das versões de host do Hyper-V

Em alguns casos, você pode usar uma versão diferente do Gerenciador do Hyper-V do que a versão do Hyper-V no host, conforme mostrado na tabela. Ao fazer isso, o Gerenciador do Hyper-V fornece os recursos disponíveis para a versão do Hyper-V no host que você está gerenciando. Por exemplo, se você usar a versão do Gerenciador do Hyper-V no Windows Server 2012 R2 para gerenciar remotamente um host que executa o Hyper-V no Windows Server 2012, não será possível usar os recursos disponíveis no Windows Server 2012 R2 nesse host Hyper-V.

A tabela a seguir mostra quais versões de um host Hyper-V você pode gerenciar de uma versão específica do Gerenciador do Hyper-V. Somente as versões de sistema operacional com suporte são listadas. Para obter detalhes sobre o status de suporte de uma versão específica do sistema operacional, use o botão **Pesquisar o produto Lifecyle** na página [política de ciclo de vida da Microsoft](https://support.microsoft.com/lifecycle) . Em geral, as versões mais antigas do Gerenciador do Hyper-V só podem gerenciar um host Hyper-V executando a mesma versão ou a versão comparável do Windows Server.

|Versão do Gerenciador do Hyper-V | Versão do host do Hyper-V|
|---|---|
|Windows 2016, Windows 10|-Windows Server 2016 – todas as edições e opções de instalação, incluindo o nano Server e a versão correspondente do Hyper-V Server <br>-Windows Server 2012 R2 – todas as edições e opções de instalação e a versão correspondente do Hyper-V Server <br>-Windows Server 2012 – todas as edições e opções de instalação e a versão correspondente do Hyper-V Server <br> – Windows 10 <br> – Windows 8.1  |
| Windows Server 2012 R2, Windows 8.1 | -Windows Server 2012 R2 – todas as edições e opções de instalação e a versão correspondente do Hyper-V Server <br>-Windows Server 2012 – todas as edições e opções de instalação e a versão correspondente do Hyper-V Server <br>– Windows 8.1
| Windows Server 2012 | -Windows Server 2012 – todas as edições e opções de instalação e a versão correspondente do Hyper-V Server
| Windows Server 2008 R2 Service Pack 1, Windows 7 Service Pack 1 | -Windows Server 2008 R2 – todas as edições e opções de instalação e a versão correspondente do Hyper-V Server
| Windows Server 2008, Windows Vista Service Pack 2 | -Windows Server 2008 – todas as edições e opções de instalação e a versão correspondente do Hyper-V Server

>[!NOTE]
>O suporte ao Service Pack foi encerrado para o Windows 8 em 12 de janeiro de 2016. Para obter mais informações, consulte as [perguntas frequentes Windows 8.1](https://support.microsoft.com/help/18581).

## <a name="connect-to-a-hyper-v-host"></a>Conectar-se a um host Hyper-V

Para se conectar a um host Hyper-V do Gerenciador do Hyper-V, clique com o botão direito do mouse em **Gerenciador do Hyper-v** no painel esquerdo e clique em **conectar ao servidor**.

## <a name="manage-hyper-v-on-a-local-computer"></a>Gerenciar o Hyper-V em um computador local

O Gerenciador do Hyper-V não lista os computadores que hospedam o Hyper-V até que você adicione o computador, incluindo um computador local. Para fazer isso:

1. No painel esquerdo, clique com o botão direito do mouse em **Gerenciador do Hyper-V**.
2. Clique em **conectar ao servidor**.
3. Em **Selecionar computador**, clique em **computador local** e em **OK**.

Se você não conseguir se conectar:

* É possível que apenas as ferramentas do Hyper-V sejam instaladas. Para verificar se a plataforma Hyper-V está instalada, procure o serviço gerenciamento de máquinas virtuais. /(Abra o aplicativo de área de trabalho serviços: clique em **Iniciar**, clique na caixa **Iniciar pesquisa** , digite **Services. msc**e pressione **Enter**. Se o serviço gerenciamento de máquinas virtuais não estiver listado, instale a plataforma Hyper-V seguindo as instruções em [instalar o Hyper-v](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).
* Verifique se o hardware atende aos requisitos. Consulte [requisitos do sistema](../System-requirements-for-Hyper-V-on-Windows.md).
* Verifique se sua conta de usuário pertence ao grupo Administradores ou ao grupo Administradores do Hyper-V.

## <a name="manage-hyper-v-hosts-remotely"></a>Gerenciar hosts Hyper-V remotamente

Para gerenciar hosts remotos do Hyper-V, habilite o gerenciamento remoto no computador local e no host remoto.

No Windows Server, abra Gerenciador do servidor \> gerenciamento remoto **do servidor local** \> **Remote management** e, em seguida, clique em **permitir conexões remotas com este computador**.

Ou, do sistema operacional, abra o Windows PowerShell como administrador e execute:

```
Enable-PSRemoting
```

### <a name="connect-to-hosts-in-the-same-domain"></a>Conectar-se a hosts no mesmo domínio

Para Windows 8.1 e anteriores, o gerenciamento remoto funciona somente quando o host está no mesmo domínio e sua conta de usuário local também está no host remoto.

Para adicionar um host remoto do Hyper-V ao Gerenciador do Hyper-V, selecione **outro computador** na caixa de diálogo **Selecionar computador** e digite o nome de host do host remoto, o nome NetBIOS ou o FQDN do domínio totalmente qualificado \( \) .

O Gerenciador do Hyper-V no Windows Server 2016 e no Windows 10 oferece mais tipos de conexão remota do que as versões anteriores, descritas nas seções a seguir.

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-as-a-different-user"></a>Conectar-se a um host remoto do Windows 2016 ou Windows 10 como um usuário diferente

Isso permite que você se conecte ao host Hyper-V quando não estiver executando no computador local como um usuário que seja membro do grupo Administradores do Hyper-V ou do grupo Administradores no host Hyper-V. Para fazer isso:

1. No painel esquerdo, clique com o botão direito do mouse em **Gerenciador do Hyper-V**.
1. Clique em **conectar ao servidor**.
1. Selecione **conectar-se como outro usuário** na caixa de diálogo **Selecionar computador** .
1. Selecione **definir usuário**.

>[!NOTE]
> Isso só funcionará para hosts **remotos** do windows Server 2016 ou do Windows 10.

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-using-ip-address"></a>Conectar-se a um host remoto do Windows 2016 ou Windows 10 usando o endereço IP

Para fazer isso:

1. No painel esquerdo, clique com o botão direito do mouse em **Gerenciador do Hyper-V**.
1. Clique em **conectar ao servidor**.
1. Digite o endereço IP no campo de texto **outro computador** .

>[!NOTE]
> Isso só funcionará para hosts **remotos** do windows Server 2016 ou do Windows 10.

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-outside-your-domain-or-with-no-domain"></a>Conectar-se a um host remoto do Windows 2016 ou Windows 10 fora do seu domínio ou sem nenhum domínio

Para fazer isso:

1. No host Hyper-V a ser gerenciado, abra uma sessão do Windows PowerShell como administrador.

1. Crie as regras de firewall necessárias para zonas de rede privada:

   ```
   Enable-PSRemoting
   ```

2. Para permitir o acesso remoto em zonas públicas, habilite as regras de firewall para CredSSP e WinRM:

   ```
   Enable-WSManCredSSP -Role server
   ```

    Para obter detalhes, consulte [Enable-PSRemoting](/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-7) e [Enable-WSManCredSSP](/powershell/module/microsoft.wsman.management/enable-wsmancredssp?view=powershell-7).

Em seguida, configure o computador que você usará para gerenciar o host do Hyper-V.

1. Abra uma sessão do Windows PowerShell como administrador.
1. Execute estes comandos:

     ```
     Set-Item WSMan:\localhost\Client\TrustedHosts -Value "fqdn-of-hyper-v-host"
     ```
     ```
     Enable-WSManCredSSP -Role client -DelegateComputer "fqdn-of-hyper-v-host"
     ```
1. Talvez você também precise configurar a seguinte política de Grupo:
    * **Configuração** \> do computador **Modelos administrativos** \> **Sistema** \> **Delegação** \> de credenciais **Permitir a delegação de novas credenciais com autenticação de servidor somente NTML**
    * Clique em **habilitar** e adicionar *WSMan/FQDN-of-Hyper-v-host*.
1. Abra o **Gerenciador do Hyper-V**.
1. No painel esquerdo, clique com o botão direito do mouse em **Gerenciador do Hyper-V**.
1. Clique em **conectar ao servidor**.

>[!NOTE]
> Isso só funcionará para hosts **remotos** do windows Server 2016 ou do Windows 10.

Para obter detalhes do cmdlet, consulte [set-item](/powershell/module/microsoft.powershell.management/set-item?view=powershell-7) e [Enable-WSManCredSSP](/powershell/module/microsoft.wsman.management/enable-wsmancredssp?view=powershell-7).

## <a name="install-hyper-v-manager"></a>Instalar o Gerenciador do Hyper-V

Para usar uma ferramenta de interface do usuário, escolha aquela apropriada para o sistema operacional no computador em que você executará o Gerenciador do Hyper-V:

No Windows Server, abra Gerenciador do servidor \> **gerenciar** \> **adicionar funções e recursos**. Vá para a página **recursos** e expanda **ferramentas de administração de servidor remoto** ferramentas de administração \> de **função** ferramentas \> **de gerenciamento do Hyper-V**.

No Windows, o Gerenciador do Hyper-V está disponível em [qualquer sistema operacional Windows que inclua o Hyper-v](/virtualization/hyper-v-on-windows/reference/hyper-v-requirements).

1. Na área de trabalho do Windows, clique no botão Iniciar e comece a digitar **programas e recursos**.
1. Em resultados da pesquisa, clique em **programas e recursos**.
1. No painel esquerdo, clique em **Ativar ou desativar recursos do Windows**.
1. Expanda a pasta Hyper-V e **clique em ferramentas de gerenciamento do Hyper-v**.
1. Para instalar o Gerenciador do Hyper-V, clique em **ferramentas de gerenciamento do Hyper-v**. Se você também quiser instalar o módulo Hyper-V, clique nessa opção.

Para usar o Windows PowerShell, execute o seguinte comando como administrador:

```
add-windowsfeature rsat-hyper-v-tools
```

## <a name="additional-references"></a>Referências adicionais

[Instalar o Hyper-V](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md)