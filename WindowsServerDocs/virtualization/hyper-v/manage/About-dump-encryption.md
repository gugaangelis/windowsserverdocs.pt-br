---
title: Sobre a criptografia de despejo
description: Descreve como criptografar arquivos de despejo e solucionar problemas de criptografia.
ms.prod: windows-server-threshold
manager: dongill
ms.topic: article
author: larsiwer
ms.asset: b78ab493-e7c3-41f5-ab36-29397f086f32
ms.author: kathydav
ms.date: 11/03/2016
ms.openlocfilehash: e0ca8829aa8f93e7543b4183539beade3b4aeb47
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812387"
---
# <a name="about-dump-encryption"></a>Sobre a criptografia de despejo
Criptografia de despejo pode ser usada para criptografar os despejos de memória e despejos de memória em tempo real gerados para um sistema. Os despejos são criptografados usando uma chave de criptografia simétrica que é gerada para cada despejo. Essa chave em si é criptografada usando a chave pública especificada pelo administrador confiável do host (falha despejo criptografia protetor de chave). Isso garante que somente alguém com a chave privada correspondente pode descriptografar e, portanto, acessar o conteúdo do despejo. Esse recurso é utilizado em uma malha protegida.
Observação: Se você configurar a criptografia de despejo, também desabilite relatório de erros do Windows. WER não é possível ler criptografado os despejos de memória.

# <a name="configuring-dump-encryption"></a>Configurando a criptografia de despejo
## <a name="manual-configuration"></a>Configuração manual
Para ativar a criptografia de despejo de memória usando o registro, configure os seguintes valores de registro em `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl`

| Nome do valor | Tipo | Valor |
| ---------- | ---- | ----- |
| DumpEncryptionEnabled | DWORD | 1 para habilitar a criptografia de despejo, 0 para desabilitar a criptografia de despejo |
| EncryptionCertificates\Certificate.1::PublicKey | Binário | Chave pública (RSA, 2048 bits) que deve ser usado para criptografar os despejos de memória. Isso deve ser formatado como [BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx). |
| EncryptionCertificates\Certificate.1::Thumbprint | String | Impressão digital do certificado para permitir uma pesquisa automática de chave privada no repositório de certificados local ao descriptografar um despejo de memória. |


## <a name="configuration-using-script"></a>Usando o script de configuração
Para simplificar a configuração, uma [exemplo de script](https://github.com/Microsoft/Virtualization-Documentation/tree/live/hyperv-tools/DumpEncryption) está disponível para habilitar a criptografia de despejo com base em uma chave pública de um certificado.

1. Em um ambiente confiável: Criar um certificado com uma chave RSA de 2048 bits e exporte o certificado público
2. Em hosts de destino: Importar o certificado público para o repositório de certificados local
3. Execute o script de configuração de exemplo 
    ```
    .\Set-DumpEncryptionConfiguration.ps1 -Certificate (Cert:\CurrentUser\My\093568AB328DF385544FAFD57EE53D73EFAAF519) -Force
    ```

# <a name="decrypting-encrypted-dumps"></a>Descriptografando criptografados despejos de memória
Para descriptografar um arquivo de despejo criptografado existente, você precisa baixar e instalar as ferramentas de depuração para Windows. Esse conjunto de ferramentas contém KernelDumpDecrypt.exe que pode ser usado para descriptografar um arquivo de despejo criptografado.
Se o certificado, incluindo a chave privada está presente no repositório de certificados do usuário atual, o arquivo de despejo pode ser descriptografado por meio da chamada

```
    KernelDumpDecrypt.exe memory.dmp memory_decr.dmp
```
Após a descriptografia, ferramentas como o WinDbg podem abrir o arquivo de despejo descriptografada.

# <a name="troubleshooting-dump-encryption"></a>Solução de problemas de criptografia de despejo
Se a criptografia de despejo está habilitada em um sistema, mas nenhum despejos são gerados, verifique se o sistema `System` log de eventos para `Kernel-IO` 1207 do evento. Quando não é possível inicializar a criptografia de despejo, esse evento é criado e despejos de memória são desabilitados.

| Mensagem de erro detalhada | Etapas para reduzir |
| ---------------------- | ----------------- |
| Público impressão digital ou a chave do registro ausente | Verifique se ambos os valores do Registro existirem no local esperado |
| Chave pública inválida | Certifique-se de que a chave pública armazenada no valor do registro PublicKey é armazenada como [BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx). |
| Tamanho da chave pública sem suporte | Atualmente, há suporte para apenas chaves RSA de 2048 bits. Configurar uma chave que corresponda a esse requisito |

Verifique também se o valor `GuardedHost` sob `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled` é definido como um valor diferente de 0. Isso desabilita os despejos de memória completamente. Se esse for o caso, defina-o como 0.
