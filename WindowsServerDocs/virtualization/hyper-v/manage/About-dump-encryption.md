---
title: Sobre a criptografia de despejo
description: Descreve como criptografar arquivos de despejo e solucionar problemas de criptografia.
ms.topic: article
ms.author: benarm
author: BenjaminArmstrong
ms.asset: b78ab493-e7c3-41f5-ab36-29397f086f32
ms.date: 11/03/2016
ms.openlocfilehash: d2258a810993ea903efe670355720a5fc65a888b
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746481"
---
# <a name="about-dump-encryption"></a>Sobre a criptografia de despejo
A criptografia de despejo pode ser usada para criptografar despejos de memória e despejos dinâmicos gerados para um sistema. Os despejos são criptografados usando uma chave de criptografia simétrica que é gerada para cada despejo. Em seguida, essa chave é criptografada usando a chave pública especificada pelo administrador confiável do host (protetor de chave de criptografia de despejo de memória). Isso garante que apenas alguém que tenha a chave privada correspondente possa descriptografar e, portanto, acessar o conteúdo do despejo. Esse recurso é utilizado em uma malha protegida.
Observação: se você configurar a criptografia de despejo, também desabilite Relatório de Erros do Windows. O WER não pode ler despejos de memória criptografados.

## <a name="configuring-dump-encryption"></a>Configurando a criptografia de despejo
### <a name="manual-configuration"></a>Configuração manual
Para ativar a criptografia de despejo usando o registro, configure os seguintes valores de registro em `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl`

| Nome do valor | Type | Valor |
| ---------- | ---- | ----- |
| DumpEncryptionEnabled | DWORD | 1 para habilitar a criptografia de despejo, 0 para desabilitar a criptografia de despejo |
| EncryptionCertificates\Certificate.1::P ublicKey | Binário | Chave pública (RSA, 2048 bits) que deve ser usada para criptografar despejos. Isso deve ser formatado como [BCRYPT_RSAKEY_BLOB](/windows/win32/api/bcrypt/ns-bcrypt-bcrypt_rsakey_blob). |
| EncryptionCertificates\Certificate.1:: impressão digital | String | Impressão digital do certificado para permitir a pesquisa automática de chave privada no repositório de certificados local ao descriptografar um despejo de memória. |


### <a name="configuration-using-script"></a>Configuração usando script
Para simplificar a configuração, um [script de exemplo](https://github.com/Microsoft/Virtualization-Documentation/tree/live/hyperv-tools/DumpEncryption) está disponível para habilitar a criptografia de despejo com base em uma chave pública de um certificado.

1. Em um ambiente confiável: Crie um certificado com uma chave RSA de 2048 bits e exporte o certificado público
2. Em hosts de destino: importe o certificado público para o repositório de certificados local
3. Executar o script de configuração de exemplo
    ```
    .\Set-DumpEncryptionConfiguration.ps1 -Certificate (Cert:\CurrentUser\My\093568AB328DF385544FAFD57EE53D73EFAAF519) -Force
    ```

## <a name="decrypting-encrypted-dumps"></a>Descriptografando despejos criptografados
Para descriptografar um arquivo de despejo criptografado existente, você precisa baixar e instalar as ferramentas de depuração para Windows. Esse conjunto de ferramentas contém KernelDumpDecrypt.exe que podem ser usados para descriptografar um arquivo de despejo criptografado.
Se o certificado que inclui a chave privada estiver presente no repositório de certificados do usuário atual, o arquivo de despejo poderá ser descriptografado chamando

```
    KernelDumpDecrypt.exe memory.dmp memory_decr.dmp
```
Após a descriptografia, ferramentas como o WinDbg podem abrir o arquivo de despejo descriptografado.

## <a name="troubleshooting-dump-encryption"></a>Solucionando problemas de criptografia de despejo
Se a criptografia de despejo estiver habilitada em um sistema, mas nenhum despejo estiver sendo gerado, verifique o `System` log de eventos do sistema para o `Kernel-IO` evento 1207. Quando a criptografia de despejo não puder ser inicializada, esse evento será criado e os despejos serão desabilitados.

| Mensagem de erro detalhada | Etapas para mitigar |
| ---------------------- | ----------------- |
| Registro de chave pública ou impressão digital ausente | Verificar se ambos os valores do registro existem no local esperado |
| Chave pública inválida | Verifique se a chave pública armazenada no valor do registro PublicKey está armazenada como [BCRYPT_RSAKEY_BLOB](/windows/win32/api/bcrypt/ns-bcrypt-bcrypt_rsakey_blob). |
| Tamanho de chave pública sem suporte | Atualmente, há suporte apenas para chaves RSA de 2048 bits. Configurar uma chave que corresponda a esse requisito |

Verifique também se o valor `GuardedHost` em `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled` está definido com um valor diferente de 0. Isso desabilita completamente os despejos de memória. Se esse for o caso, defina-o como 0.