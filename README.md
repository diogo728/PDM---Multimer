# Multimer

Aplicação Android nativa (Kotlin, Android Studio, API mínima 29) de alarmes, simples e compostos, bem como de outras ferramentas associadas à gestão de tempo pessoal, como cronómetro e temporizador dinâmicos.
Funcionamento 100% offline.

Trabalho prático individual de Programação de Dispositivos Móveis (UBI, 2026/27).
Autor: **Diogo Rafael Nunes Freire Nogueira, nº53851**

## Como abrir, compilar e testar
1. Abrir a pasta do projeto no Android Studio (versão recente) e esperar pelo *Gradle Sync*
   (precisa de internet só para descarregar as dependências).
2. Escolher um emulador ou telemóvel com Android 10 (API 29) ou superior e carregar em *Run*.
3. Em Android 13+ aceitar a permissão de notificações; para alarmes exatos pode ser preciso ativar
   "Alarmes e lembretes" nas definições da app.
4. Testes unitários: `./gradlew test`.

## Funcionalidades
**Base offline (sem conta, sem internet):**
- **Alarmes**: etiqueta, hora, repetição por dias, vibração, volume crescente, adiar e **biblioteca de toques própria da app**
  (15 sons incluídos no APK, não dependem do aparelho); criar, listar, editar, duplicar e eliminar (com confirmação).
  Cada alarme mostra a data da última alteração.
- **Cascata**: vários alarmes num só, iguais entre si (sequenciais, "de X em X tempo") ou distintos (passos personalizados com
  tempo, nome, som, vibração e modo próprios). Têm barra de progresso por passos, pausar, saltar e parar, e notificação própria.
- **Temporizador** (roda h/min/s, anel de progresso, recentes) e **Cronómetro** (voltas, modo de foco), mais tradicionais.
- Notificações adequadas a cada mecanismo (alarme, cascata, temporizador, cronómetro).
- **Partilha offline**: gerar um **QR code** ou um **código de texto** (copiar/enviar por mensagem) e importar noutro aparelho
  lendo o QR ou colando o código. O que chega é validado e entra sempre desligado.
- Modo claro e escuro, adaptação a vários tamanhos de ecrã.

**Desenvolvido mas não incluído nesta versão, por colisões formais com o regulamentado no enunciado:** contas/perfil, sincronização entre aparelhos (alarmes, nunca o estado
ligado/desligado) com base de dados na nuvem, e **sala coletiva ao vivo** (um administrador inicia uma cascata para todos os
participantes, que entram por código, QR ou como convidados). Os botões existem na interface, etiquetados como "Em desenvolvimento".

## Armazenamento
- **Room (SQLite)** guarda alarmes, cascatas (com os respetivos passos) e temporizadores recentes,
  com migrações da base de dados entre versões. Os dados mantêm-se depois de fechar a app e de reiniciar o aparelho.
- **SharedPreferences** só para definições (estado do cronómetro, último temporizador, etc.).
- Estrutura: cada registo tem identificador único (`id`), campos próprios (hora, etiqueta, repetição, título, passos...)
  e datas (`updatedAt`).

## Organização do código
`data/` (entidades, DAOs, repositórios) · `ui/` (activities, fragments, adapters, ViewModels, vistas próprias) ·
`service/` e `receiver/` (execução em segundo plano e alarmes) · `util/` (ajudantes).
Arquitetura MVVM simples: a interface observa os ViewModels, que usam os repositórios sobre o Room.

## Bibliotecas externas (justificação)
- **Room**: persistência local estruturada.
- **ZXing core / zxing-android-embedded**: gerar e ler QR codes na partilha de alarmes e cascatas.
- **BlurView** e **AndroidX DynamicAnimation**: efeitos visuais da barra de abas e animações de mola.
- **Firebase Auth/Firestore** (opcional, em desenvolvimento): contas e sincronização entre aparelhos. **Está desativado nesta
  entrega**: o `google-services.json` não foi incluído (sem chaves no projeto) e a app arranca só em modo offline.

## Decisões e dificuldades
- Alarmes e cascatas usam `AlarmManager` e relógio monotónico para tocarem com o ecrã desligado e
  não saltarem com mudanças de hora/fuso.
- Dificuldades: reagendar alarmes após reinício/mudança de fuso, manter várias cascatas em execução e
  migrar a base de dados sem perder dados. Criação e bom funcionamento das salas virtuais.

## Utilização de IA e código externo
Partes deste projeto foram desenvolvidas com o apoio de ferramentas de IA generativa
(**Claude e Gemini 3.6 Flash via Android Studio**), usadas para gerar e rever código (orientação de desenvolvimento prático e aprimoramento visual).
Bibliotecas de terceiros: ver a secção acima. A fonte "Multimer Sans" está descrita em `FONTE_TIPOGRAFICA.md`
(licença em `app/src/main/assets/licenses/`).
