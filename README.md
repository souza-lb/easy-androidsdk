# EASY ANDROID SDK - Instalação e Desinstalação Automática

## Visão Geral
O Easy Android SDK fornece scripts para instalação e desinstalação simplificada do Android SDK em sistemas Linux. Os scripts automatizam todo o processo, incluindo configuração de ambiente, instalação de componentes essenciais e gerenciamento de dependências.

## 📦 Como instalar?

**No terminal com sua conta de usuário comum execute:**

```bash
wget -qO- https://raw.githubusercontent.com/souza-lb/easy-androidsdk/main/install | bash
```

### Saída Esperada:
```
--------------------------------------------
  Instalação Android SDK - Versão 1.0        
--------------------------------------------

ATENÇÃO: Todos os arquivos temporários serão
         armazenados em: /tmp/androidsdk-XXXXX

Progresso:
1 - Verificar dependências
2 - Verificar instalação existente
3 - Criar estrutura de pastas
4 - Baixar Command Line Tools
5 - Instalar Command Line Tools
6 - Configurar variáveis de ambiente
7 - Aceitar licenças

[1/7] Verificando dependências... OK
[2/7] Verificando instalação existente... OK
[3/7] Criando estrutura de pastas... OK
[4/7] Baixando Command Line Tools... OK
[5/7] Instalando Command Line Tools... OK
[6/7] Configurando variáveis de ambiente... OK
[7/7] Aceitando licenças do Android SDK... OK

--------------------------------------------
 Instalando componentes Android SDK          
--------------------------------------------
Esta etapa pode levar vários minutos...
Por favor, aguarde...

[========================================] 100% (8/8) Concluído
Todos os componentes foram instalados com sucesso.

--------------------------------------------
  Instalação concluída com sucesso!          
--------------------------------------------

Próximos passos:
1. Recarregue o terminal ou execute:
   source ~/.bashrc
2. Verifique a instalação:
   sdkmanager --list
3. Crie um AVD:
   avdmanager create avd -n Meu_Dispositivo -k 'system-images;android-30;google_apis;x86_64'
4. Inicie o emulador:
   emulator -avd Meu_Dispositivo &
```

## Comandos Após Instalação

### Gerenciar SDK
```bash
# Listar pacotes disponíveis
sdkmanager --list

# Instalar novo componente
sdkmanager "build-tools;35.0.0"

# Atualizar todos os pacotes
sdkmanager --update
```

### Gerenciar AVDs (Android Virtual Devices)
```bash
# Criar novo dispositivo virtual
avdmanager create avd -n Meu_Dispositivo -k "system-images;android-30;google_apis;x86_64"

# Listar dispositivos disponíveis
avdmanager list avd

# Iniciar emulador
emulator -avd Meu_Dispositivo &

# Excluir dispositivo
avdmanager delete avd -n Meu_Dispositivo
```

### ADB (Android Debug Bridge)
```bash
# Listar dispositivos conectados
adb devices

# Instalar aplicativo
adb install app.apk

# Acessar shell do dispositivo
adb shell
```

## 🚀 Criação Personalizada de AVD

Para criar um dispositivo virtual (AVD) com configurações específicas:

```bash
# Cria um dispositivo virtual com configurações personalizadas
avdmanager create avd \
-n "meu-avd" \
-k "system-images;android-30;google_apis;x86_64" \
-d "Nexus One" \
-c 512M \
--force

# Habilita teclado físico e botões de navegação
sed -i \
-e 's/^hw\.keyboard\s*=\s*no.*/hw.keyboard = yes/' \
-e 's/^hw\.mainKeys\s*=\s*no.*/hw.mainKeys = no/' \
"$HOME/.android/avd/meu-avd.avd/config.ini"

# Inicia o emulador com otimizações de desempenho
emulator -avd meu-avd \
-no-boot-anim \      # Desativa animação de boot (início mais rápido)
-gpu host \          # Usa aceleração de GPU da máquina hospedeira
-qemu -enable-kvm    # Habilita virtualização KVM (requer suporte de hardware)
```

### Opções de Configuração:
- `-n`: Nome do AVD (deve ser único)
- `-k`: Imagem de sistema (deve corresponder a uma imagem instalada)
- `-d`: Dispositivo de referência (use `avdmanager list device` para ver modelos)
- `-c`: Tamanho do cartão SD virtual (ex: 512M, 2G)
- `--force`: Sobrescreve AVD existente com mesmo nome
- `-gpu host`: Usa GPU física para melhor desempenho gráfico
- `-no-boot-anim`: Acelera inicialização do emulador

### Personalizações Comuns:
```bash
# Alterar densidade de tela
sed -i 's/^hw\.lcd\.density\s*=\s*.*/hw.lcd.density = 240/' config.ini

# Aumentar memória RAM
sed -i 's/^hw\.ramSize\s*=\s*.*/hw.ramSize = 2048/' config.ini

# Habilitar câmera traseira
sed -i 's/^hw\.camera\.back\s*=\s*.*/hw.camera.back = virtualscene/' config.ini

# Alterar orientação inicial
sed -i 's/^hw\.initOrientation\s*=\s*.*/hw.initOrientation = landscape/' config.ini

# Habilitar armazenamento persistente
sed -i 's/^disk\.dataPartition\.size\s*=\s*.*/disk.dataPartition.size = 2G/' config.ini
```

## Fluxo de Trabalho Recomendado
1. Recarregue o terminal: `source ~/.bashrc`
2. Crie um novo AVD com a versão Android desejada
3. Personalize as configurações do AVD conforme necessário
4. Inicie o emulador com otimizações de desempenho
5. Desenvolva e teste seu aplicativo
6. Use ADB para instalar/debuggar aplicativos

## Notas Importantes
- Instalação completa requer ~5GB de espaço em disco
- Componentes instalados por padrão:
  - Platform Tools (ADB, Fastboot)
  - Android 35 SDK Platform
  - Build Tools 34.0.0 e 35.0.0
  - CMake 3.22.1
  - NDK 26.1.10909125
  - Emulador e Imagem de Sistema Android 30
- Mantém backups do `.bashrc` durante modificações
- Compatível com Ubuntu, Debian, Fedora e derivados
- Sempre solicita confirmação para operações críticas
- Para melhor desempenho do emulador:
  - Habilite virtualização no BIOS
  - Use imagens x86_64 com Google APIs
  - Aloque pelo menos 4GB de RAM para o sistema hospedeiro

## 🗑️ Como desinstalar?

**No terminal com sua conta de usuário comum execute:**

```bash
wget -qO- https://raw.githubusercontent.com/souza-lb/easy-androidsdk/main/uninstall | bash
```

### Etapas da Desinstalação:
1. **Verificação de instalação**: Confirma existência do Android SDK
2. **Confirmação do usuário**: Solicita confirmação para desinstalação
3. **Remoção de configurações**:
   - Remove variáveis de ambiente do `.bashrc`
   - Cria backup do arquivo original
4. **Remoção da instalação principal**:
   - Remove o diretório `$HOME/android-sdk`
   - Mostra tamanho da instalação antes de remover
   - Remove arquivo de repositórios (opcional)
5. **Finalização**: Exibe mensagem de conclusão

### Saída Esperada:
```
--------------------------------------------
  Desinstalação Android SDK - Versão 1.0     
--------------------------------------------

1 - Verificar instalação
2 - Remover entradas do bashrc
3 - Remover arquivos do SDK
4 - Finalizar desinstalação

Instalação localizada: /home/user/android-sdk
Deseja realmente desinstalar o Android SDK? [s/N]: s
Removendo configurações do .bashrc...
Configurações removidas do .bashrc!
Backup salvo em: /home/user/.bashrc.backup-androidsdk
Verificando arquivos do SDK...
Tamanho da pasta do SDK: 4.7G
Atenção! Esta operação é irreversível!
Deseja remover todos os arquivos do Android SDK? [s/N]: s
Confirme digitando 'remover': remover
Removendo arquivos do SDK...
Pasta do SDK removida: /home/user/android-sdk

--------------------------------------------
   Desinstalação concluída com sucesso!      
--------------------------------------------

Recomendações:
1. Recarregue seu terminal com:
   source ~/.bashrc
2. Verifique se todas as entradas foram removidas

Suporte: souzalb@proton.me
```

---

## ❤️ Apoie o Projeto

**Dúvidas, sugestões e contribuições?**  
Leonardo Bruno  
souzalb@proton.me  

**Gostou do projeto e quer realizar uma contribuição voluntária?**  
*(Pode ser o valor de uma xícara de café ou chá...) ☕ 🍵*  

Chave PIX:  
`8dcc7e3c-0c6a-4c6f-a4c0-26a5e62686db`  

Ou utilize o QR Code abaixo:  

<p align="center">
  <img src="images/qrcode-pix.png" alt="QR Code PIX" width="200">
</p>

**Você também pode utilizar o PayPal:**  

[![PayPal](https://img.shields.io/badge/Donate-PayPal-00457C?style=for-the-badge&logo=paypal)](https://www.paypal.com/donate/?hosted_button_id=EQVW5QQ7GBGSY)

<p align="center">
  <img src="images/qrcode-paypal.png" alt="QR Code PayPal" width="200">
</p>

**A utilização deste projeto é livre para alterações e adaptações**  
*Desde que feita a devida referência ao repositório original e seu criador.*
```
