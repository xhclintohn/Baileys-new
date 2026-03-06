# Baileys-MOD (Super Baileys) ⭐

<div align="center">

[![npm version](https://img.shields.io/npm/v/toxic-baileys.svg?style=for-the-badge)](https://www.npmjs.com/package/toxic-baileys)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)
[![npm downloads](https://img.shields.io/npm/dm/toxic-baileys.svg?style=for-the-badge)](https://www.npmjs.com/package/toxic-baileys)
[![GitHub](https://img.shields.io/badge/GitHub-Repository-blue?style=for-the-badge&logo=github)](https://github.com/xhclintohn/Baileys)

</div>

A professionally enhanced, feature-rich version of the Baileys WhatsApp Web API. Built for developers requiring robust WhatsApp automation with modern tooling and comprehensive documentation.

**Maintainer:** 𝐱𝐡_𝐜𝐥𝐢𝐧𝐭𝐨𝐧 [Dev]

---

## 📋 Table of Contents

- [✨ Features](#✨-features)

- [📦 Installation](#📦-installation)

- [🚀 Quick Start](#🚀-quick-start)

- [🔌 Connection & Configuration](#🔌-connection--configuration)

- [💾 Authentication State Management](#💾-authentication-state-management)

- [📤 Sending Messages](#📤-sending-messages)

- [📁 Chat & Message Management](#📁-chat--message-management)

- [👥 Group Management](#👥-group-management)
  - [Group Operations](#group-operations)
  - [Group Status](#group-status-management)

- [👤 User & Profile Management](#👤-user--profile-management)

- [🔒 Privacy & Block Management](#🔒-privacy--block-management)

- [🗄️ Data Store Implementation](#🗄️-data-store-implementation)

- [🛠️ Utility Functions](#🛠️-utility-functions)
- [💡 Best Practices & Tips](#💡-best-practices--tips)
- 
- [⚠️ Important Legal Notice](#⚠️-important-legal-notice)
- 
- [🆘 Getting Help](#🆘-getting-help)
- 
- [📄 License](#📄-license)
- 
- [🤝 Contributing](#🤝-contributing)

---

<h2 align="center">✨ Features</h2>

- 🚀 **Modern & Fast** – Built with TypeScript and latest technologies
- 
- 🔧 **Enhanced Stability** – Improved connection handling and error recovery
- 
- 📱 **Multi-Device Support** – Full support for WhatsApp's multi-device protocol
- 
- 🔐 **End-to-End Encryption** – Secure communication using Signal Protocol
- 
- 📨 **All Message Types** – Support for text, media, documents, contacts, locations, polls, and more
- 
- 👥 **Advanced Group Management** – Comprehensive group controls and utilities, including group status support
- 
- 💾 **Flexible Authentication** – Multiple auth state storage options
- 
- 🛠️ **Developer Friendly** – Clean API, extensive examples, and detailed documentation

---

<h2 align="center">📦 Installation</h2>

Choose the installation method that best fits your workflow:

### **Method 1: Package Aliasing (Advanced)**
Use npm aliasing to replace the original package with this enhanced version:

#### Using Git (Recomended)
```json
// In your package.json
"dependencies": {
    "@whiskeysockets/baileys": "github:xhclintohn/Baileys.git"
}
```

#### Using NPM
```json
// In your package.json
"dependencies": {
    "@whiskeysockets/baileys": "npm:toxic-baileys@latest"
}
```

Then run `npm install`.

**Note:** This will make your imports of `@whiskeysockets/baileys` resolve to this enhanced package.

### **Method 2: Via npm (Recommended for Production)**
```bash
npm install toxic-baileys
```

### **Method 3: Directly from GitHub**
Install the latest development version directly from the source repository:

```bash
# Install main branch
npm install https://github.com/xhclintohn/Baileys.git
```

### **Method 4: Local Development Setup**
For contributing or local testing, install from a local directory:

```bash
# Clone the repository first
git clone https://github.com/xhclintohn/Baileys.git
cd your-project

# Install as local dependency
npm install /path/to/cloned/Baileys
```

### **Method 5: Using Yarn**
```bash
yarn add toxic-baileys
# or from GitHub
yarn add https://github.com/xhclintohn/Baileys.git
```

---

<h2 align="center">🚀 Quick Start</h2>

### Basic Connection Example
<details>
<summary>Click to expand code</summary>

```javascript
const { makeWASocket, useMultiFileAuthState, DisconnectReason } = require('@whiskeysockets/baileys');
const { Boom } = require('@hapi/boom');

async function connectToWhatsApp() {
    const { state, saveCreds } = await useMultiFileAuthState('auth_info_baileys');
    
    const sock = makeWASocket({
        auth: state,
        printQRInTerminal: true,
        browser: ["Ubuntu", "Chrome", "125"],
        logger: console, // Optional: Enable detailed logging
    });

    sock.ev.on('connection.update', (update) => {
        const { connection, lastDisconnect } = update;
        if (connection === 'close') {
            const shouldReconnect = (lastDisconnect?.error)?.output?.statusCode !== DisconnectReason.loggedOut;
            console.log('Connection closed due to ', lastDisconnect.error, ', reconnecting ', shouldReconnect);
            if (shouldReconnect) {
                connectToWhatsApp();
            }
        } else if (connection === 'open') {
            console.log('✅ Successfully connected to WhatsApp!');
            // Send a welcome message to yourself
            const selfJid = sock.user.id;
            sock.sendMessage(selfJid, { text: 'Hello! I am online using toxic-baileys 🤖' });
        }
    });

    sock.ev.on('messages.upsert', async ({ messages }) => {
        for (const m of messages) {
            if (!m.message) continue;
            console.log('📱 New message received:', JSON.stringify(m, undefined, 2));
            
            // Example: Auto-reply to messages
            if (m.message.conversation) {
                await sock.sendMessage(m.key.remoteJid, { 
                    text: `You said: "${m.message.conversation}"` 
                });
            }
        }
    });

    sock.ev.on('creds.update', saveCreds);
}

connectToWhatsApp().catch(console.error);
```
</details>

### Connect with Pairing Code (Alternative to QR)
<details>
<summary>Click to expand code</summary>

```javascript
const { makeWASocket } = require('@whiskeysockets/baileys');

const sock = makeWASocket({ printQRInTerminal: false });

// Request a pairing code for a phone number
if (!sock.authState.creds.registered) {
    const phoneNumber = '1234567890'; // Include country code, no symbols
    const pairingCode = await sock.requestPairingCode(phoneNumber);
    console.log('🔑 Your Pairing Code:', pairingCode);
    
    // For custom pairing codes (must be 8 characters):
    // const customPairingCode = await sock.requestPairingCode(phoneNumber, 'MYCODE123');
}
```
</details>

---

<h2 align="center">🔌 Connection & Configuration</h2>

### Browser Configuration Constants
<details>
<summary>Click to expand code</summary>

```javascript
const { makeWASocket, Browsers } = require('@whiskeysockets/baileys');

// Pre-defined browser configurations
const sock = makeWASocket({
    browser: Browsers.ubuntu('MyApp'),   // Ubuntu/Chrome
    // browser: Browsers.macOS('MyApp'), // macOS/Safari
    // browser: Browsers.windows('MyApp'), // Windows/Edge
    printQRInTerminal: true,
    syncFullHistory: true, // Receive full chat history
});
```
</details>

### Important Socket Configuration Notes
<details>
<summary>Click to expand code</summary>

```javascript
const NodeCache = require('node-cache');
const groupCache = new NodeCache({ stdTTL: 300, useClones: false });

const sock = makeWASocket({
    // Cache group metadata for better performance
    cachedGroupMetadata: async (jid) => groupCache.get(jid),
    
    // Custom message store retrieval
    getMessage: async (key) => {
        // Implement your own message store logic here
        return await yourDatabase.getMessage(key);
    },
    
    // Disable online presence to receive notifications in mobile app
    markOnlineOnConnect: false,
    
    // Connection timeouts
    connectTimeoutMs: 60000,
    defaultQueryTimeoutMs: 60000,
});

// Update cache when groups change
sock.ev.on('groups.update', async ([event]) => {
    const metadata = await sock.groupMetadata(event.id);
    groupCache.set(event.id, metadata);
});
```
</details>

---

<h2 align="center">💾 Authentication State Management</h2>

### Multi-File Auth State (Recommended for Development)
<details>
<summary>Click to expand code</summary>

```javascript
const { makeWASocket, useMultiFileAuthState } = require('@whiskeysockets/baileys');

async function connect() {
    const { state, saveCreds } = await useMultiFileAuthState('auth_info_directory');
    const sock = makeWASocket({ auth: state });
    sock.ev.on('creds.update', saveCreds);
}
```
</details>

### Custom Database Auth State (Production)
<details>
<summary>Click to expand code</summary>

```javascript
const { makeWASocket, makeCacheableSignalKeyStore } = require('@whiskeysockets/baileys');

// Example with custom database storage
const myAuthState = {
    creds: await yourDatabase.getAuthCreds(),
    keys: makeCacheableSignalKeyStore(
        await yourDatabase.getSignalKeys(),
        console.log // Optional logger
    )
};

const sock = makeWASocket({ auth: myAuthState });

// Update your database when credentials change
sock.ev.on('creds.update', async (creds) => {
    await yourDatabase.saveAuthCreds(creds);
});
```
</details>

---

<h2 align="center">📤 Sending Messages</h2>

### Basic Message Types
<details>
<summary>Click to expand code</summary>

```javascript
// Text message
await sock.sendMessage(jid, { text: 'Hello World!' });

// Text with mentions
await sock.sendMessage(jid, {
    text: 'Hello @12345678901!',
    mentions: ['12345678901@s.whatsapp.net']
});

// Reply to a message
await sock.sendMessage(jid, { text: 'This is a reply!' }, { quoted: originalMessage });

// Forward a message
await sock.sendMessage(jid, { forward: messageToForward });
```
</details>

### Media Messages
<details>
<summary>Click to expand code</summary>

```javascript
// Image with caption
await sock.sendMessage(jid, {
    image: { url: './path/to/image.jpg' }, // Can also use Buffer or stream
    caption: 'Check out this image!',
    mimetype: 'image/jpeg'
});

// Video
await sock.sendMessage(jid, {
    video: { url: './path/to/video.mp4' },
    caption: 'Watch this video',
    gifPlayback: true // For GIF-like playback
});

// Audio/voice note
await sock.sendMessage(jid, {
    audio: { url: './path/to/audio.ogg' },
    mimetype: 'audio/ogg; codecs=opus',
    ptt: true // For voice note
});

// Document
await sock.sendMessage(jid, {
    document: { url: './path/to/document.pdf' },
    fileName: 'ImportantDocument.pdf',
    mimetype: 'application/pdf'
});
```
</details>

### Interactive Messages
<details>
<summary>Click to expand code</summary>

```javascript
// Location
await sock.sendMessage(jid, {
    location: {
        degreesLatitude: 40.7128,
        degreesLongitude: -74.0060,
        name: 'New York City'
    }
});

// Contact card
const vcard = `BEGIN:VCARD
VERSION:3.0
FN:John Doe
ORG:Example Corp;
TEL;type=CELL;type=VOICE;waid=1234567890:+1 234 567 890
EMAIL:john@example.com
END:VCARD`;

await sock.sendMessage(jid, {
    contacts: {
        displayName: 'John Doe',
        contacts: [{ vcard }]
    }
});

// Poll
await sock.sendMessage(jid, {
    poll: {
        name: 'Favorite Programming Language?',
        values: ['JavaScript', 'Python', 'TypeScript', 'Go', 'Rust'],
        selectableCount: 1
    }
});

// Reaction
await sock.sendMessage(jid, {
    react: {
        text: '👍', // Empty string to remove reaction
        key: targetMessage.key
    }
});
```
</details>

---

<h2 align="center">📁 Chat & Message Management</h2>

### Chat Operations
<details>
<summary>Click to expand code</summary>

```javascript
// Archive/unarchive chat
await sock.chatModify({ archive: true }, jid);
await sock.chatModify({ archive: false }, jid);

// Mute/unmute chat (8 hours example)
await sock.chatModify({ mute: 8 * 60 * 60 * 1000 }, jid);
await sock.chatModify({ mute: null }, jid);

// Pin/unpin chat
await sock.chatModify({ pin: true }, jid);
await sock.chatModify({ pin: false }, jid);

// Mark as read/unread
await sock.chatModify({ markRead: true }, jid);
await sock.chatModify({ markRead: false }, jid);

// Delete chat
await sock.chatModify({ delete: true }, jid);
```
</details>

### Message Operations
<details>
<summary>Click to expand code</summary>

```javascript
// Delete message for everyone
const sentMsg = await sock.sendMessage(jid, { text: 'This will be deleted' });
await sock.sendMessage(jid, { delete: sentMsg.key });

// Edit message
const response = await sock.sendMessage(jid, { text: 'Original text' });
await sock.sendMessage(jid, { 
    text: 'Updated text!',
    edit: response.key
});

// Read messages
await sock.readMessages([messageKey1, messageKey2]);

// Download media from message
const { downloadMediaMessage } = require('@whiskeysockets/baileys');
const fs = require('fs');

sock.ev.on('messages.upsert', async ({ messages }) => {
    const m = messages[0];
    if (m.message?.imageMessage) {
        const stream = await downloadMediaMessage(
            m, 'stream', {}, { logger: console }
        );
        const file = fs.createWriteStream('./download.jpg');
        stream.pipe(file);
    }
});
```
</details>

---

<h2 align="center">👥 Group Management</h2>

### Group Operations
<details>
<summary>Click to expand code</summary>

```javascript
// Create group
const group = await sock.groupCreate('My New Group', [
    '12345678901@s.whatsapp.net',
    '09876543210@s.whatsapp.net'
]);

// Add/remove participants
await sock.groupParticipantsUpdate(
    groupJid,
    ['12345678901@s.whatsapp.net'],
    'add' // 'remove', 'promote', or 'demote'
);

// Update group info
await sock.groupUpdateSubject(groupJid, 'New Group Name');
await sock.groupUpdateDescription(groupJid, 'New group description');

// Group settings
await sock.groupSettingUpdate(groupJid, 'announcement'); // Only admins can send
await sock.groupSettingUpdate(groupJid, 'not_announcement'); // Everyone can send
await sock.groupSettingUpdate(groupJid, 'locked'); // Only admins can change settings
await sock.groupSettingUpdate(groupJid, 'unlocked'); // Everyone can change settings

// Invite links
const inviteCode = await sock.groupInviteCode(groupJid);
const inviteLink = `https://chat.whatsapp.com/${inviteCode}`;

// Join group via invite
const groupJid = await sock.groupAcceptInvite('INVITECODEHERE');

// Get group metadata
const metadata = await sock.groupMetadata(groupJid);
console.log(`Group: ${metadata.subject}, Participants: ${metadata.participants.length}`);

// Leave group
await sock.groupLeave(groupJid);
```
</details>

### Group Status Management
<details>
<summary>Click to expand code</summary>

```javascript
module.exports = {
  name: 'gstatus',
  aliases: ['groupstatus', 'gs'],
  description: 'Posts a group status with text, image, video, or audio.',
  run: async (context) => {
    const { client, m, prefix, isBotAdmin, IsGroup, botname } = context;

    const formatMsg = (text) => `◈━━━━━━━━━━━━━━━◈\n${text}\n◈━━━━━━━━━━━━━━━◈`;

    try {
      if (!botname) {
        return client.sendText(m.chat, formatMsg(`Bot name is not set. Please configure it before using this command.`), m);
      }

      if (!m.sender || typeof m.sender !== 'string' || !m.sender.includes('@s.whatsapp.net')) {
        return client.sendText(m.chat, formatMsg(`Could not identify your WhatsApp ID. Please try again.`), m);
      }

      if (!IsGroup) {
        return client.sendText(m.chat, formatMsg(`This command can only be used in group chats.`), m);
      }

      if (!isBotAdmin) {
        return client.sendText(m.chat, formatMsg(`I need to be *admin* to post a group status. Please make me admin first.`), m);
      }

      const settings = await getSettings();
      if (!settings) {
        return client.sendText(m.chat, formatMsg(`Failed to load settings. Please try again later.`), m);
      }

      const quoted = m.quoted ? m.quoted : m;
      const mime = (quoted.msg || quoted).mimetype || '';
      const caption = m.body
        .replace(new RegExp(`^${prefix}(gstatus|groupstatus|gs)\\s*`, 'i'), '')
        .trim();

      if (!/image|video|audio/.test(mime) && !caption) {
        return client.sendText(
          m.chat,
          formatMsg(`Please reply to an image, video, audio, or include text with the command.\nExample: ${prefix}gstatus Check out this update!`),
          m
        );
      }

      const defaultCaption = `Group status Posted By Toxic-MD✅\n\nJOIN\nhttps://chat.whatsapp.com/GoXKLVJgTAAC3556FXkfFI?mode=wwt`;

      if (/image/.test(mime)) {
        const buffer = await client.downloadMediaMessage(quoted);
        await client.sendMessage(m.chat, {
          groupStatusMessage: {
            image: buffer,
            caption: caption || defaultCaption
          }
        });
        await client.sendText(m.chat, formatMsg(`Image status has been posted successfully ✅`), m);
      } else if (/video/.test(mime)) {
        const buffer = await client.downloadMediaMessage(quoted);
        await client.sendMessage(m.chat, {
          groupStatusMessage: {
            video: buffer,
            caption: caption || defaultCaption
          }
        });
        await client.sendText(m.chat, formatMsg(`Video status has been posted successfully ✅`), m);
      } else if (/audio/.test(mime)) {
        const buffer = await client.downloadMediaMessage(quoted);
        await client.sendMessage(m.chat, {
          groupStatusMessage: {
            audio: buffer,
            mimetype: 'audio/mp4'
          }
        });
        await client.sendText(m.chat, formatMsg(`Audio status has been posted successfully ✅`), m);
      } else if (caption) {
        await client.sendMessage(m.chat, {
          groupStatusMessage: { text: caption }
        });
        await client.sendText(m.chat, formatMsg(`Text status has been posted successfully ✅`), m);
      }

    } catch (error) {
      await client.sendText(
        m.chat,
        formatMsg(`An error occurred while posting status:\n${error.message}`),
        m
      );
    }
  }
};
```
</details>

---

<h2 align="center">👤 User & Profile Management</h2>

### User Operations
<details>
<summary>Click to expand code</summary>

```javascript
// Check if user exists on WhatsApp
const [result] = await sock.onWhatsApp('12345678901@s.whatsapp.net');
if (result.exists) {
    console.log(`User exists with JID: ${result.jid}`);
}

// Get profile picture
const ppUrl = await sock.profilePictureUrl('12345678901@s.whatsapp.net');
const ppUrlHighRes = await sock.profilePictureUrl('12345678901@s.whatsapp.net', 'image');

// Get user status
const status = await sock.fetchStatus('12345678901@s.whatsapp.net');

// Subscribe to presence updates
sock.ev.on('presence.update', ({ id, presences }) => {
    console.log(`${id} presence:`, presences);
});
await sock.presenceSubscribe('12345678901@s.whatsapp.net');

// Business profile
const businessProfile = await sock.getBusinessProfile('12345678901@s.whatsapp.net');
```
</details>

### Profile Operations
<details>
<summary>Click to expand code</summary>

```javascript
// Update your own profile
await sock.updateProfileName('Your New Name');
await sock.updateProfileStatus('Online and coding! 🚀');

// Update profile picture
await sock.updateProfilePicture(
    '12345678901@s.whatsapp.net', 
    fs.readFileSync('./new-avatar.jpg')
);

// Remove profile picture
await sock.removeProfilePicture('12345678901@s.whatsapp.net');
```
</details>

---

<h2 align="center">🔒 Privacy & Block Management</h2>

### Privacy Settings
<details>
<summary>Click to expand code</summary>

```javascript
// Update various privacy settings
await sock.updateLastSeenPrivacy('contacts'); // 'all', 'contacts', 'contact_blacklist', 'none'
await sock.updateOnlinePrivacy('match_last_seen'); // 'all', 'match_last_seen'
await sock.updateProfilePicturePrivacy('contacts');
await sock.updateStatusPrivacy('contacts');
await sock.updateReadReceiptsPrivacy('all'); // 'all', 'none'
await sock.updateGroupsAddPrivacy('contacts');

// Get current privacy settings
const privacySettings = await sock.fetchPrivacySettings(true);
```
</details>

### Block Management
<details>
<summary>Click to expand code</summary>

```javascript
// Block/unblock user
await sock.updateBlockStatus(jid, 'block');
await sock.updateBlockStatus(jid, 'unblock');

// Get blocklist
const blocklist = await sock.fetchBlocklist();
```
</details>

---

<h2 align="center">🗄️ Data Store Implementation</h2>

### In-Memory Store (Development)
<details>
<summary>Click to expand code</summary>

```javascript
const { makeInMemoryStore } = require('@whiskeysockets/baileys');
const store = makeInMemoryStore({ logger: console });

// Read from/write to file
store.readFromFile('./baileys_store.json');
setInterval(() => {
    store.writeToFile('./baileys_store.json');
}, 10_000);

const sock = makeWASocket({ });
store.bind(sock.ev);

// Use store data
sock.ev.on('chats.upsert', () => {
    console.log('Chats in store:', store.chats.all());
});
```
</details>

### Custom Store (Production)
<details>
<summary>Click to expand code</summary>

```javascript
// Implement your own store using databases like MongoDB, PostgreSQL, etc.
class MyCustomStore {
    async saveMessage(key, message) {
        await yourDatabase.messages.insertOne({ key, message });
    }
    
    async loadMessage(key) {
        return await yourDatabase.messages.findOne({ key });
    }
    
    // Implement other store methods...
}

const myStore = new MyCustomStore();
```
</details>

---

<h2 align="center">🛠️ Utility Functions</h2>

### Core Utilities
<details>
<summary>Click to expand code</summary>

```javascript
const {
    getContentType,
    areJidsSameUser,
    isJidGroup,
    isJidBroadcast,
    isJidStatusBroadcast,
    jidNormalizedUser,
    generateMessageID,
    generateWAMessageContent,
    downloadContentFromMessage,
    getAggregateVotesInPollMessage,
    proto
} = require('@whiskeysockets/baileys');

// Message type detection
const messageType = getContentType(message);

// JID validation
if (isJidGroup(jid)) {
    console.log('This is a group JID');
}

// Download content
const stream = await downloadContentFromMessage(message.imageMessage, 'image');
```
</details>

### Event Debugging
<details>
<summary>Click to expand code</summary>

```javascript
// Enable debug logging
const sock = makeWASocket({
    logger: console, // Simple logging
    // logger: P({ level: 'debug' }), // Detailed logging with pino
});

// Custom WebSocket event handling
sock.ws.on('CB:edge_routing', (node) => {
    console.log('Edge routing event:', node);
});
```
</details>

---

<h2 align="center">💡 Best Practices & Tips</h2>

### Performance Optimization
1. Cache Group Metadata: Always cache group metadata to reduce API calls
2. Implement Proper Store: Use databases instead of in-memory stores for production
3. Handle Reconnections: Implement robust reconnection logic
4. Rate Limiting: Respect WhatsApp's rate limits to avoid bans
5. Error Handling: Wrap socket calls in try-catch blocks

### Security Considerations
1. Never Hardcode Credentials: Use environment variables
2. Secure Auth Storage: Encrypt authentication data in databases
3. Input Validation: Validate all inputs before processing
4. Regular Updates: Keep the package updated for security fixes

### Common Patterns
<details>
<summary>Click to expand code</summary>

```javascript
// Auto-reconnect pattern
async function connectWithRetry(maxRetries = 5) {
    for (let i = 0; i < maxRetries; i++) {
        try {
            await connectToWhatsApp();
            break;
        } catch (error) {
            console.log(`Connection attempt ${i + 1} failed:`, error.message);
            if (i === maxRetries - 1) throw error;
            await new Promise(resolve => setTimeout(resolve, 5000));
        }
    }
}

// Message processing queue
class MessageQueue {
    constructor(sock) {
        this.sock = sock;
        this.queue = [];
        this.processing = false;
    }
    
    async add(message) {
        this.queue.push(message);
        if (!this.processing) this.process();
    }
    
    async process() {
        this.processing = true;
        while (this.queue.length > 0) {
            const message = this.queue.shift();
            try {
                await this.sock.sendMessage(message.jid, message.content);
                await new Promise(resolve => setTimeout(resolve, 1000)); // Rate limiting
            } catch (error) {
                console.error('Failed to send message:', error);
            }
        }
        this.processing = false;
    }
}
```
</details>

---

<h2 align="center">⚠️ Important Legal Notice</h2>

**Disclaimer:** This project is NOT affiliated with, authorized, maintained, sponsored, or endorsed by WhatsApp LLC or any of its affiliates or subsidiaries.

The official WhatsApp website can be found at https://whatsapp.com. "WhatsApp" as well as related names, marks, emblems, and images are registered trademarks of their respective owners.

### Terms of Service Compliance
- This library is meant for legitimate automation purposes only
- Do NOT use for spamming, bulk messaging, or harassment
- Respect WhatsApp's rate limits and terms of service
- Users are solely responsible for how they use this tool
- The maintainer assumes NO liability for misuse or damages

### Responsible Use Guidelines
1. Obtain Consent: Only message users who have explicitly consented
2. Respect Privacy: Do not collect or misuse personal data
3. Limit Frequency: Avoid excessive messaging that could be considered spam
4. Provide Value: Ensure your automation provides genuine utility
5. Monitor Usage: Regularly audit your usage patterns

---

<h2 align="center">🆘 Getting Help</h2>

1. **Documentation:** First, check this README and the official Baileys documentation
2. **GitHub Issues:** For bug reports and feature requests, please use the GitHub Issues page
3. **Contact Directly:** For urgent matters or specific questions, reach out via WhatsApp: +254735342808
4. **Community:** Consider joining relevant developer communities for broader support

**Response Time:** The developer typically responds within 24-48 hours on business days.

---

<h2 align="center">📄 License</h2>

Distributed under the MIT License. See the LICENSE file in the repository for more information.

### Third-Party Credits
This project is based on the excellent work of:
- Baileys by WhiskeySockets
- Signal Protocol implementations
- Various open-source contributors

---

<h2 align="center">🤝 Contributing</h2>

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

1. Fork the repository
2. Create your feature branch (git checkout -b feature/AmazingFeature)
3. Commit your changes (git commit -m 'Add some AmazingFeature')
4. Push to the branch (git push origin feature/AmazingFeature)
5. Open a Pull Request

<div align="center">

🌟 Crafted with ❤️ by 𝐱𝐡_𝐜𝐥𝐢𝐧𝐭𝐨𝐧 [Dev]

Empowering developers with powerful WhatsApp automation tools

If this project helped you, consider starring the repository! ⭐

</div>