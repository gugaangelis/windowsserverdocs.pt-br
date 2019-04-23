Como alternativa, você pode especificar uma impressão digital, se você quiser usar seu próprio certificado. Isso pode ser útil se você quiser compartilhar um certificado em vários computadores ou usar um certificado associado a um TPM ou um HSM. Aqui está um exemplo de como criar um certificado de TPM associado (que impede de ter a chave privada roubado e usado em outro computador e requer um TPM 1.2):

```powersehll
$tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
```


<!-- Appears in set-up-hgs-for-always-encrypted-in-sql-server.md and guarded-fabric-create-host-key.md
-->