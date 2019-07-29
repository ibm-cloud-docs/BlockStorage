---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

keywords: Block storage, encryption, LUKS, RHEL, Linux, security, auxiliary storage

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Atingindo a criptografia total de disco com o LUKS no RHEL6
{: #LUKSencryption}

É possível criptografar partições em seu servidor RHEL6 com o Linux Unified Key Setup-on-disk-format
(LUKS), que é importante quando se trata de computadores móveis e de mídia removível. O LUKS permite que múltiplas chaves de usuário decriptografem uma chave mestra
que é usada para a criptografia em massa da partição.

Estas etapas assumem que o servidor pode acessar um novo volume não criptografado do {{site.data.keyword.blockstoragefull}} que não estava formatado nem montado. Para obter mais informações sobre como conectar o {{site.data.keyword.blockstorageshort}} a um host do Linux, consulte [Conectando-se ao armazenamento no Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux).

O {site.data.keyword.blockstorageshort}} que é provisionado na [maioria dos data centers](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC) é provisionado automaticamente com a criptografia em repouso gerenciada por provedor. Para obter mais informações, consulte [Protegendo seus dados: criptografia em repouso gerenciada pelo provedor](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption).
{:note}

## O que o LUKS faz

- Criptografa dispositivos de bloco inteiros e é, portanto, bem apropriado para proteger o conteúdo de dispositivos móveis, como mídia de armazenamento removível ou unidades de disco de notebook.
- O conteúdo subjacente do dispositivo de bloco criptografado é arbitrário, tornando útil para
criptografia de dispositivos de troca. A criptografia também pode ser útil com determinados bancos de dados
que usam dispositivos de bloco especialmente formatados para armazenamento de dados.
- Usa o subsistema de kernel do mapeador de dispositivo existente.
- Fornece reforço de passphrase, que protege contra anexações de dicionário.
- Permite que os usuários incluam chaves ou passphrases de backup, já que os dispositivos LUKS
contêm múltiplos slots de chave.


## O que o LUKS não faz

- Permitir que os aplicativos que requerem muitos usuários (mais de oito) tenham chaves de acesso distintas para os mesmos dispositivos.
- Trabalhe com aplicativos que requerem criptografia de nível de arquivo. Para obter mais
informações, consulte o [Guia de segurança do RHEL](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Security_Guide/sec-Encryption.html){: external}.

## Configurando um volume criptografado LUKS com o {{site.data.keyword.blockstorageshort}} Endurance

O processo de criptografia de dados cria um carregamento no host que pode potencialmente afetar o desempenho.
{:note}

1. Digite o seguinte comando em um prompt de shell como raiz para instalar o pacote necessário:   <br/>
   ```
   # yum install cryptsetup-luks
   ```
   {: pre}
2. Obtenha o ID do disco:<br/>
   ```
   # fdisk –l | grep /dev/mapper
   ```
   {: pre}
3. Localize seu volume na listagem.
4. Criptografe o dispositivo de bloco;

   1. Esse comando inicializa o volume e é possível configurar uma passphrase. <br/>

      ```
      # cryptsetup -y -v luksFormat /dev/mapper/3600a0980383034685624466470446564
      ```
      {: pre}

   2. Responda com SIM (todas as letras maiúsculas).

   3. O dispositivo agora aparece como um volume criptografado:

      ```
      # blkid | grep LUKS
      /dev/mapper/3600a0980383034685624466470446564: UUID="46301dd4-035a-4649-9d56-ec970ceebe01" TYPE="crypto_LUKS"
      ```

5. Abra o volume e crie um mapeamento.<br/>
   ```
   # cryptsetup luksOpen /dev/mapper/3600a0980383034685624466470446564 cryptData
   ```
   {: pre}
6. Insira a passphrase.
7. Verifique o mapeamento e visualize o status do volume criptografado.   <br/>
   ```
   # cryptsetup -v status cryptData
   /dev/mapper/cryptData is active.
     type:  LUKS1
     cipher:  aes-cbc-essiv:sha256
     keysize: 256 bits
     device:  /dev/mapper/3600a0980383034685624466470446564
     offset:  4096 sectors
     size:    41938944 sectors
     mode:    read/write
     Command successful
   ```
8. Grave dados aleatórios em `/dev/mapper/cryptData` no dispositivo criptografado. Essa ação assegura que o mundo exterior veja isso como dados aleatórios, o que significa que são protegidos contra a divulgação de padrões de uso. Esta etapa pode demorar um pouco.<br/>
    ```
    # shred -v -n1 /dev/mapper/cryptData
    ```
    {: pre}
9. Formate o volume.<br/>
   ```
   # mkfs.ext4 /dev/mapper/cryptData
   ```
   {: pre}
10. Monte o volume.<br/>
   ```
   # mkdir /cryptData
   ```
   {: pre}
   ```
   # mount /dev/mapper/cryptData /cryptData
   ```
   {: pre}
   ```
   # df -H /cryptData
   ```
   {: pre}

### Desmontando e fechando o volume criptografado com segurança
   ```
   # umount /cryptData
   # cryptsetup luksClose cryptData
   ```
   {: codeblock}

### Remontando e montando uma partição criptografada pelo LUKS existente
   ```
   # cryptsetup luksOpen /dev/mapper/3600a0980383034685624466470446564 cryptData
      Enter the password previously provided.
   # mount /dev/mapper/cryptData /cryptData
   # df -H /cryptData
   # lsblk
   NAME                                       MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
   xvdb                                       202:16   0    2G  0 disk
   └─xvdb1                                    202:17   0    2G  0 part  [SWAP]
   xvda                                       202:0    0   25G  0 disk
   ├─xvda1                                    202:1    0  256M  0 part  /boot
   └─xvda2                                    202:2    0 24.8G  0 part  /
   sda                                          8:0    0   20G  0 disk
   └─3600a0980383034685624466470446564 (dm-0) 253:0 0 20G 0 mpath
   └─cryptData (dm-1)                       253:1    0   20G  0 crypt /cryptData
   sdb                                          8:16   0   20G  0 disk
   └─3600a0980383034685624466470446564 (dm-0) 253:0 0 20G 0 mpath
   └─cryptData (dm-1)                       253:1    0   20G  0 crypt /cryptData
   ```
