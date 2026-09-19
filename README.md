# 🌱 @pathumtech/baileys

> **Fork maintained by [pathumtech](https://github.com/pathumtech)**
>
> Based on [@itsliaaa/baileys](https://www.npmjs.com/package/@itsliaaa/baileys) by [Lia Wynn](https://github.com/itsliaaa), which is a fork of [WhiskeySockets/Baileys](https://github.com/WhiskeySockets/Baileys).

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

### ✨ Highlights

This fork designed for production use with a focus on clarity and safety:

- 🚫 No obfuscation. Easy to read and audit.
- 🚫 No auto-follow channel (newsletter) behavior.

> [!IMPORTANT]
> Hi everyone,
>
> I want to clarify two separate attribution issues regarding packages derived from this fork.
>
> 1. Direct redistribution of my modifications without attribution
>
> The following packages are operated by the same individual under multiple npm accounts:
>
> - [@nuisockets](https://www.npmjs.com/package/@nuisockets/baileys)
> - [@nuiisatoru](https://www.npmjs.com/package/@nuiisatoru/baileys)
> - [@nuiisweetberry](https://www.npmjs.com/package/@nuiisweetberry/baileys)
> - [@nuiisweety](https://www.npmjs.com/package/@nuiisweety/baileys)
>
> These packages redistribute files and modifications originating from this fork while removing contributor credits and modification notes.
>
> 2. Rebranded republishes of this fork
>
> - [@lumina-md](https://www.npmjs.com/package/@lumina-md/baileys)
> - [@sairidev](https://www.npmjs.com/package/@sairidev/baileys-new)
> - [@lordmega/baileys](https://www.npmjs.com/package/@lordmega/baileys)
> - [phantom-baileys](https://www.npmjs.com/package/phantom-baileys)
> - [nexora-baileys](https://www.npmjs.com/package/nexora-baileys)
>
> These packages primarily repackage or republish this fork under different names while failing to preserve proper attribution, credits, or modification notes.
> 
> To be clear, I am **NOT** the original maintainer of Baileys. Full credit and respect belong to:
>
> https://github.com/WhiskeySockets/Baileys
>
> **Forking is completely acceptable. Removing attribution, contributor credits, or modification history is not.**
>
> Please report if necessary.
>
> Thank you. 🤍

> [!NOTE]
> 📄 This project is maintained with limited scope and is not intended to replace upstream Baileys.
>
> 😞 And, really sorry for my bad english.

### 📋 Table of Contents
- [📋 Table of Contents](#-table-of-contents)
- [✨ Highlights](#-highlights)
- [🛠️ Internal Adjustments](#%EF%B8%8F-internal-adjustments)
- [📨 Messages Handling & Compatibility](#-messages-handling--compatibility)
- [🧩 Additional Message Options](#-additional-message-options)
- [📥 Installation](#-installation)
   - [🧩 Import (ESM & CJS)](#-import-esm--cjs)
- [🌐 Connect to WhatsApp (Quick Step)](#-connect-to-whatsapp-quick-step)
   - [🔐 Auth State](#-auth-state)
- [🗄️ Implementing Data Store](#%EF%B8%8F-implementing-data-store)
- [🪪 WhatsApp IDs Explain](#-whatsapp-ids-explain)
- [✉️ Sending Messages](#%EF%B8%8F-sending-messages)
   - [🔠 Text](#-text)
   - [🔔 Mention](#-mention)
   - [😁 Reaction](#-reaction)
   - [📌 Pin Message](#-pin-message)
   - [🔖 Keep Chat](#-keep-chat)
   - [➡️ Forward Message](#%EF%B8%8F-forward-message)
   - [👤 Contact](#-contact)
   - [📍 Location](#-location)
   - [🗓️ Event](#%EF%B8%8F-event)
   - [👥 Group Invite](#-group-invite)
   - [🛍️ Product](#%EF%B8%8F-product)
   - [📊 Poll](#-poll)
   - [💭 Button Response](#-button-response)
   - [✨ Rich Response](#-rich-response)
   - [🧾 Message with Code Block](#-message-with-code-block)
   - [🌏 Message with Inline Entities](#-message-with-inline-entities)
   - [📋 Message with Table](#-message-with-table)
   - [🎞️ Status Mention](#%EF%B8%8F-status-mention)
- [📁 Sending Media Messages](#-sending-media-messages)
   - [🖼️ Image](#%EF%B8%8F-image)
   - [🎥 Video](#-video)
   - [📃 Sticker](#-sticker)
   - [💽 Audio](#-audio)
   - [🗂️ Document](#%EF%B8%8F-document)
   - [🖼️ Album (Image & Video)](#%EF%B8%8F-album-image--video)
   - [📦 Sticker Pack](#-sticker-pack)
- [👉🏻 Sending Interactive Messages](#-sending-interactive-messages)
   - [🔘 Buttons](#-buttons)
   - [📋 List](#-list)
   - [🗄️ Interactive](#%EF%B8%8F-interactive)
   - [🫙 Hydrated Template](#-hydrated-template)
- [💳 Sending Payment Messages](#-sending-payment-messages)
   - [➕ Invite Payment](#-invite-payment)
   - [🧾 Invoice](#-invoice)
   - [🛍️ Order](#%EF%B8%8F-order)
   - [💳 Request Payment](#-request-payment)
- [👁️ Other Message Options](#%EF%B8%8F-other-message-options)
   - [🤖 AI Icon](#-ai-icon)
   - [🕒 Ephemeral](#-ephemeral)
   - [📰 External Ad Reply](#-external-ad-reply)
   - [🧑‍🧑‍🧒 Group Status](#%E2%80%8D%E2%80%8D-group-status)
   - [🐱 Lottie Sticker](#-lottie-sticker)
   - [🧩 Raw](#-raw)
   - [🏷️ Secure Meta Service Label](#%EF%B8%8F-secure-meta-service-label)
   - [📑 Spoiler](#-spoiler)
   - [👁️ View Once](#%EF%B8%8F-view-once)
   - [👁️ View Once V2](#%EF%B8%8F-view-once-v2)
   - [👁️ View Once V2 Extension](#%EF%B8%8F-view-once-v2-extension)
- [♻️ Modify Messages](#%EF%B8%8F-modify-messages)
   - [🗑️ Delete Messages](#%EF%B8%8F-delete-messages)
   - [✏️ Edit Messages](#%EF%B8%8F-edit-messages)
- [🧰 Additional Contents](#-additional-contents)
   - [🏷️ Find User ID (JID|PN/LID)](#%EF%B8%8F-find-user-id-jidpnlid)
   - [🔑 Request Custom Pairing Code](#-request-custom-pairing-code)
   - [🖼️ Image Processing](#%EF%B8%8F-image-processing)
   - [📣 Newsletter Management](#-newsletter-management)
   - [👥 Group Management](#-group-management)
   - [👥 Community Management](#-community-management)
   - [👤 Profile Management](#-profile-management)
   - [🛒 Business Management](#-business-management)
   - [🔐 Privacy Management](#-privacy-management)
   - [📡 Events](#-events)
- [🚀 Try the Bot](#-try-the-bot)
- [📦 Fork Base](#-fork-base)
- [📣 Credits](#-credits)

### 🛠️ Internal Adjustments
- 🖼️ Fixed an issue where media could not be sent to newsletters due to an upstream issue.
- 📁 Reintroduced [`makeInMemoryStore`](#%EF%B8%8F-implementing-data-store) with a minimal ESM adaptation and small adjustments for Baileys v7.
- 📦 Switched FFmpeg execution from `exec` to `spawn` for safer process handling.
- 🗃️ Added [`@napi-rs/image`](https://www.npmjs.com/package/@napi-rs/image) as a supported image processing backend in [`getImageProcessingLibrary()`](#%EF%B8%8F-image-processing), offering a balance between performance and compatibility.

### 📨 Messages Handling & Compatibility
- 📩 Expanded messages support for:
   - 🖼️ [Album Message](#%EF%B8%8F-album-image--video)
   - 👤 [Group Status Message](#%E2%80%8D%E2%80%8D-group-status)
   - 👉🏻 [Interactive Message](#-sending-interactive-messages) (buttons, lists, native flows, templates, carousels).
   - 🎞️ [Status Mention Message](#%EF%B8%8F-status-mention)
   - 📦 [Sticker Pack Message](#-sticker-pack)
   - ✨ [Rich Response Message](#-rich-response) **[NEW]**
   - 🧾 [Message with Code Blocks](#-message-with-code-block) **[NEW]**
   - [🌏 Message with Inline Entities](#-message-with-inline-entities) **[NEW]**
   - 📋 [Message with Table](#-message-with-table) **[NEW]**
   - 💳 [Payment-related Message](#-sending-payment-messages) (payment requests, invites, orders, invoices).
- 📰 Simplified sending messages with ad thumbnail using [`externalAdReply`](#-external-ad-reply), without requiring manual `contextInfo`.
- 💭 Added support for quoting messages inside channel (newsletter). **[NEW]**
- 🎀 Added support for [custom button icon](#%EF%B8%8F-interactive). **[NEW]**

### 🧩 Additional Message Options
- 👁️ Added optional boolean flags for message handling:  
   - 🤖 [`ai`](#-ai-icon) - AI icon on message
   - 📣 [`mentionAll`](#-mention) - Mention all group participants without requiring their JIDs in `mentions` or `mentionedJid` **[NEW]**
   - 🔧 [`ephemeral`](#-ephemeral), [`groupStatus`](#%E2%80%8D%E2%80%8D-group-status), [`isLottie`](#-lottie-sticker), [`spoiler`](#-spoiler), [`viewOnce`](#%EF%B8%8F-view-once), [`viewOnceV2`](#%EF%B8%8F-view-once-v2), [`viewOnceV2Extension`](#%EF%B8%8F-view-once-v2-extension), [`interactiveAsTemplate`](#%EF%B8%8F-interactive) - Message wrappers
   - 🔒 [`secureMetaServiceLabel`](#%EF%B8%8F-secure-meta-service-label) - Secure meta service label on message **[NEW]**
   - 📄 [`raw`](#-raw) - Build your message manually **(DO NOT USE FOR EXPLOITATION)**

### 📥 Installation

- 📄 Via `package.json`

```json
# NPM
"dependencies": {
   "@pathumtech/baileys": "latest"
}

# GitHub
"dependencies": {
   "@pathumtech/baileys": "github:pathumtech/baileys"
}
