---
title: "Trabalhar com regras de políticas de restrição de Software"
description: "Segurança do Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a8047d5-9bb9-4bed-bc8f-583a237731e2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 2dd1810b50f4f02be99eb2e2c0893501f99d1e93
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="work-with-software-restriction-policies-rules"></a>Trabalhar com regras de políticas de restrição de Software

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve os procedimentos para trabalhar com o certificado, caminho, internet regras de zona e hash usando diretivas de restrição de Software.

## <a name="introduction"></a>Introdução
Com as políticas de restrição de software, você pode proteger seu ambiente de computação contra software não confiável, identificando e especificando qual software tem permissão para executar. Você pode definir um nível de segurança padrão **Unrestricted** ou **não permitido** para um objeto de política de grupo (GPO) para que o software ou pode ou não permissão para serem executados por padrão. Você pode criar exceções para esse nível de segurança padrão através da criação de regras de políticas de software específico de restrição de software. Por exemplo, se o nível de segurança padrão é definido como **não permitido**, você pode criar regras que permitam software específico ser executado. Os tipos de regras são da seguinte maneira:

-   **Regras de certificado**

    Para obter os procedimentos, consulte [trabalhando com regras de certificado](#BKMK_Cert_Rules).

-   **Regras de hash**

    Para obter os procedimentos, consulte [trabalhando com regras de hash](#BKMK_Hash_Rules).

-   **Regras de zona da Internet**

    Para obter os procedimentos, consulte [trabalhando com regras de zona da Internet](#BKMK_Internet_Zone_Rules).

-   **Regras de caminho**

    Para obter os procedimentos, consulte [trabalhando com regras de caminho](#BKMK_Path_Rules).

Para obter informações sobre outras tarefas para gerenciar políticas de restrição de Software, consulte [administrar políticas de restrição de Software](administer-software-restriction-policies.md).

## <a name="BKMK_Cert_Rules"></a>Trabalhando com regras de certificado
Políticas de restrição de software também podem identificar um software pelo certificado de assinatura. Você pode criar uma regra de certificado que identifica o software e, em seguida, permite ou não permitir que o software em execução, dependendo do nível de segurança. Por exemplo, você pode usar regras de certificado para confiar automaticamente no software de uma fonte confiável em um domínio sem avisar o usuário. Você também pode usar regras de certificado para executar arquivos em áreas não permitidas do seu sistema operacional. Regras de certificado não estão habilitadas por padrão.

Quando as regras são criadas para o domínio usando a política de grupo, você deve ter permissões para criar ou modificar um objeto de política de grupo. Se você estiver criando regras para o computador local, você deve ter credenciais administrativas no computador.

#### <a name="to-create-a-certificate-rule"></a>Para criar uma regra de certificado

1.  Abra as políticas de restrição de Software.

2.  Na árvore de console ou no painel de detalhes, clique com botão direito **regras adicionais**e clique em **nova regra de certificado**.

3.  Clique em **procurar**e, em seguida, selecione um certificado ou arquivo assinado.

4.  Em **nível de segurança**, clique em **não permitido** ou **Unrestricted**.

5.  Em **descrição**, digite uma descrição para essa regra e clique em **Okey**.

> [!NOTE]
> -   Talvez seja necessário criar uma nova configuração de política de restrição de software para o objeto de política de grupo (GPO) se você ainda não tiver feito isso.
> -   Regras de certificado não estão habilitadas por padrão.
> -   Os únicos tipos de arquivo que são afetados pelas regras de certificado são aqueles que estão listados em **tipos de arquivo designados** no painel de detalhes para políticas de restrição de Software. Há uma lista dos tipos de arquivo designados que é compartilhada por todas as regras.
> -   Para políticas de restrição de software entrem em vigor, os usuários devem atualizar configurações de política de logoff e logon em seus computadores.
> -   Quando mais de uma regra de políticas de restrição de software é aplicada a configurações de política, há uma precedência de regras para lidar com conflitos.

### <a name="enabling-certificate-rules"></a>Habilitar regras de certificado
Há diferentes procedimentos para ativar regras de certificado, dependendo do seu ambiente:

-   [Para o computador local](#BKMK_1)

-   [Em um objeto de política de grupo e você está em um servidor que tenha ingressado em um domínio](#BKMK_2)

-   [Em um objeto de política de grupo e você está em um controlador de domínio ou em estação de trabalho com as ferramentas de administração de servidor remoto instalado](#BKMK_3)

-   [Domínio somente controladores e você está em um controlador de domínio ou em uma estação de trabalho que tem o pacote de ferramentas de administração servidor remoto instalado](#BKMK_4)

#### <a name="BKMK_1"></a>Habilitar regras de certificado para o computador local

1.  Abra configurações de segurança Local.

2.  Na árvore de console, clique em **opções de segurança** localizadas em políticas de configurações/Local de segurança.

3.  No painel de detalhes, clique duas vezes em **configurações do sistema: usar regras de certificado em arquivos executáveis do Windows para diretivas de restrição de Software**.

4.  Siga um destes procedimentos e clique em **Okey**:

    -   Para habilitar regras de certificado, clique em **Enabled**.

    -   Para desabilitar as regras de certificado, clique em **desabilitada**.

#### <a name="BKMK_2"></a>Para habilitar regras de certificado para um objeto de política de grupo e você está em um servidor que tenha ingressado em um domínio

1.  Abra o Console de gerenciamento Microsoft (MMC).

2.  Sobre o **arquivo** menu, clique em **Adicionar/remover snap-in**e clique em **adicionar**.

3.  Clique em **Editor de objeto de política de Grupo Local**e clique em **adicionar**.

4.  Em **Selecionar objeto de diretiva de grupo**, clique em **procurar**.

5.  Em **procurar um objeto de política de grupo**, selecione um objeto de política de grupo (GPO) no domínio apropriado, site ou unidade organizacional- ou crie um novo e clique em **concluir**.

6.  Clique em **fechar**e clique em **Okey**.

7.  Na árvore de console, clique em **opções de segurança** localizada sob *GroupPolicyObject* [*ComputerName*] políticas de configurações de configurações de segurança de diretiva de computador Configuration/Windows/Local /.

8.  No painel de detalhes, clique duas vezes em **configurações do sistema: usar regras de certificado em arquivos executáveis do Windows para diretivas de restrição de Software**.

9. Se essa configuração de política ainda não foi definida, selecione o **definir essas configurações de política** caixa de seleção.

10. Siga um destes procedimentos e clique em **Okey**:

    -   Para habilitar regras de certificado, clique em **Enabled**.

    -   Para desabilitar as regras de certificado, clique em **desabilitada**.

#### <a name="BKMK_3"></a>Para habilitar o certificado regras para um objeto de política de grupo, e você estiver em um controlador de domínio ou em uma estação de trabalho que tenha as ferramentas de administração de servidor remoto instalado

1.  Abra Usuários e computadores Active Directory.

2.  Na árvore de console, clique com botão direito do objeto de política de grupo (GPO) para o qual você deseja habilitar regras de certificado.

3.  Clique em **propriedades**e, em seguida, clique no **política de grupo** guia.

4.  Clique em **editar** para abrir o GPO que você deseja editar. Você também pode clicar em **nova** para criar um novo GPO e clique em **editar**.

5.  Na árvore de console, clique em **opções de segurança** localizada sob *GroupPolicyObject*[*ComputerName*] políticas de configurações/Local de configurações de segurança de Configuration/Windows/computador de política.

6.  No painel de detalhes, clique duas vezes em **configurações do sistema: usar regras de certificado em arquivos executáveis do Windows para diretivas de restrição de Software**.

7.  Se essa configuração de política ainda não foi definida, selecione o **definir essas configurações de política** caixa de seleção.

8.  Siga um destes procedimentos e clique em **Okey**:

    -   Para habilitar regras de certificado, clique em **Enabled**.

    -   Para desabilitar as regras de certificado, clique em **desabilitada**.

#### <a name="BKMK_4"></a>Para habilitar regras de certificado somente controladores de domínio e você estiverem em um controlador de domínio ou em uma estação de trabalho que tenha as ferramentas de administração de servidor remoto instalado

1.  Abra configurações de segurança de controlador de domínio.

2.  Na árvore de console, clique em **opções de segurança** localizada sob *GroupPolicyObject* [*ComputerName*] políticas de configurações/Local de configurações de segurança de Configuration/Windows/computador de política.

3.  No painel de detalhes, clique duas vezes em **configurações do sistema: usar regras de certificado em arquivos executáveis do Windows para diretivas de restrição de Software**.

4.  Se essa configuração de política ainda não foi definida, selecione o **definir essas configurações de política** caixa de seleção.

5.  Siga um destes procedimentos e clique em **Okey**:

    -   Para habilitar regras de certificado, clique em **Enabled**.

    -   Para desabilitar as regras de certificado, clique em **desabilitada**.

> [!NOTE]
> Você deve executar este procedimento para que regras de certificado efetivadas.

### <a name="set-trusted-publisher-options"></a>Definir opções de editores confiáveis
Assinatura de software está sendo usado por um número cada vez maior de fornecedores de software e os desenvolvedores de aplicativos para verificar se os aplicativos vêm de uma fonte confiável. No entanto, muitos usuários não entender ou prestar atenção pouco aos certificados de autenticação associados a aplicativos que instalam.

As configurações de política no **editores confiáveis** guia da política de validação de caminho do certificado permite que os administradores controlem os certificados que podem ser aceitos como proveniente de um editor confiável.

##### <a name="to-configure-the-trusted-publishers-policy-settings-for-a-local-computer"></a>Para definir as configurações de política de fornecedores confiáveis para um computador local

1.  Sobre o **iniciar** de tela, digite**gpedit.msc** e pressione ENTER.

2.  Na árvore de console em **Local do computador diretiva \ Windows \ Configurações**, clique em **políticas de chave pública**.

3.  Clique duas vezes em **configurações de validação de caminho do certificado**e, em seguida, clique no **editores confiáveis** guia.

4.  Selecione o **definir essas configurações de política** caixa de seleção, selecione as configurações de política que você deseja aplicar e clique em **Okey** para aplicar as novas configurações.

##### <a name="to-configure-the-trusted-publishers-policy-settings-for-a-domain"></a>Para definir as configurações de política de fornecedores confiáveis para um domínio

1.  Abrir **gerenciamento de política de grupo**.

2.  Na árvore de console, clique duas vezes em **objetos de política de grupo** na floresta e no domínio que contém o **política de domínio padrão** objeto de política de grupo (GPO) que você deseja editar.

3.  Clique com botão direito do **política de domínio padrão** GPO e clique em **editar**.

4.  Na árvore de console em **\ Configurações de segurança do computador \ Configurações**, clique em **políticas de chave pública**.

5.  Clique duas vezes em **configurações de validação de caminho do certificado**e, em seguida, clique no **editores confiáveis** guia.

6.  Selecione o **definir essas configurações de política** caixa de seleção, selecione as configurações de política que você deseja aplicar e clique em **Okey** para aplicar as novas configurações.

##### <a name="to-allow-only-administrators-to-manage-certificates-used-for-code-signing-for-a-local-computer"></a>Para permitir que apenas os administradores gerenciar certificados usados para código de assinatura de um computador local

1.  Sobre o **iniciar** tela, tipo, **gpedit. msc** no **pesquisar programas e arquivos** ou no Windows 8, na área de trabalho e pressione ENTER.

2.  Na árvore de console em **política de domínio padrão** ou **política de computador Local**, clique duas vezes em **configuração do computador**, **configurações do Windows**, e **configurações de segurança**e clique em **políticas de chave pública**.

3.  Clique duas vezes em **configurações de validação de caminho do certificado**e, em seguida, clique no **editores confiáveis** guia.

4.  Selecione o **definir essas configurações de política** caixa de seleção.

5.  Em **confiável de gerenciamento de fornecedor**, clique em **permitir que somente todos os administradores gerenciem Editores confiáveis**e clique em **Okey** para aplicar as novas configurações.

##### <a name="to-allow-only-administrators-to-manage-certificates-used-for-code-signing-for-a-domain"></a>Para permitir que apenas os administradores gerenciar certificados usados para assinatura para um domínio de código

1.  Abrir **gerenciamento de política de grupo**.

2.  Na árvore de console, clique duas vezes em **objetos de política de grupo** na floresta e no domínio que contém o **política de domínio padrão** GPO que você deseja editar.

3.  Clique com botão direito do **política de domínio padrão** GPO e clique em **editar**.

4.  Na árvore de console em **\ Configurações de segurança do computador \ Configurações**, clique em **políticas de chave pública**.

5.  Clique duas vezes em **configurações de validação de caminho do certificado**e, em seguida, clique no **editores confiáveis** guia.

6.  Selecione o **definir essas configurações de política** caixa de seleção, implementar as alterações que você deseja e clique em **Okey** para aplicar as novas configurações.

## <a name="BKMK_Hash_Rules"></a>Trabalhando com regras de hash
Um hash é uma série de bytes com um tamanho fixo que identifica com exclusividade de um arquivo ou programa de software. O hash é calculado por um algoritmo de hash. Quando uma regra de hash é criada para um programa de software, políticas de restrição de software calculam um hash do programa. Quando um usuário tenta abrir um programa de software, um hash do programa é comparado com as regras de hash existentes para as políticas de restrição de software. O hash de um programa de software é sempre o mesmo, independentemente de onde o programa está localizado no computador. No entanto, se um programa de software for alterado de qualquer forma, o hash dela também muda, e ele não corresponde mais ao hash da regra de hash para políticas de restrição de software.

Por exemplo, você pode criar uma regra de hash e defina a segurança de nível de **não permitido** para impedir que os usuários executem um determinado arquivo. Um arquivo pode ser renomeado ou movido para outra pasta e ainda assim o mesmo hash. No entanto, todas as alterações para o próprio arquivo também alterar seu valor de hash e permitir que o arquivo ignorar as restrições.

#### <a name="to-create-a-hash-rule"></a>Para criar uma regra de hash

1.  Abra as políticas de restrição de Software.

2.  Na árvore de console ou no painel de detalhes, clique com botão direito **regras adicionais**e clique em **nova regra de Hash**.

3.  Clique em **procurar** para encontrar um arquivo.

    > [!NOTE]
    > No Windows XP é possível colar um hash previamente calculado em **hash do arquivo**. No Windows Server 2008 R2, Windows 7 e versões posteriores, essa opção não está disponível.

4.  Em **nível de segurança**, clique em **não permitido** ou **Unrestricted**.

5.  Em **descrição**, digite uma descrição para essa regra e clique em **Okey**.

> [!NOTE]
> -   Ele pode ser necessário criar uma nova configuração de política de restrição de software para o objeto de política de grupo (GPO) se você ainda não tiver feito isso.
> -   Uma regra de hash pode ser criada para um vírus ou um cavalo de Troia para impedir a execução.
> -   Se você quiser que outras pessoas usem uma regra de hash para que não pode executar um vírus, calcular o hash do vírus usando políticas de restrição de software e email, em seguida, o valor de hash para outras pessoas. Nunca email o vírus.
> -   Se um vírus tiver sido enviado por email, você também pode criar uma regra de caminho para impedir a execução de anexos de email.
> -   Um arquivo que é renomeado ou movido para outra pasta ainda terá o mesmo hash. Qualquer alteração para o próprio arquivo resulta em um hash diferente.
> -   Os únicos tipos de arquivo que são afetados pelas regras de hash são aqueles que estão listados em **tipos de arquivo designados** no painel de detalhes para políticas de restrição de Software. Há uma lista dos tipos de arquivo designados que é compartilhada por todas as regras.
> -   Para políticas de restrição de software entrem em vigor, os usuários devem atualizar configurações de política de logoff e logon em seus computadores.
> -   Quando mais de uma regra de políticas de restrição de software é aplicada a configurações de política, há uma precedência de regras para lidar com conflitos.

## <a name="BKMK_Internet_Zone_Rules"></a>Trabalhando com regras de zona da Internet
Regras de zona de Internet se aplicam somente para pacotes do Windows Installer. Uma regra de zona pode identificar um software de uma região é especificado através do Internet Explorer. Essas zonas são Internet, intranet Local, sites restritos, sites confiáveis e meu computador. Uma regra de zona da Internet é projetada para impedir que os usuários baixem e instalem software.

#### <a name="to-create-an-internet-zone-rule"></a>Para criar uma regra de zona da Internet

1.  Abra as políticas de restrição de Software.

2.  Na árvore de console ou no painel de detalhes, clique com botão direito **regras adicionais**e clique em **nova regra de zona da Internet**.

3.  Em **zona da Internet**, clique em uma zona da Internet.

4.  Em **nível de segurança**, clique em **não permitido** ou **Unrestricted**e clique em **Okey**.

> [!NOTE]
> -   Ele pode ser necessário criar uma nova configuração de política de restrição de software para o objeto de política de grupo (GPO) se você ainda não tiver feito isso.
> -   Regras de zona só se aplicam a arquivos com um tipo de arquivo. msi, que são os pacotes do Windows Installer.
> -   Para políticas de restrição de software entrem em vigor, os usuários devem atualizar configurações de política de logoff e logon em seus computadores.
> -   Quando mais de uma regra de políticas de restrição de software é aplicada a configurações de política, há uma precedência de regras para lidar com conflitos.

## <a name="BKMK_Path_Rules"></a>Trabalhando com regras de caminho
Uma regra de caminho identifica o software por seu caminho de arquivo. Por exemplo, se você tiver um computador que tenha um nível de segurança padrão **não permitido**, você ainda pode conceder acesso irrestrito a uma pasta específica para cada usuário. Você pode criar uma regra de caminho usando o caminho do arquivo e definir o nível de segurança da regra de caminho para **Unrestricted**. Alguns caminhos comuns para esse tipo de regra são % userprofile %, % windir %, % appdata %, % programfiles % e % temp %. Você também pode criar regras de caminho que usam a chave do registro do software como o caminho do registro.

Como essas regras são especificadas pelo caminho, se um programa de software for movido, a regra de caminho não se aplica mais.

#### <a name="to-create-a-path-rule"></a>Para criar uma regra de caminho

1.  Abra as políticas de restrição de Software.

2.  Na árvore de console ou no painel de detalhes, clique com botão direito **regras adicionais**e clique em **nova regra de caminho**.

3.  Em **caminho**, digite um caminho ou clique em **procurar** para encontrar um arquivo ou pasta.

4.  Em **nível de segurança**, clique em **não permitido** ou **Unrestricted**.

5.  Em **descrição**, digite uma descrição para essa regra e clique em **Okey**.

> [!CAUTION]
> -   Em determinadas pastas, como a pasta Windows, a configuração segurança de nível para **não permitido** pode afetar negativamente a operação do seu sistema operacional. Verifique se você não permitir um componente essencial do sistema operacional ou um dos seus programas dependentes.

> [!NOTE]
> -   Ele pode ser necessário criar novas políticas de restrição de software para o objeto de política de grupo (GPO) se você ainda não tiver feito isso.
> -   Se você criar uma regra de caminho de software com uma segurança de nível de **não permitido**, os usuários ainda podem executar o software, copiando-os para outro local.
> -   Os caracteres curinga que são compatíveis com a regra de caminho são * e?.
> -   Você pode usar variáveis de ambiente, como % programfiles % ou % systemroot %, na regra de caminho.
> -   Se você quiser criar uma regra de caminho para o software quando você não sabe onde ele está armazenado em um computador, mas você tem sua chave do registro, você pode criar uma regra de caminho do registro.
> -   Para evitar que os usuários executem anexos de email, você pode criar uma regra de caminho para o diretório de anexo do seu programa de email que impede que os usuários executem anexos de email.
> -   Os únicos tipos de arquivo que são afetados pelas regras de caminho são aqueles que estão listados em **tipos de arquivo designados** no painel de detalhes para políticas de restrição de Software. Há uma lista dos tipos de arquivo designados que é compartilhada por todas as regras.
> -   Para políticas de restrição de software entrem em vigor, os usuários devem atualizar configurações de política de logoff e logon em seus computadores.
> -   Quando mais de uma regra de políticas de restrição de software é aplicada a configurações de política, há uma precedência de regras para lidar com conflitos.

#### <a name="to-create-a-registry-path-rule"></a>Para criar uma regra de caminho do registro

1.  Sobre o **iniciar** tela, digite regedit.

2.  Na árvore de console, clique com botão direito a chave do registro que você deseja criar uma regra para e clique em **Copiar nome da chave**. Observe o nome do valor no painel de detalhes.

3.  Abra as políticas de restrição de Software.

4.  Na árvore de console ou no painel de detalhes, clique com botão direito **regras adicionais**e clique em **nova regra de caminho**.

5.  Em **caminho**, cole o nome da chave do registro, seguido pelo nome do valor.

6.  Coloque o caminho do registro sinais de porcentagem (%)), por exemplo, % HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PlatformSDK\Directories\InstallDir%.

7.  Em **nível de segurança**, clique em **não permitido** ou **Unrestricted**.

8.  Em **descrição**, digite uma descrição para essa regra e clique em **Okey**.


