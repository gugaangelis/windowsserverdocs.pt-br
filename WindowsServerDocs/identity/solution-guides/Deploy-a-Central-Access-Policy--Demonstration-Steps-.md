---
ms.assetid: 8738c03d-6ae8-49a7-8b0c-bef7eab81057
title: Implantar uma política de acesso central (passo a passo)
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f83e828dd8ce90da4265eb03f94b498933d9c2a6
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87518604"
---
# <a name="deploy-a-central-access-policy-demonstration-steps"></a>Implantar uma política de acesso central (passo a passo)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Neste cenário, as operações de segurança do departamento financeiro estão trabalhando em conjunto com a segurança das informações para especificar a necessidade de uma política de acesso central que permita proteger as informações financeiras arquivadas que são armazenadas nos servidores de arquivos. As informações financeiras arquivadas de cada país podem ser acessadas em modo somente leitura pelos funcionários da área financeira do mesmo país. Um grupo de administração financeira central pode acessar as informações financeiras de todos os países.

A implantação de uma política de acesso central abrange as seguintes fases:

| Fase | Descrição |
|--|--|
| [Plano: identificar a necessidade de política e a configuração necessária para a implantação](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.2) | Identificar a necessidade de uma política e a configuração necessária para a implantação. |
| [Implementar: configurar os componentes e a política](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.3) | Configurar os componentes e a política. |
| [Implantar a política de acesso central](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.4) | Implantar a política. |
| [Manter: alterar e preparar a política](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.5) | Alterações de política e preparo. |

## <a name="set-up-a-test-environment"></a><a name="BKMK_1.1"></a>Configurar um ambiente de teste
Antes de começar, você precisa configurar um laboratório para testar esse cenário. As etapas para configurar o laboratório são explicadas em detalhes no [Apêndice B: Configurando o ambiente de teste](Appendix-B--Setting-Up-the-Test-Environment.md).

## <a name="plan-identify-the-need-for-policy-and-the-configuration-required-for-deployment"></a><a name="BKMK_1.2"></a>Plano: identificar a necessidade de política e a configuração necessária para a implantação
Esta seção apresenta a série de etapas de alto nível que auxiliam na fase de planejamento da sua implantação.

|Etapa nº | Etapa | Exemplo |
|--|--|--|
| 1,1 | A empresa determina que uma política de acesso central é necessária. | Para proteger as informações financeiras armazenadas nos servidores de arquivos, as operações de segurança do departamento financeiro trabalham com a segurança das informações para especificar a necessidade de uma política de acesso central. |  |
| 1.2 | Expressar a política de acesso | Os documentos financeiros só devem ser lidos pelos membros do departamento financeiro. Os membros do departamento financeiro só devem acessar documentos do próprio país. Apenas os administradores financeiros devem ter acesso de gravação. Uma exceção será permitida para os membros do grupo FinanceException. Esse grupo terá acesso de leitura. |  |
| 1.3 | Expressar a política de acesso em construções do Windows Server 2012 | Direcionamento:<p>-Resource. Department contém Finance<p>Regras de acesso:<p>-Permitir leitura de usuário. país = recurso. país e usuário. departamento = recurso. departamento<br />-Permitir controle total de usuário. MemberOf (FinanceAdmin)<p>Exceção:<p>Allow read memberOf(FinanceException) |  |
| 1.4 | Determinar as propriedades de arquivo necessárias para a política | Marque os arquivos com:<p>-Departamento<br />-País |  |
| 1.5 | Determinar os tipos de declarações e os grupos necessários para a política | Tipos de declarações:<p>-País<br />-Departamento<p>Grupos de usuários:<p>- FinanceAdmin<br />-Financeexception |  |
| 1.6 | Determinar os servidores aos quais essa política será aplicada | Aplique a política a todos os servidores de arquivos financeiros. |  |

## <a name="implement-configure-the-components-and-policy"></a><a name="BKMK_1.3"></a>Implementar: configurar os componentes e a política
Esta seção apresenta um exemplo de implantação de uma política de acesso central para documentos financeiros.

| Etapa nº | Etapa | Exemplo |
|--|--|--|
| 2.1 | Criar tipos de declarações | Crie os seguintes tipos de declarações:<p>-Departamento<br />-País |
| 2.2 | Criar propriedades de recurso | Crie e habilite as seguintes propriedades de recurso:<p>-Departamento<br />-País |
| 2.3 | Configurar uma regra de acesso central | Crie a regra Documentos Financeiros incluindo a política determinada na seção anterior. |
| 2.4 | Configurar uma CAP (política de acesso central) | Crie uma CAP chamada Política Financeira e adicione a ela a regra Documentos Financeiros. |
| 2.5 | Direcionar a política de acesso central aos servidores de arquivos | Publique a CAP Política Financeira nos servidores de arquivos. |
| 2.6 | Habilitar o Suporte KDC (centro de distribuição de chaves) para declarações, autenticação composta e proteção Kerberos. | Habilite o Suporte KDC para declarações, autenticação composta e proteção Kerberos para contoso.com. |

No procedimento a seguir, você cria dois tipos de declaração: país e departamento.

#### <a name="to-create-claim-types"></a>Para criar tipos de declarações

1. Abra o servidor DC1 no Gerenciador do Hyper-V e faça logon como CONTOSO\Administrator, com a senha <strong>pass@word1</strong> .

2. Abra o Centro Administrativo do Active Directory.

3. Clique no **ícone Modo de Exibição de Árvore**, expanda **Controle de Acesso Dinâmico** e selecione **Tipos de Declarações**.

   Clique com o botão direito do mouse em **Tipos de Declarações**, clique em **Novo** e, depois, clique em **Tipo de Declaração**.

   > [!TIP]
   > Você também pode abrir a janela **Criar Tipo de Declaração:** a partir do painel **Tarefas**. No painel **Tarefas**, clique em **Novo** e, então, em **Tipo de Declaração**.

4. Na lista **Atributo de Origem**, role a lista de atributos para baixo e clique em **departamento**. Isso deve preencher o campo **Nome de exibição** com o valor **departamento**. Clique em **OK**.

5. No painel **Tarefas**, clique em **Novo** e, depois, em **Tipo de Declaração**.

6. Na lista **Atributo de Origem**, role a lista de atributos para baixo e clique no atributo **c** (Country-Name). No campo **Nome de exibição**, digite **país**.

7. Na seção **Valores Sugeridos**, selecione **Os seguintes valores são sugeridos:** e clique em **Adicionar**.

8. Nos campos **Valor** e **Nome de exibição**, digite **US** e clique em **OK**.

9. Repita a etapa acima. Na caixa de diálogo **Adicionar um valor sugerido**, digite**JP** nos campos **Valor** e **Nome de exibição** e clique em **OK**.

![guias de solução](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

```powershell
New-ADClaimType country -SourceAttribute c -SuggestedValues:@((New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("US","US","")), (New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("JP","JP","")))
New-ADClaimType department -SourceAttribute department
```

> [!TIP]
> Você pode usar o Visualizador do Windows PowerShell History no Centro Administrativo do Active Directory para pesquisar os cmdlets do Windows PowerShell para cada procedimento realizado no Centro Administrativo do Active Directory. Para obter mais informações, consulte [Visualizador de Histórico do Windows PowerShell:](../ad-ds/get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-.md)

A próxima etapa consiste em criar as propriedades de recurso. No procedimento a seguir, você criará uma propriedade de recurso que será adicionada automaticamente à lista Propriedades de Recursos Globais no controlador de domínio para que seja disponibilizada ao servidor de arquivos.

#### <a name="to-create-and-enable-pre-created-resource-properties"></a>Para criar e habilitar propriedades de recurso criadas previamente

1.  No painel esquerdo do Centro Administrativo do Active Directory, clique em **Modo de Exibição de Árvore**. Expanda **Controle de Acesso Dinâmico** e selecione **Propriedades de Recurso**.

2.  Clique com o botão direito do mouse em **Propriedades de Recurso**, clique em **Novo** e, então, em **Propriedade de Recurso de Referência**.

    > [!TIP]
    > Você também pode escolher uma propriedade de recurso no painel **Tarefas**. Clique em **Novo** e, depois, em **Propriedade de Recurso de Referência**.

3.  Em **Selecionar um tipo de declaração para compartilhar sua lista de valores sugeridos**, clique em **país**.

4.  No campo **Nome de exibição**, digite **país** e clique em **OK**.

5.  Clique duas vezes na lista **Propriedades de Recurso** e role a lista para baixo até a propriedade de recurso **Departamento**. Clique com o botão direito do mouse e clique em **Habilitar**. Isso habilitará a propriedade de recurso interna **Departamento**.

6.  Na lista **Propriedades de Recurso** do painel de navegação do Centro Administrativo do Active Directory, você terá agora duas propriedades de recurso habilitadas:

    -   País

    -   Departamento

![guias de solução](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

```powershell
New-ADResourceProperty Country -IsSecured $true -ResourcePropertyValueType MS-DS-MultivaluedChoice -SharesValuesWith country
Set-ADResourceProperty Department_MS -Enabled $true
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Country
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Department_MS
```

A próxima etapa consiste em criar regras de acesso que definam quem pode acessar os recursos. Neste cenário, as regras de negócio são:

-   Os documentos financeiros podem ser lidos apenas por membros do departamento financeiro.

-   Os membros do departamento financeiro podem acessar apenas os documentos do próprio país.

-   Apenas os administradores financeiros podem ter acesso de gravação.

-   Permitiremos uma exceção aos membros do grupo FinanceException. Esse grupo terá acesso de leitura.

-   O administrador e o proprietário do documento ainda terão acesso total.

Ou para expressar as regras com construções do Windows Server 2012:

Direcionamento: Resource. Department contém Finance

Regras de acesso:

-   Allow Read User.Country=Resource.Country AND User.department = Resource.Department

-   Allow Full control User.MemberOf(FinanceAdmin)

-   Allow Read User.MemberOf(FinanceException)

#### <a name="to-create-a-central-access-rule"></a>Para criar uma regra de acesso central

1. No painel esquerdo do Centro Administrativo do Active Directory, clique em **Modo de Exibição de Árvore**, selecione **Controle de Acesso Dinâmico** e clique em **Regras de Acesso Central**.

2. Clique com o botão direito do mouse em **Regras de Acesso Central**, clique em **Novo** e, então, em **Regra de Acesso Central**.

3. No campo **Nome**, digite **Regra de Documentos Financeiros**.

4. Na seção **Recursos de Destino**, clique em **Editar** e, na caixa de diálogo **Regra de Acesso Central**, clique em **Adicionar uma condição**. Adicione a seguinte condição: [**recurso**] [**Departamento**] [**Equals**] [**valor**] [**Finance**] e clique em **OK**.

5. Na seção **Permissões**, selecione **Usar as seguintes permissões como atuais**, clique em **Editar** e, na caixa de diálogo **Configurações de Segurança Avançadas para Permissões**, clique em **Adicionar**.

   > [!NOTE]
   > A opção **Usar as seguintes permissões como propostas** permite criar a política em preparo. Para obter mais informações sobre como fazer isso, consulte a seção manter: alterar e preparar a política neste tópico.

6. Na caixa de diálogo **Entrada de Permissão para Permissões**, clique em **Selecionar uma entidade de segurança**, digite **Usuários Autenticados** e clique em **OK**.

7. Na caixa de diálogo **entrada de permissão para permissões** , clique em **Adicionar uma condição**e adicione as seguintes condições: [**usuário**]**[país**] [**qualquer de**] [**recurso**] [**país**] clique em **Adicionar uma condição**.
    [**E**] clique em [**Usuário**] [**Departamento**] [**Qualquer**] [**Recurso**] [**Departamento**]. Defina as **Permissões** como **Leitura**.

8. Clique em **OK** e, depois, em **Adicionar**. Clique em **Selecionar uma entidade de segurança**, digite **FinanceAdmin** e clique em **OK**.

9. Selecione as permissões **Modificar, Ler e Executar, Ler, Gravar** e clique em **OK**.

10. Clique em **Adicionar**, clique em **Selecionar uma entidade de segurança**, digite **FinanceException** e, em seguida, clique em **OK**. Selecione as permissões futuras **Ler** e **Ler e Executar**.

11. Clique em **OK** três vezes para concluir e voltar ao Centro Administrativo do Active Directory.

![guias de solução](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

```powershell
$countryClaimType = Get-ADClaimType country
$departmentClaimType = Get-ADClaimType department
$countryResourceProperty = Get-ADResourceProperty Country
$departmentResourceProperty = Get-ADResourceProperty Department
$currentAcl = "O:SYG:SYD:AR(A;;FA;;;OW)(A;;FA;;;BA)(A;;0x1200a9;;;S-1-5-21-1787166779-1215870801-2157059049-1113)(A;;0x1301bf;;;S-1-5-21-1787166779-1215870801-2157059049-1112)(A;;FA;;;SY)(XA;;0x1200a9;;;AU;((@USER." + $countryClaimType.Name + " Any_of @RESOURCE." + $countryResourceProperty.Name + ") && (@USER." + $departmentClaimType.Name + " Any_of @RESOURCE." + $departmentResourceProperty.Name + ")))"
$resourceCondition = "(@RESOURCE." + $departmentResourceProperty.Name + " Contains {`"Finance`"})"
New-ADCentralAccessRule "Finance Documents Rule" -CurrentAcl $currentAcl -ResourceCondition $resourceCondition
```

> [!IMPORTANT]
> No cmdlet de exemplo acima, os SIDs (identificadores de segurança) do grupo FinanceAdmin e dos usuários são determinados no momento de criação e serão diferentes em seu exemplo. Por exemplo, o valor de SID fornecido (S-1-5-21-1787166779-1215870801-2157059049-1113) para FinanceAdmins precisa ser substituído pelo SID real do grupo FinanceAdmin que você deve criar na sua implantação. Você pode usar o Windows PowerShell para pesquisar o valor de SID desse grupo, atribuir esse valor a uma variável e usar a variável aqui. Para obter mais informações, consulte [dica do Windows PowerShell: trabalhando com SIDs](https://go.microsoft.com/fwlink/?LinkId=253545).

Você deve agora ter uma regra de acesso central que permita às pessoas acessar documentos do mesmo país e do mesmo departamento. A regra permite que o grupo FinanceAdmin edite documentos, e permite que o grupo FinanceException leia os documentos. Essa regra se destina apenas aos documentos classificados como Financeiros.

#### <a name="to-add-a-central-access-rule-to-a-central-access-policy"></a>Para adicionar uma regra de acesso central a uma política de acesso central

1. No painel esquerdo do Centro Administrativo do Active Directory, clique em **Controle de Acesso Dinâmico** e, então, em **Políticas de Acesso Central**.

2. No painel **Tarefas**, clique em **Novo** e, depois, em **Política de Acesso Central.**

3. Em **Criar Política de Acesso Central**, digite **Política Financeira** na caixa **Nome**.

4. Em **Regras de acesso central de membro**, clique em **Adicionar**.

5. Clique duas vezes na **Regra de Documentos Financeiros** para adicioná-la à lista **Adicionar as seguintes regras de acesso central** e clique em **OK**.

6. Clique em **OK** para concluir. Você deve ter agora uma política de acesso central denominada Política Financeira.

![guias de solução](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

```powershell
New-ADCentralAccessPolicy "Finance Policy" Add-ADCentralAccessPolicyMember
-Identity "Finance Policy"
-Member "Finance Documents Rule"
```

#### <a name="to-apply-the-central-access-policy-across-file-servers-by-using-group-policy"></a>Para aplicar a política de acesso central aos servidores de arquivos usando a Política de Grupo

1.  Na tela **Iniciar**, na caixa **Pesquisar**, digite **Gerenciamento de Política de Grupo**. Clique duas vezes em **Gerenciamento de Política de Grupo**.

    > [!TIP]
    > Se a configuração **Mostrar Ferramentas Administrativas** estiver desabilitada, a pasta **Ferramentas Administrativas** e seus conteúdos não aparecerão nos resultados de **Configurações**.

    > [!TIP]
    > Em seu ambiente de produção, você deve criar uma UO (Unidade Organizacional) de Servidor de Arquivos e adicionar todos os seus servidores de arquivos a essa UO, à qual você deseja aplicar essa política. Você poderá então criar uma política de grupo e adicionar essa UO a essa política.

2.  Nessa etapa, você deve editar o objeto de política de grupo criado na seção [Criar o controlador de domínio](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_Build) do ambiente de teste para incluir a política de acesso central que você criou. Na Editor de Gerenciamento de Política de Grupo, navegue até e selecione a unidade organizacional no domínio (contoso.com neste exemplo): **política de grupo Gerenciamento**, **floresta: contoso.com**, **domínios**, **contoso.com**, **contoso**, **FileServerOU**.

3.  Clique com o botão direito do mouse em **FlexibleAccessGPO**, e clique em **Editar**.

4.  Na janela do Editor de Gerenciamento de Política de Grupo, navegue para **Configuração do computador**, expanda **Políticas**, expanda **Configurações do Windows** e clique nas **Configurações de Segurança**.

5.  Expanda **Sistema de Arquivos**, clique com o botão direito do mouse em **Política de Acesso Central** e clique em **Gerenciar políticas de acesso central**.

6.  Na caixa de diálogo **Configuração de Políticas de Acesso Central**, adicione a **Política Financeira** e clique em **OK**.

7.  Role a tela para baixo até a **Configuração de Política de Auditoria Avançada** e expanda-a.

8.  Expanda **Políticas de Auditoria** e selecione **Acesso a Objeto**.

9. Clique duas vezes em **Preparo de Política de Acesso Central de Auditoria**. Marque todas as três caixas de seleção e clique em **OK**. Essa etapa permite que o sistema receba eventos de auditoria relacionados às Políticas de Preparo de Acesso Central.

10. Clique duas vezes em **Propriedades do Sistema de Arquivos de Auditoria**. Marque todas as três caixas de seleção e clique em **OK**.

11. Feche o Editor de Gerenciamento de Política de Grupo. Agora você incluiu a política de acesso central à Política de Grupo.

Para os controladores de domínio de um domínio fornecerem declarações ou dados de autorização de dispositivo, os controladores de domínio precisam ser configurados para dar suporte ao controle de acesso dinâmico.

#### <a name="to-enable-support-for-claims-and-compound-authentication-for-contosocom"></a>Para habilitar o suporte a declarações e autenticação composta para contoso.com

1.  Abra o Gerenciamento de Política de Grupo, clique em **contoso.com** e, depois, em **Controladores de Domínio**.

2.  Clique com botão direito na **Política de Controladores de Domínio Padrão** e clique em **Editar**.

3.  Na janela Editor de Gerenciamento de Política de Grupo, clique duas vezes em **Configuração do Computador**, duas vezes em **Políticas**, duas vezes em **Modelos Administrativos**, duas vezes em **Sistema** e, por fim, duas vezes em **KDC**.

4.  Clique duas vezes em **Suporte KDC para declarações, autenticação composta e proteção Kerberos**. Na caixa de diálogo **Suporte KDC para declarações, autenticação composta e proteção Kerberos**, clique em **Habilitado** e selecione **Com suporte** na lista suspensa **Opções**. (Você precisa habilitar essa configuração para utilizar declarações de usuário nas políticas de acesso central.)

5.  Feche o **Gerenciamento de Política de Grupo**.

6.  Abra um prompt de comando e digite `gpupdate /force`.

## <a name="deploy-the-central-access-policy"></a><a name="BKMK_1.4"></a>Implantar a política de acesso central

| Etapa nº | Etapa | Exemplo |
|--|--|--|
| 3.1 | Atribuir a CAP às pastas compartilhadas apropriadas no servidor de arquivos. | Atribua a política de acesso central à pasta compartilhada apropriada no servidor de arquivos. |
| 3.2 | Verificar se o acesso está configurado adequadamente. | Verifique o acesso de usuários de países e departamentos diferentes. |

Nesta etapa, você atribuirá a política de acesso central ao servidor de arquivos. Você fará logon no servidor de arquivos que está recebendo a política de acesso central criada nas etapas anteriores e atribuirá a política a uma pasta compartilhada.

#### <a name="to-assign-a-central-access-policy-to-a-file-server"></a>Para atribuir uma política de acesso central a um servidor de arquivos

1. No Gerenciador Hyper-V, conecte-se ao servidor FILE1. Faça logon no servidor usando CONTOSO\Administrator com a senha: <strong>pass@word1</strong> .

2. Abra um prompt de comandos com privilégios elevados e digite: **gpupdate /force**. Isso assegura que suas alterações na Política de Grupo entrem em vigor no servidor.

3. Será necessário atualizar as Propriedades de Recurso Global do Active Directory. Abra uma janela do Windows PowerShell com privilégios elevados e digite `Update-FSRMClassificationpropertyDefinition`. Clique em ENTER e feche o Windows PowerShell.

   > [!TIP]
   > Você também pode atualizar as Propriedades de Recursos Globais fazendo logon no servidor de arquivos. Para atualizar as Propriedades de Recursos Globais a partir do servidor de arquivos, faça o seguinte:
   >
   > 1. Logon no servidor de arquivos FILE1 como CONTOSO\Administrator, usando a senha <strong>pass@word1</strong> .
   > 2. Abra o Gerenciador de Recursos de Servidor de Arquivos. Para abrir o Gerenciador de Recursos de Servidor de Arquivos, clique em **Iniciar**, digite **gerenciador de recursos do servidor de arquivos** e clique em **Gerenciador de Recursos do Servidor de Arquivos**.
   > 3. No Gerenciador de Recursos de Servidor de Arquivos, clique em **Gerenciamento de Classificação de Arquivos**, clique com o botão direito do mouse em **Propriedades de Classificação** e clique em **Atualizar**.

4. Abra o Windows Explorer e, no painel esquerdo, clique na unidade D. Clique com o botão direito do mouse na pasta **Documentos Financeiros** e clique em **Propriedades**.

5. Clique na guia **Classificação**, clique em **País** e selecione **EUA** no campo **Valor**.

6. Clique em **Departamento** e selecione **Financeiro** no campo **Valor**. Em seguida, clique em **Aplicar**.

   > [!NOTE]
   > Lembre-se de que a política de acesso central foi configurada para os arquivos de destino do Departamento Financeiro. As etapas anteriores marcam todos os documentos da pasta que contêm os atributos País e Departamento.

7. Clique na guia **Segurança** e clique em **Avançado**. Clique na guia **Política Central**.

8. Clique em **Alterar**, selecione **Política Financeira** no menu suspenso e clique em **Aplicar**. Você poderá ver a **Regra de Documentos Financeiros** listada na política. Expanda o item para exibir todas as permissões que você definiu quando criou a regra no Active Directory.

9. Clique em **OK** para retornar ao Windows Explorer.

Na próxima etapa, você garantirá que o acesso esteja configurado adequadamente.  As contas de usuários precisam ter o atributo Departamento apropriado definido (defina-o usando o Centro Administrativo do Active Directory). A maneira mais fácil de exibir os resultados efetivos da nova política é usar a guia **Acesso Efetivo** no Windows Explorer. A guia **Acesso Efetivo** mostra os direitos de acesso de determinada conta de usuário.

#### <a name="to-examine-the-access-for-various-users"></a>Para examinar o acesso de vários usuários

1.  No Gerenciador Hyper-V, conecte-se ao servidor FILE1. Faça logon no servidor usando contoso\administrador. Navegue para D:\ no Windows Explorer. Clique com o botão direito do mouse na pasta **Documentos Financeiros** e clique em **Propriedades**.

2.  Clique na guia **Segurança**, clique em **Avançado** e, depois, clique na guia **Acesso Efetivo**.

3.  Para examinar as permissões de um usuário, clique em **selecionar um usuário**, digite o nome do usuário e, em seguida, clique em **exibir acesso efetivo** para ver os direitos de acesso efetivos. Por exemplo:

    -   Myriam Delesalle (MDelesalle) faz parte do departamento financeiro e deve ter acesso de leitura à pasta.

    -   Miles Reid (MReid) é membro do grupo FinanceAdmin e deve ter acesso de modificação à pasta.

    -   Esther Valle (EValle) não faz parte do departamento financeiro, mas é membro do grupo FinanceException e deve ter acesso de leitura.

    -   Maira Wenzel (MWenzel) não faz parte do departamento financeiro e não é membro dos grupos FinanceAdmin ou FinanceException. Ela não deve ter nenhum acesso à pasta.

    Observe a última coluna denominada **Acesso limitado por** na janela de acesso efetivo. Esta coluna informa quais Gates estão afetando as permissões da pessoa. Nesse caso, as permissões Compartilhar e NTFS permitem controle total a todos os usuários. No entanto, a política de acesso central restringe o acesso com base nas regras configuradas anteriormente.

## <a name="maintain-change-and-stage-the-policy"></a><a name="BKMK_1.5"></a>Manter: alterar e preparar a política

| Etapa nº | Etapa | Exemplo |
|--|--|--|
| 4.1 | Configurar declarações de dispositivo para clientes | Definir a configuração da política de grupo para habilitar declarações de dispositivo |
| 4.2 | Habilitar uma declaração para os dispositivos. | Habilite o tipo de declaração de país para os dispositivos. |
| 4.3 | Adicionar uma política de preparo à regra de acesso central existente que você deseja modificar. | Modifique a Regra de Documentos Financeiros para adicionar uma política de preparo. |
| 4.4 | Exibir os resultados da política de preparo. | Verifique as permissões do Ester Velle. |

#### <a name="to-set-up-group-policy-setting-to-enable-claims-for-devices"></a>Para definir uma configuração de política de grupo a fim de habilitar declarações para dispositivos

1.  Faça logon no DC1, abra o Gerenciamento de Política de Grupo, clique em **contoso.com**, clique em **Política de Domínio Padrão**, clique com o botão direito do mouse e selecione **Editar**.

2.  Na janela Editor de Gerenciamento de Política de Grupo, navegue para **Configuração do Computador**, **Políticas**, **Modelos Administrativos**, **Sistema**, **Kerberos**.

3.  Selecione **Suporte Kerberos a declarações de cliente, autenticação composta e proteção Kerberos** e clique em **Habilitar**.

#### <a name="to-enable-a-claim-for-devices"></a>Para habilitar uma declaração para dispositivos

1. Abra o servidor DC1 no Gerenciador do Hyper-V e faça logon como contoso\Administrator, com a senha <strong>pass@word1</strong> .

2. No menu **Ferramentas**, abra o Centro Administrativo do Active Directory.

3. Clique em **Modo de Exibição de Árvore**, expanda **Controle de Acesso Dinâmico**, clique duas vezes em **Tipos de Declarações** e clique duas vezes na declaração **país**.

4. Em **As declarações desse tipo podem ser emitidas para as seguintes classes**, marque a caixa de seleção **Computador**. Clique em **OK**.
   Agora, as caixas de seleção **Usuário** e **Computador** devem estar marcadas. A declaração de país pode agora ser usada com dispositivos, além dos usuários.

A próxima etapa é criar uma regra de política de preparo. As políticas de preparo podem ser usadas para monitorar os efeitos de uma nova entrada de política antes que você a habilite. Na etapa a seguir, você criará uma entrada de política de preparo e monitorará o efeito sobre sua pasta compartilhada.

#### <a name="to-create-a-staging-policy-rule-and-add-it-to-the-central-access-policy"></a>Para criar uma regra de política de preparo e adicioná-la à política de acesso central

1. Abra o servidor DC1 no Gerenciador do Hyper-V e faça logon como contoso\Administrator, com a senha <strong>pass@word1</strong> .

2. Abra o Centro Administrativo do Active Directory.

3. Clique em **Modo de Exibição de Árvore**, expanda **Controle de Acesso Dinâmico** e selecione **Regras de Acesso Central**.

4. Clique com o botão direito do mouse em **Regra de Documentos Financeiros** e clique em **Propriedades**.

5. Na seção **Permissões Propostas**, marque a caixa de seleção **Habilitar a configuração de preparação de permissões**, clique em **Editar** e, então, em **Adicionar**. Na janela **Entrada de Permissão para Permissões Propostas**, clique no link **Selecionar uma Entidade de Segurança**, digite **Usuários Autenticados** e clique em **OK**.

6. Clique no link **Adicionar uma condição** e adicione a seguinte condição: [**usuário**] [**país**] [**qualquer de**] [**recurso**] [**país**].

7. Clique em **Adicionar uma condição** novamente e adicione a seguinte condição: [**e**] [**dispositivo**] [**país**] [**qualquer de**] [**recurso**] [**país**]

8. Clique novamente em **Adicionar uma condição** e adicione a condição a seguir.
   E [**Usuário**] [**Grupo**] [**Membro de qualquer**] [**Valor**] \( **Financeexception**)

9. Para definir o grupo FinanceException, clique em **Adicionar itens** na janela **Selecionar Usuário, Computador, Conta de Serviço ou Grupo** e digite **FinanceException**.

10. Clique em **Permissões**, selecione **Controle Total** e clique em **OK**.

11. Na janela Configurações de Segurança Avançadas para Permissões Propostas, selecione **FinanceException** e clique em **Remover**.

12. Clique em **OK** duas vezes para concluir.

![guias de solução](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

```powershell
Set-ADCentralAccessRule
-Identity: "CN=FinanceDocumentsRule,CN=CentralAccessRules,CN=ClaimsConfiguration,CN=Configuration,DC=Contoso.com"
-ProposedAcl: "O:SYG:SYD:AR(A;;FA;;;BA)(A;;FA;;;SY)(A;;0x1301bf;;;S-1-21=1426421603-1057776020-1604)"
-Server: "WIN-2R92NN8VKFP.Contoso.com"
```

> [!NOTE]
> No cmdlet de exemplo acima, o valor de servidor reflete o servidor no ambiente de laboratório de teste. Você pode usar o Visualizador do Windows PowerShell History para pesquisar os cmdlets do Windows PowerShell para cada procedimento realizado no Centro Administrativo do Active Directory. Para obter mais informações, consulte [Visualizador de Histórico do Windows PowerShell:](../ad-ds/get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-.md)

Neste conjunto de permissões propostas, os membros do grupo FinanceException terão acesso total aos arquivos do próprio país quando acessarem esses arquivos usando um dispositivo do mesmo país do documento. Entradas de auditoria são disponibilizados no log de segurança dos servidores de arquivos quando alguém do departamento financeiro tenta acessar os arquivos. No entanto, as configurações de segurança não são impostas até que a política seja promovida do preparo.

No próximo procedimento, você verificará os resultados da política de preparo. Acesse a pasta compartilhada com um nome de usuário que tenha permissões baseadas na regra atual. Esther Valle (EValle) é membro de FinanceException e tem direitos de leitura no momento. De acordo com a política de preparo, EValle não deve ter nenhum direito.

#### <a name="to-verify-the-results-of-the-staging-policy"></a>Para verificar os resultados da política de preparo

1. Conecte-se ao servidor de arquivos FILE1 no Gerenciador do Hyper-V e faça logon como CONTOSO\Administrator, com a senha <strong>pass@word1</strong> .

2. Abra uma janela de prompt de comando e digite **gpupdate /force**. Isso assegura que suas alterações na Política de Grupo entrem em vigor no servidor.

3. No Gerenciador Hyper-V, conecte-se ao servidor CLIENT1. Faça logoff do usuário que está conectado no momento. Reinicie a máquina virtual CLIENT1. Em seguida, faça logon no computador usando contoso\EValle pass@word1 .

4. Clique duas vezes no atalho da área de trabalho para \\ \FILE1\Finance documentos. EValle ainda deve ter acesso aos arquivos. Mude novamente para o FILE1.

5. Abra o **Visualizador de Eventos** a partir do atalho da área de trabalho. Expanda **Logs do Windows** e selecione **Segurança**. Abra as entradas com a **ID de evento 4818**na categoria de tarefa de **preparo da política de acesso central** . Você verá que EValle recebeu permissão de acesso. No entanto, de acordo com a política de preparo, a usuária teria o acesso negado.

## <a name="next-steps"></a>Próximas etapas
Se tiver um sistema de gerenciamento de servidor central como o System Center Operations Manager, você também poderá configurar o monitoramento de eventos. Isso permite que os administradores monitorem os efeitos das políticas de acesso central antes de impô-las.
