# 🤠 Gato Pistoleiro — projeto Android (100% offline)

Tudo do jogo (código, sprites, sons e música) é gerado/guardado dentro do app.
Depois de instalado ele **não usa internet**. Moedas, conquistas, gatos, mapas,
melhorias, recorde e som ficam salvos no aparelho (localStorage).

## O que tem aqui
- `www/` → o jogo (index.html, sw.js, manifest, ícones, fonte)
- `scripts/get-font.js` → baixa a fonte pixel UMA vez (precisa de internet só nessa hora)
- `scripts/check-offline.js` → confere que não há recurso externo
- `.github/workflows/build-apk.yml` → gera o APK sozinho na nuvem
- `capacitor.config.json`, `package.json` → empacota o jogo como app Android

> **Sobre a fonte:** o visual retrô usa a fonte *Press Start 2P* (licença livre OFL).
> Ela precisa estar em `www/fonts/PressStart2P-Regular.ttf`. Os dois métodos abaixo
> já baixam a fonte para dentro do app. Se ela faltar, o jogo funciona igual,
> mas com uma fonte comum nos textos.

---
## Método A — Só pelo celular (sem Android Studio), usando o GitHub
1. Crie uma conta grátis em github.com e um repositório novo (ex.: `gato-pistoleiro`).
2. Envie os arquivos do zip. **Atenção:** a pasta `.github` tem nome oculto e o
   envio pelo celular pode ignorá-la. Faça assim:
   - Em *Add file → Upload files*, envie tudo **menos** a pasta `.github`.
   - Depois *Add file → Create new file*, digite o nome
     `.github/workflows/build-apk.yml` e cole o conteúdo do arquivo de mesmo nome do zip.
3. Aba **Actions → Gerar APK → Run workflow**. Espere uns 5 a 8 minutos.
4. Abra a execução concluída, baixe o artefato **gato-pistoleiro-apk** (zip) e extraia.
5. Instale o `app-debug.apk` (permita "instalar apps desconhecidos" quando o Android pedir).

## Método B — No computador (Windows/Mac/Linux)
Precisa de: Node.js 18+, Java 17 e Android Studio (ele instala o Android SDK).
```bash
npm install
npm run setup               # baixa a fonte (único passo que usa internet)
npm run check               # confere que está tudo offline
npx cap add android
npx @capacitor/assets generate --android
npx cap sync android
cd android && ./gradlew assembleDebug      # no Windows: gradlew.bat assembleDebug
```
O APK fica em `android/app/build/outputs/apk/debug/app-debug.apk`.
Para travar em retrato, adicione `android:screenOrientation="portrait"` na tag
`<activity>` de `android/app/src/main/AndroidManifest.xml` (o Método A já faz isso).
Também dá para rodar `npx cap open android` e usar *Build → Build APK(s)* no Android Studio.

## Testar offline
1. Instale o APK, abra uma vez, ative o **modo avião** e abra de novo: deve iniciar normalmente.
2. No navegador (opcional): sirva a pasta `www` (`npx serve www`), abra no Chrome, deixe o
   service worker instalar, desligue a rede e recarregue — o jogo continua abrindo.

## Publicar na Play Store (opcional)
O `app-debug.apk` serve para instalar direto. Para a loja é preciso um APK/AAB assinado
(`./gradlew bundleRelease` + sua chave de assinatura) e conta de desenvolvedor.

## Dica
Para ver as super habilidades sem esperar a sorte, abra a versão web com `#teste` no final do endereço.
