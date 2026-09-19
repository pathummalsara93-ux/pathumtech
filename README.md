# 🌱 @pathumtech/baileys

> **Fork maintained by [pathumtech](https://github.com/pathumtech)**
>
> Based on [@itsliaaa/baileys](https://www.npmjs.com/package/@itsliaaa/baileys) by [Lia Wynn](https://github.com/itsliaaa),
> which is a fork of [WhiskeySockets/Baileys](https://github.com/WhiskeySockets/Baileys).

[![Logo](https://files.catbox.moe/c5s9g0.jpg)](https://www.npmjs.com/package/@pathumtech/baileys)

<p align="center">
   Enhanced Baileys v7 with fixes for newsletter media uploads, plus support for interactive messages, albums, and additional message types.
   <br><br>
   <a href="https://www.npmjs.com/package/@pathumtech/baileys">
      <img src="https://img.shields.io/npm/v/@pathumtech/baileys?style=for-the-badge&logo=npm"/>
   </a>
   <a href="https://www.npmjs.com/package/@pathumtech/baileys">
      <img src="https://img.shields.io/npm/dm/@pathumtech/baileys?style=for-the-badge&logo=npm"/>
   </a>
   <a href="https://github.com/pathumtech/baileys">
      <img src="https://img.shields.io/github/stars/pathumtech/baileys?style=for-the-badge&logo=github"/>
   </a>
   <a href="LICENSE">
      <img src="https://img.shields.io/badge/license-MIT-blue?style=for-the-badge"/>
   </a>
   <a href="https://nodejs.org">
      <img src="https://img.shields.io/badge/node-%3E%3D20-339933?logo=node.js&labelColor=green&logoColor=white&style=for-the-badge"/>
   </a>
   <a href="#">
      <img src="https://img.shields.io/badge/ESM-only?logo=javascript&labelColor=yellow&logoColor=black&style=for-the-badge"/>
   </a>
</p>

> [!NOTE]
> This is a personal fork maintained by [pathumtech](https://github.com/pathumtech).
> All original credits belong to the original authors — see [Credits](#-credits) below.

### ✨ Highlights

This fork is designed for production use with a focus on clarity and safety:

- 🚫 No obfuscation. Easy to read and audit.
- 🚫 No auto-follow channel (newsletter) behavior.

> [!IMPORTANT]
> **Attribution notice from the original author ([Lia Wynn](https://github.com/itsliaaa)):**
>
> The following packages redistribute files and modifications originating from the
> original fork while removing contributor credits and modification notes:
>
> - [@nuisockets](https://www.npmjs.com/package/@nuisockets/baileys)
> - [@nuiisatoru](https://www.npmjs.com/package/@nuiisatoru/baileys)
> - [@nuiisweetberry](https://www.npmjs.com/package/@nuiisweetberry/baileys)
> - [@nuiisweety](https://www.npmjs.com/package/@nuiisweety/baileys)
> - [@lumina-md](https://www.npmjs.com/package/@lumina-md/baileys)
> - [@sairidev](https://www.npmjs.com/package/@sairidev/baileys-new)
> - [@lordmega/baileys](https://www.npmjs.com/package/@lordmega/baileys)
> - [phantom-baileys](https://www.npmjs.com/package/phantom-baileys)
> - [nexora-baileys](https://www.npmjs.com/package/nexora-baileys)
>
> Full credit and respect belong to:
>
> https://github.com/WhiskeySockets/Baileys
>
> **Forking is completely acceptable. Removing attribution, contributor credits, or modification history is not.**

> [!NOTE]
> 📄 This project is maintained with limited scope and is not intended to replace upstream Baileys.

### 🛠️ Internal Adjustments
- 🖼️ Fixed an issue where media could not be sent to newsletters due to an upstream issue.
- 📁 Reintroduced `makeInMemoryStore` with a minimal ESM adaptation and small adjustments for Baileys v7.
- 📦 Switched FFmpeg execution from `exec` to `spawn` for safer process handling.
- 🗃️ Added [`@napi-rs/image`](https://www.npmjs.com/package/@napi-rs/image) as a supported image processing backend in `getImageProcessingLibrary()`, offering a balance between performance and compatibility.

### 📨 Messages Handling & Compatibility
- 📩 Expanded messages support for:
   - 🖼️ Album Message
   - 👤 Group Status Message
   - 👉🏻 Interactive Message (buttons, lists, native flows, templates, carousels).
   - 🎞️ Status Mention Message
   - 📦 Sticker Pack Message
   - ✨ Rich Response Message
   - 🧾 Message with Code Blocks
   - 🌏 Message with Inline Entities
   - 📋 Message with Table
   - 💳 Payment-related Message (payment requests, invites, orders, invoices).
- 📰 Simplified sending messages with ad thumbnail using `externalAdReply`, without requiring manual `contextInfo`.
- 💭 Added support for quoting messages inside channel (newsletter).
- 🎀 Added support for custom button icon.

### 📥 Installation

```bash
# NPM
npm i @pathumtech/baileys@latest

# GitHub
npm i github:pathumtech/baileys
```

#### 🧩 Import (ESM & CJS)

```javascript
// --- ESM
import { makeWASocket } from '@pathumtech/baileys'

// --- CJS
const { makeWASocket } = require('@pathumtech/baileys')
```

### 🌐 Connect to WhatsApp (Quick Step)

```javascript
import { makeWASocket, delay, DisconnectReason, useMultiFileAuthState } from '@pathumtech/baileys'
import { Boom } from '@hapi/boom'
import pino from 'pino'

const myPhoneNumber = '6288888888888'
const logger = pino({ level: 'silent' })

const connectToWhatsApp = async () => {
   const { state, saveCreds } = await useMultiFileAuthState('session')

   const sock = makeWASocket({
      logger,
      auth: state
   })

   sock.ev.on('creds.update', saveCreds)

   sock.ev.on('connection.update', async (update) => {
      const { connection, lastDisconnect } = update
      if (connection === 'connecting' && !sock.authState.creds.registered) {
         await delay(1500)
         const code = await sock.requestPairingCode(myPhoneNumber)
         console.log('🔗 Pairing code', ':', code)
      }
      else if (connection === 'close') {
         const shouldReconnect = new Boom(connection?.lastDisconnect?.error)?.output?.statusCode !== DisconnectReason.loggedOut
         console.log('⚠️ Connection closed because', lastDisconnect.error, ', reconnecting ', shouldReconnect)
         if (shouldReconnect) {
            connectToWhatsApp()
         }
      }
      else if (connection === 'open') {
         console.log('✅ Successfully connected to WhatsApp')
      }
   })

   sock.ev.on('messages.upsert', async ({ messages }) => {
      for (const message of messages) {
         if (!message.message) continue
         console.log('🔔 Got new message', ':', message)
         await sock.sendMessage(message.key.remoteJid, {
            text: '👋🏻 Hello world'
         })
      }
   })
}

connectToWhatsApp()
```

### 📦 Fork Base

This fork is based on:

- [WhiskeySockets/Baileys](https://github.com/WhiskeySockets/Baileys) — the original Baileys library
- [@itsliaaa/baileys](https://www.npmjs.com/package/@itsliaaa/baileys) by [Lia Wynn](https://github.com/itsliaaa) — the enhanced fork this is based on

### 📣 Credits

**Original Baileys library:**
- [WhiskeySockets/Baileys](https://github.com/WhiskeySockets/Baileys)
- [purpshell](https://github.com/purpshell)
- [jlucaso1](https://github.com/jlucaso1)
- [adiwajshing](https://github.com/adiwajshing)

**Enhanced fork by:**
- [Lia Wynn (@itsliaaa)](https://github.com/itsliaaa) — original enhancements and modifications

**This fork is maintained by:**
- [pathumtech](https://github.com/pathumtech)

**Protocol Buffer definitions** maintained by [WPP Connect](https://github.com/wppconnect-team) via [`wa-proto`](https://github.com/wppconnect-team/wa-proto)

Special thanks to [itsreimau](https://github.com/itsreimau) for the fix to the `updateBlockStatus` implementation.

> [!CAUTION]
> ⚠️ **Modification, removal, or misrepresentation of these credits is strictly prohibited. Any redistribution or fork must preserve this section in its original form without exception.**

### 📄 License

MIT © pathumtech (fork of Lia Wynn / @itsliaaa, based on WhiskeySockets/Baileys)
