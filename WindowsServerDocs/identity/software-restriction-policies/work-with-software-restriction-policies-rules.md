---
title: Trabalhar com regras de políticas de restrição de software
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: bb5e56fe541a06b1100de2f25fc10f4db46b8d24
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322948"
---
# <a name="work-with-software-restriction-policies-rules"></a>Trabalhar com regras de políticas de restrição de software

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve os procedimentos que funcionam com regras de certificado, caminho, zona da Internet e hash usando diretivas de restrição de software.

## <a name="introduction"></a>Introdução
Com as diretivas de restrição de software, você pode proteger seu ambiente de computação contra software não confiável, identificando e especificando qual software pode ser executado. Você pode definir um nível de segurança padrão de **irrestrito** ou não **permitido** para um objeto de política de grupo (GPO) para que o software seja permitido ou não tenha permissão para ser executado por padrão. Você pode fazer exceções a esse nível de segurança padrão criando regras de diretivas de restrição de software para software específico. Por exemplo, se o nível de segurança padrão for definido como **Não Permitido**, você poderá criar regras que permitam que software específico seja executado. Os tipos de regras são os seguintes:

-   **Regras de certificado**

    Para procedimentos, consulte o artigo sobre [trabalho com regras de certificado](#BKMK_Cert_Rules).

-   **Regras de hash**

    Para procedimentos, consulte o artigo sobre [trabalho com regras de hash](#BKMK_Hash_Rules).

-   **Regras de zona da Internet**

    Para procedimentos, consulte [trabalhando com regras de zona da Internet](#BKMK_Internet_Zone_Rules).

-   **Regras de caminho**

    Para procedimentos, consulte o artigo sobre [trabalho com regras de caminho](#BKMK_Path_Rules).

Para obter informações sobre outras tarefas para gerenciar diretivas de restrição de software, consulte [administrar políticas de restrição de software](administer-software-restriction-policies.md).

## <a name="BKMK_Cert_Rules"></a>Trabalhando com regras de certificado
As diretivas de restrição de software também podem identificar o software por seu certificado de autenticação. Você pode criar uma regra de certificado que identifica software e que permite ou não que ele seja executado, dependendo do nível de segurança. Por exemplo, você pode usar regras de certificado para confiar automaticamente em software de uma fonte confiável em um domínio sem solicitar informações do usuário. Também pode usar regras de certificado para executar arquivos em áreas não permitidas do sistema operacional. As regras de certificado não são habilitadas por padrão.

Quando as regras são criadas para o domínio usando Política de Grupo, você deve ter permissões para criar ou modificar um objeto de Política de Grupo. Se estiver criando regras para o computador local, você deverá ter credenciais administrativas nesse computador.

#### <a name="to-create-a-certificate-rule"></a>Para criar uma regra de certificado

1.  Abra Políticas de Restrição de Software.

2.  Na árvore de console ou no painel de detalhes, clique com o botão direito do mouse em **regras adicionais**e clique em **nova regra de certificado**.

3.  Clique em **Procurar** e selecione um certificado ou arquivo assinado.

4.  Em **nível de segurança**, clique em não **permitido** ou **irrestrito**.

5.  Em **Descrição**, digite uma descrição para essa regra e clique em **OK**.

> [!NOTE]
> -   Talvez seja necessário criar uma nova configuração de política de restrição de software para o GPO (Objeto de Política de Grupo), se você ainda não tiver feito isso.
> -   As regras de certificado não são habilitadas por padrão.
> -   Os únicos tipos de arquivos afetados pelas regras de certificado são aqueles listados em **Tipos de arquivo designados** no painel de detalhes para Políticas de Restrição de Software. Há uma lista de tipos de arquivo designados que é compartilhada por todas as regras.
> -   Para que as diretivas de restrição de software entrem em vigor, os usuários devem atualizar as configurações de política, fazendo logoff e fazendo logon em seus computadores.
> -   Quando mais de uma regra de diretivas de restrição de software é aplicada às configurações de política, há uma precedência de regras para lidar com conflitos.

### <a name="enabling-certificate-rules"></a>Habilitando as regras de certificado
Há diferentes procedimentos para habilitar as regras de certificado dependendo do seu ambiente:

-   [Para seu computador local](#BKMK_1)

-   [Para um objeto Política de Grupo, e você está em um servidor que ingressou em um domínio](#BKMK_2)

-   [Para um objeto Política de Grupo, e você está em um controlador de domínio ou em uma estação de trabalho que tenha o Ferramentas de Administração de Servidor Remoto instalado](#BKMK_3)

-   [Somente para controladores de domínio, e você está em um controlador de domínio ou em uma estação de trabalho que tem o pacote de Ferramentas de Administração de Servidor Remoto instalado](#BKMK_4)

#### <a name="BKMK_1"></a>Para habilitar regras de certificado para seu computador local

1.  Abra as Configurações de Segurança Locais.

2.  Na árvore de console, clique em **Opções de segurança** localizadas em configurações de segurança/políticas locais.

3.  No painel de detalhes, clique duas vezes em **Configurações do sistema: usar regras de certificado em arquivos executáveis do Windows para políticas de restrição de software**.

4.  Siga um destes procedimentos e clique em **OK**:

    -   Para habilitar as regras de certificado, clique em **Habilitado**.

    -   Para desabilitar as regras de certificado, clique em **Desabilitado**.

#### <a name="BKMK_2"></a>Para habilitar regras de certificado para um objeto Política de Grupo, e você está em um servidor que ingressou em um domínio

1.  Abra o MMC (Console de Gerenciamento Microsoft).

2.  No menu **Arquivo**, clique em **Adicionar ou Remover snap-in** e clique em **Adicionar**.

3.  Clique em **Editor de Objeto de Política de Grupo Local** e clique em **Adicionar**.

4.  Em **Selecionar Objeto de Política de Grupo**, clique em **Procurar**.

5.  Em **procurar um objeto de política de grupo**, selecione um objeto de política de grupo (GPO) no domínio, site ou unidade organizacional apropriado-ou crie um novo e, em seguida, clique em **concluir**.

6.  Clique em **Fechar**e clique em **OK**.

7.  Na árvore de console, clique em **Opções de segurança** localizadas em *GroupPolicyObject* [*ComputerName*] política/configuração do computador/configurações do Windows/configurações de segurança/políticas locais/.

8.  No painel de detalhes, clique duas vezes em **Configurações do sistema: usar regras de certificado em arquivos executáveis do Windows para políticas de restrição de software**.

9. Se essa configuração de política não tiver sido definida ainda, marque a caixa de seleção **Definir estas configurações de políticas**.

10. Siga um destes procedimentos e clique em **OK**:

    -   Para habilitar as regras de certificado, clique em **Habilitado**.

    -   Para desabilitar as regras de certificado, clique em **Desabilitado**.

#### <a name="BKMK_3"></a>Para habilitar regras de certificado para um objeto Política de Grupo e você está em um controlador de domínio ou em uma estação de trabalho que tenha o Ferramentas de Administração de Servidor Remoto instalado

1.  Abra Usuários e Computadores do Active Directory.

2.  Na árvore de console, clique com o botão direito do mouse no GPO (Objeto de Política de Grupo) para o qual você quer habilitar as regras de certificado.

3.  Clique em **Propriedades** e clique na guia **Política de Grupo**.

4.  Clique em **Editar** para abrir o GPO que quer editar. Você também pode clicar em **Novo** para criar um novo GPO e clique em **Editar**.

5.  Na árvore de console, clique em **Opções de segurança** localizadas em *GroupPolicyObject*[*ComputerName*] política/configuração do computador/configurações do Windows/configurações de segurança/políticas locais.

6.  No painel de detalhes, clique duas vezes em **Configurações do sistema: usar regras de certificado em arquivos executáveis do Windows para políticas de restrição de software**.

7.  Se essa configuração de política não tiver sido definida ainda, marque a caixa de seleção **Definir estas configurações de políticas**.

8.  Siga um destes procedimentos e clique em **OK**:

    -   Para habilitar as regras de certificado, clique em **Habilitado**.

    -   Para desabilitar as regras de certificado, clique em **Desabilitado**.

#### <a name="BKMK_4"></a>Para habilitar regras de certificado somente para controladores de domínio, e você está em um controlador de domínio ou em uma estação de trabalho com o Ferramentas de Administração de Servidor Remoto instalado

1.  Abra as Configurações de Segurança do Controlador de Domínio.

2.  Na árvore de console, clique em **Opções de Segurança**, localizado em *GroupPolicyObject* [*Nome_do_Computador*] Política/Configuração do computador/Configurações do Windows/Configurações de segurança/Políticas locais.

3.  No painel de detalhes, clique duas vezes em **Configurações do sistema: usar regras de certificado em arquivos executáveis do Windows para políticas de restrição de software**.

4.  Se essa configuração de política não tiver sido definida ainda, marque a caixa de seleção **Definir estas configurações de políticas**.

5.  Siga um destes procedimentos e clique em **OK**:

    -   Para habilitar as regras de certificado, clique em **Habilitado**.

    -   Para desabilitar as regras de certificado, clique em **Desabilitado**.

> [!NOTE]
> Você deve executar esse procedimento antes que as regras de certificado possam surtir efeito.

### <a name="set-trusted-publisher-options"></a>Definir opções de Publicador confiáveis
A assinatura de software está sendo usada por um crescente número de fornecedores de software e desenvolvedores de aplicativo para verificar se os aplicativos vêm de uma origem confiável. Porém, muitos usuários não entendem ou prestam pouca atenção aos certificados de autenticação associados a aplicativos que instalam.

As configurações de políticas na guia **Fornecedores Confiáveis** da política de validação de caminho do certificado permitem que administradores controlem os certificados que podem ser aceitos por virem de um fornecedor confiável.

##### <a name="to-configure-the-trusted-publishers-policy-settings-for-a-local-computer"></a>Para configurar as definições de política de fornecedores confiáveis para um computador local

1.  Na tela **Iniciar** , digite**gpedit. msc** e pressione Enter.

2.  Na árvore de console em **Política do Computador Local\Configuração do Computador\Configurações do Windows\Configurações de Segurança**, clique em **Políticas de Chave Pública**.

3.  Clique duas vezes em **Configurações de Validação de Caminho do Certificado** e clique na guia **Fornecedores Confiáveis**.

4.  Marque a caixa de seleção **Definir estas configurações de política**, selecione as configurações de política que deseja aplicar e clique em **OK** para aplicar as novas configurações.

##### <a name="to-configure-the-trusted-publishers-policy-settings-for-a-domain"></a>Para definir as configurações da política de fornecedores confiáveis para um domínio

1.  Abra **Gerenciamento de Política de Grupo**.

2.  Na árvore de console, clique duas vezes em **política de grupo objetos** na floresta e no domínio que contém o GPO (objeto de política de grupo) **padrão de diretiva de domínio** que você deseja editar.

3.  Clique com o botão direito do mouse no GPO **Política de Domínio Padrão** e clique em **Editar**.

4.  Na árvore de console em **Configuração do Computador\Configurações do Windows\Configurações de Segurança**, clique em **Políticas de Chave Pública**.

5.  Clique duas vezes em **Configurações de Validação de Caminho do Certificado** e clique na guia **Fornecedores Confiáveis**.

6.  Marque a caixa de seleção **Definir estas configurações de política**, selecione as configurações de política que deseja aplicar e clique em **OK** para aplicar as novas configurações.

##### <a name="to-allow-only-administrators-to-manage-certificates-used-for-code-signing-for-a-local-computer"></a>Para permitir que apenas administradores gerenciem certificados usados na assinatura de códigos de um computador local

1.  Na tela **Iniciar** , digite, **gpedit. msc** , nos **programas e arquivos de pesquisa** ou no Windows 8, na área de trabalho e pressione Enter.

2.  Na árvore de console, em política de **domínio padrão** ou **diretiva de computador local**, clique duas vezes em **configuração do computador**, **configurações do Windows**e configurações de **segurança**e clique em **políticas de chave pública**.

3.  Clique duas vezes em **Configurações de Validação de Caminho do Certificado** e clique na guia **Fornecedores Confiáveis**.

4.  Marque a caixa de seleção **Definir estas configurações de políticas**.

5.  Em **Gerenciamento de fornecedores confiáveis**, clique em **Permitir que somente todos os administradores gerenciem os Fornecedores Confiáveis** e clique em **OK** para aplicar as novas configurações.

##### <a name="to-allow-only-administrators-to-manage-certificates-used-for-code-signing-for-a-domain"></a>Para permitir que somente administradores gerenciem certificados usados na assinatura de códigos de um domínio

1.  Abra **Gerenciamento de Política de Grupo**.

2.  Na árvore de console, clique duas vezes em **política de grupo objetos** na floresta e no domínio que contém o GPO de **política de domínio padrão** que você deseja editar.

3.  Clique com o botão direito do mouse no GPO **Política de Domínio Padrão** e clique em **Editar**.

4.  Na árvore de console em **Configuração do Computador\Configurações do Windows\Configurações de Segurança**, clique em **Políticas de Chave Pública**.

5.  Clique duas vezes em **Configurações de Validação de Caminho do Certificado** e clique na guia **Fornecedores Confiáveis**.

6.  Marque a caixa de seleção **Definir estas configurações de política**, implemente as alterações desejadas e clique em **OK** para aplicar as novas configurações.

## <a name="BKMK_Hash_Rules"></a>Trabalhando com regras de hash
Um hash é uma série de bytes com comprimento fixo que identifica exclusivamente um programa de software ou arquivo. O hash é calculado por um algoritmo de hash. Quando uma regra de hash é criada para um programa de software, as políticas de restrição de software calculam um hash do programa. Quando um usuário tenta abrir um programa de software, um hash do programa é comparado com as regras de hash existentes para as políticas de restrição de software. O hash de um programa de software sempre é o mesmo, independentemente de onde o programa está localizado no computador. Entretanto, se um programa de software for alterado de alguma maneira, seu hash também será alterado e ele não corresponderá mais ao hash na regra de hash para políticas de restrição de software.

Por exemplo, é possível criar uma regra de hash e definir o nível de segurança como **Não Permitido** para impedir que os usuários executem um determinado arquivo. Um arquivo pode ser renomeado ou movido para outra pasta e ainda resultar no mesmo hash. Entretanto, quaisquer alterações no arquivo em si também alteram seu valor hash e permitem que o arquivo ignore as restrições.

#### <a name="to-create-a-hash-rule"></a>Para criar uma regra de hash

1.  Abra Políticas de Restrição de Software.

2.  Na árvore de console ou no painel de detalhes, clique com o botão direito do mouse em **regras adicionais**e clique em **nova regra de hash**.

3.  Clique em **procurar** para localizar um arquivo.

    > [!NOTE]
    > No Windows XP é possível colar um hash previamente calculado no **hash do arquivo**. No Windows Server 2008 R2, no Windows 7 e em versões posteriores, essa opção não está disponível.

4.  Em **nível de segurança**, clique em não **permitido** ou **irrestrito**.

5.  Em **Descrição**, digite uma descrição para essa regra e clique em **OK**.

> [!NOTE]
> -   Talvez seja necessário criar uma nova configuração de política de restrição de software para o GPO (Objeto de Política de Grupo), se você ainda não tiver feito isso.
> -   Uma regra de hash pode ser criada para impedir que um vírus ou um cavalo de Troia seja executado.
> -   Se você quiser que outras pessoas usem uma regra de hash para que um vírus não possa ser executado, calcule o hash do vírus usando as políticas de restrição de software e envie por email o valor de hash para as outras pessoas. Nunca envie um email para o vírus em si.
> -   Se um vírus tiver sido enviado por email, você também poderá criar uma regra de caminho para evitar a execução de anexos de email.
> -   Um arquivo que é renomeado ou movido para outra pasta resulta no mesmo hash. Qualquer alteração no próprio arquivo resulta em um hash diferente.
> -   Os únicos tipos de arquivos afetados pelas regras de hash são aqueles listados em **Tipos de Arquivo Designados** no painel de detalhes para Políticas de Restrição de Software. Há uma lista de tipos de arquivo designados que é compartilhada por todas as regras.
> -   Para que as diretivas de restrição de software entrem em vigor, os usuários devem atualizar as configurações de política, fazendo logoff e fazendo logon em seus computadores.
> -   Quando mais de uma regra de diretivas de restrição de software é aplicada às configurações de política, há uma precedência de regras para lidar com conflitos.

## <a name="BKMK_Internet_Zone_Rules"></a>Trabalhando com regras de zona da Internet
As regras de zona da Internet se aplicam somente aos pacotes do Windows Installer. Uma zona da Internet pode identificar software de uma zona especificada pelo Internet Explorer. Essas zonas são Internet, Intranet local, Sites restritos, Sites confiáveis e Meu Computador. Uma regra de zona da Internet foi criada para impedir que os usuários baixem e instalem software.

#### <a name="to-create-an-internet-zone-rule"></a>Para criar uma regra de zona da Internet

1.  Abra Políticas de Restrição de Software.

2.  Na árvore de console ou no painel de detalhes, clique com o botão direito do mouse em **regras adicionais**e clique em **nova regra de zona da Internet**.

3.  Em **Zona da Internet**, clique em uma zona da Internet.

4.  Em **nível de segurança**, clique em não **permitido** ou **irrestrito**e, em seguida, clique em **OK**.

> [!NOTE]
> -   Talvez seja necessário criar uma nova configuração de política de restrição de software para o GPO (Objeto de Política de Grupo), se você ainda não tiver feito isso.
> -   As regras de zona se aplicam somente a arquivos com um tipo de arquivo .msi, que são pacotes do Windows Installer.
> -   Para que as diretivas de restrição de software entrem em vigor, os usuários devem atualizar as configurações de política, fazendo logoff e fazendo logon em seus computadores.
> -   Quando mais de uma regra de diretivas de restrição de software é aplicada às configurações de política, há uma precedência de regras para lidar com conflitos.

## <a name="BKMK_Path_Rules"></a>Trabalhando com regras de caminho
Uma regra de caminho identifica o software pelo respectivo caminho de arquivo. Por exemplo, se você tiver um computador com nível de segurança padrão de **Não Permitido**, ainda assim poderá conceder acesso irrestrito a uma pasta específica para cada usuário. Você pode criar uma regra de caminho usando o caminho de arquivo e definindo o nível de segurança como **Irrestrito**. Alguns caminhos comuns para esse tipo de regra são %userprofile%, %windir%, %appdata%, %programfiles% e %temp%. Você também pode criar regras de caminho do Registro que usam a chave do Registro do software como caminho.

Como essas regras são especificadas pelo caminho, se um programa de software for movido, a regra de caminho não será mais aplicada.

#### <a name="to-create-a-path-rule"></a>Para criar uma regra de caminho

1.  Abra Políticas de Restrição de Software.

2.  Na árvore de console ou no painel de detalhes, clique com o botão direito do mouse em **regras adicionais**e clique em **nova regra de caminho**.

3.  Em **Caminho**, digite um caminho, ou clique em **Procurar** para encontrar um arquivo ou pasta.

4.  Em **nível de segurança**, clique em não **permitido** ou **irrestrito**.

5.  Em **Descrição**, digite uma descrição para essa regra e clique em **OK**.

> [!CAUTION]
> -   Em determinadas pastas, como a pasta do Windows, a definição do nível de segurança como não **permitido** pode afetar negativamente a operação do seu sistema operacional. Verifique se você permitiu um componente crucial do sistema operacional ou um de seus programas dependentes.

> [!NOTE]
> -   Talvez seja necessário criar novas políticas de restrição de software para o GPO (Objeto de Política de Grupo), se você ainda não tiver feito isso.
> -   Se você criar uma regra de caminho para software com um nível de segurança **Não Permitido**, os usuários ainda poderão executar o software copiando-o para outro local.
> -   Os caracteres curinga com suporte na regra de caminho são * e ?.
> -   Você pode usar variáveis de ambiente, como %programfiles% ou %systemroot%, na regra de caminho.
> -   Se você quiser criar uma regra de caminho para software quando não souber onde ela está armazenada em um computador, mas tiver a respectiva chave do Registro, será possível criar uma regra de caminho do Registro.
> -   Para impedir que os usuários executem anexos de email, você pode criar uma regra de caminho para o diretório de anexos do seu programa de email que impede que os usuários executem anexos de email.
> -   Os únicos tipos de arquivos afetados pelas regras de caminho são aqueles listados em **Tipos de Arquivo Designados** no painel de detalhes para Políticas de Restrição de Software. Há uma lista de tipos de arquivo designados que é compartilhada por todas as regras.
> -   Para que as diretivas de restrição de software entrem em vigor, os usuários devem atualizar as configurações de política, fazendo logoff e fazendo logon em seus computadores.
> -   Quando mais de uma regra de diretivas de restrição de software é aplicada às configurações de política, há uma precedência de regras para lidar com conflitos.

#### <a name="to-create-a-registry-path-rule"></a>Para criar uma regra de caminho do Registro

1.  Na tela **Iniciar** , digite regedit.

2.  Na árvore de console, clique com o botão direito do mouse na chave do Registro para a qual deseja criar uma regra e clique em **Copiar Nome da Chave**. Observe o nome do valor no painel de detalhes.

3.  Abra Políticas de Restrição de Software.

4.  Na árvore de console ou no painel de detalhes, clique com o botão direito do mouse em **regras adicionais**e clique em **nova regra de caminho**.

5.  Em **caminho**, Cole o nome da chave do registro, seguido pelo nome do valor.

6.  Coloque o caminho do registro em sinais de porcentagem (%), por exemplo,% HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\PlatformSDK\Directories\InstallDir%.

7.  Em **nível de segurança**, clique em não **permitido** ou **irrestrito**.

8.  Em **Descrição**, digite uma descrição para essa regra e clique em **OK**.


