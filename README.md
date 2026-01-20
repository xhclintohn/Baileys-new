# WhatsApp Baileys

<p align="center">
  <img src="https://raw.githubusercontent.com/z4phdev/client/refs/heads/main/thumbnail.png" alt="Thumbnail" />
</p>

WhatsApp Baileys is an open-source library designed to help developers build automation solutions and integrations with WhatsApp efficiently and directly. Using websocket technology without the need for a browser, this library supports a wide range of features such as message management, chat handling, group administration, as well as interactive messages and action buttons for a more dynamic user experience.

Actively developed and maintained, baileys continuously receives updates to enhance stability and performance. One of the main focuses is to improve the pairing and authentication processes to be more stable and secure. Pairing features can be customized with your own codes, making the process more reliable and less prone to interruptions.

This library is highly suitable for building business bots, chat automation systems, customer service solutions, and various other communication automation applications that require high stability and comprehensive features. With a lightweight and modular design, baileys is easy to integrate into different systems and platforms.

---

### Main Features and Advantages

- Supports automatic and custom pairing processes
- Fixes previous pairing issues that often caused failures or disconnections
- Supports interactive messages, action buttons, and dynamic menus
- Efficient automatic session management for reliable operation
- Compatible with the latest multi-device features from WhatsApp
- Lightweight, stable, and easy to integrate into various systems
- Suitable for developing bots, automation, and complete communication solutions
- Comprehensive documentation and example codes to facilitate development

---

## Getting Started

Begin by installing the library via your preferred package manager, then follow the provided configuration guide. You can also utilize the ready-made example codes to understand how the features work. Use session storage and interactive messaging features to build complete, stable solutions tailored to your business or project needs.

---

## Add Function ( Simple code )

### Label Group
Tag/Label Member Grop

```javascript
sock.setLabelGroup(jid, string)
```
---
### Check ID Channel 
Get ID Channel From Url

```javascript
sock.newsletterFromUrl(url)
```
Result JSON
```json
{
  "name": "Name Channel",
  "id": "Channel ID",
  "state": "Status Channel",
  "subscribers": "Followers",
  "verification": "UNVERIFIED",
  "creation_time": 1728547155,
  "description": "Description Channel"
}
```
---
### Check banned number
You can see the status of blocked numbers here 

```javascript
sock.checkWhatsApp(jid)
```
---

## SendMessage Documentation

### Status Mention Group & Private Message
Send Status Mention Group/Private Chat

```javascript
await sock.sendStatusMention(content, jid);
```

### Status Group Message V2
Send Group Status With Version 2 

```javascript
await sock.sendMessage(jid, {
     groupStatusMessage: {
          text: "Hello World"
     }
});
```

### Album Message (Multiple Images)
Send multiple images in a single album message:

```javascript
await sock.sendMessage(jid, { 
    albumMessage: [
        { image: cihuy, caption: "Foto pertama" },
        { image: { url: "URL IMAGE" }, caption: "Foto kedua" }
    ] 
}, { quoted: m });
```

### Event Message
Create and send WhatsApp event invitations:

```javascript
await sock.sendMessage(jid, { 
    eventMessage: { 
        isCanceled: false, 
        name: "Hello World", 
        description: "z4phdev", 
        location: { 
            degreesLatitude: 0, 
            degreesLongitude: 0, 
            name: "rowrrrr" 
        }, 
        joinLink: "https://call.whatsapp.com/video/saweitt", 
        startTime: "1763019000", 
        endTime: "1763026200", 
        extraGuestsAllowed: false 
    } 
}, { quoted: m });
```

### Poll Result Message
Display poll results with vote counts:

```javascript
await sock.sendMessage(jid, { 
    pollResultMessage: { 
        name: "Hello World", 
        pollVotes: [
            {
                optionName: "TEST 1",
                optionVoteCount: "112233"
            },
            {
                optionName: "TEST 2",
                optionVoteCount: "1"
            }
        ] 
    } 
}, { quoted: m });
```

### Simple Interactive Message
Send basic interactive messages with copy button functionality:

```javascript
await sock.sendMessage(jid, {
    interactiveMessage: {
        header: "Hello World",
        title: "Hello World",
        footer: "telegram: @saweitt ",
        buttons: [
            {
                name: "cta_copy",
                buttonParamsJson: JSON.stringify({
                    display_text: "copy code",
                    id: "123456789",              
                    copy_code: "ABC123XYZ"
                })
            }
        ]
    }
}, { quoted: m });
```

### Interactive Message with Native Flow
Send interactive messages with buttons, copy actions, and native flow features:

```javascript
await sock.sendMessage(jid, {    
    interactiveMessage: {      
        header: "Hello World",
        title: "Hello World",      
        footer: "telegram: @saweitt",      
        image: { url: "https://example.com/image.jpg" },      
        nativeFlowMessage: {        
            messageParamsJson: JSON.stringify({          
                limited_time_offer: {            
                    text: "idk hummmm?",            
                    url: "https://t.me/saweitt",            
                    copy_code: "z4phdev",            
                    expiration_time: Date.now() * 999          
                },          
                bottom_sheet: {            
                    in_thread_buttons_limit: 2,            
                    divider_indices: [1, 2, 3, 4, 5, 999],            
                    list_title: "z4phdev",            
                    button_title: "z4phdev"          
                },          
                tap_target_configuration: {            
                    title: " X ",            
                    description: "bomboclard",            
                    canonical_url: "https://t.me/saweitt",            
                    domain: "shop.example.com",            
                    button_index: 0          
                }        
            }),        
            buttons: [          
                {            
                    name: "single_select",            
                    buttonParamsJson: JSON.stringify({              
                        has_multiple_buttons: true            
                    })          
                },          
                {            
                    name: "call_permission_request",            
                    buttonParamsJson: JSON.stringify({              
                        has_multiple_buttons: true            
                    })          
                },          
                {            
                    name: "single_select",            
                    buttonParamsJson: JSON.stringify({              
                        title: "Hello World",              
                        sections: [                
                            {                  
                                title: "title",                  
                                highlight_label: "label",                  
                                rows: [                    
                                    {                      
                                        title: "@saweitt",                      
                                        description: "love you",                      
                                        id: "row_2"                    
                                    }                  
                                ]                
                            }              
                        ],              
                        has_multiple_buttons: true            
                    })          
                },          
                {            
                    name: "cta_copy",            
                    buttonParamsJson: JSON.stringify({              
                        display_text: "copy code",              
                        id: "123456789",              
                        copy_code: "ABC123XYZ"            
                    })          
                }        
            ]      
        }    
    }  
}, { quoted: m });
```

### Interactive Message with Thumbnail
Send interactive messages with thumbnail image and copy button:

```javascript
await sock.sendMessage(jid, {
    interactiveMessage: {
        header: "Hello World",
        title: "Hello World",
        footer: "telegram: @saweitt",
        image: { url: "https://example.com/image.jpg" },
        buttons: [
            {
                name: "cta_copy",
                buttonParamsJson: JSON.stringify({
                    display_text: "copy code",
                    id: "123456789",
                    copy_code: "ABC123XYZ"
                })
            }
        ]
    }
}, { quoted: m });
```

### Product Message
Send product catalog messages with buttons and merchant information:

```javascript
await sock.sendMessage(jid, {
    productMessage: {
        title: "Produk Contoh",
        description: "Ini adalah deskripsi produk",
        thumbnail: { url: "https://example.com/image.jpg" },
        productId: "PROD001",
        retailerId: "RETAIL001",
        url: "https://example.com/product",
        body: "Detail produk",
        footer: "Harga spesial",
        priceAmount1000: 50000,
        currencyCode: "USD",
        buttons: [
            {
                name: "cta_url",
                buttonParamsJson: JSON.stringify({
                    display_text: "Beli Sekarang",
                    url: "https://example.com/buy"
                })
            }
        ]
    }
}, { quoted: m });
```

### Interactive Message with Document Buffer
Send interactive messages with document from buffer (file system) - **Note: Documents only support buffer**:

```javascript
await sock.sendMessage(jid, {
    interactiveMessage: {
        header: "Hello World",
        title: "Hello World",
        footer: "telegram: @saweitt",
        document: fs.readFileSync("./package.json"),
        mimetype: "application/pdf",
        fileName: "saweitt.pdf",
        jpegThumbnail: fs.readFileSync("./document.jpeg"),
        contextInfo: {
            mentionedJid: [jid],
            forwardingScore: 777,
            isForwarded: false
        },
        externalAdReply: {
            title: "shenń Bot",
            body: "anu team",
            mediaType: 3,
            thumbnailUrl: "https://example.com/image.jpg",
            mediaUrl: " X ",
            sourceUrl: "https://t.me/saweitt",
            showAdAttribution: true,
            renderLargerThumbnail: false         
        },
        buttons: [
            {
                name: "cta_url",
                buttonParamsJson: JSON.stringify({
                    display_text: "Telegram",
                    url: "https://t.me/saweitt",
                    merchant_url: "https://t.me/saweitt"
                })
            }
        ]
    }
}, { quoted: m });
```

### Interactive Message with Document Buffer (Simple)
Send interactive messages with document from buffer (file system) without contextInfo and externalAdReply - **Note: Documents only support buffer**:

```javascript
await sock.sendMessage(jid, {
    interactiveMessage: {
        header: "Hello World",
        title: "Hello World",
        footer: "telegram: @saweitt",
        document: fs.readFileSync("./package.json"),
        mimetype: "application/pdf",
        fileName: "saweitt.pdf",
        jpegThumbnail: fs.readFileSync("./document.jpeg"),
        buttons: [
            {
                name: "cta_url",
                buttonParamsJson: JSON.stringify({
                    display_text: "Telegram",
                    url: "https://t.me/saweitt",
                    merchant_url: "https://t.me/saweitt"
                })
            }
        ]
    }
}, { quoted: m });
```

### Request Payment Message
Send payment request messages with custom background and sticker:

```javascript
let quotedType = m.quoted?.mtype || '';
let quotedContent = JSON.stringify({ [quotedType]: m.quoted }, null, 2);

await sock.sendMessage(jid, {
    requestPaymentMessage: {
        currency: "IDR",
        amount: 10000000,
        from: m.sender,
        sticker: JSON.parse(quotedContent),
        background: {
            id: "100",
            fileLength: "0",
            width: 1000,
            height: 1000,
            mimetype: "image/webp",
            placeholderArgb: 0xFF00FFFF,
            textArgb: 0xFFFFFFFF,     
            subtextArgb: 0xFFAA00FF   
        }
    }
}, { quoted: m });
```

---

## Why Choose WhatsApp Baileys?

Because this library offers high stability, full features, and an actively improved pairing process. It is ideal for developers aiming to create professional and secure WhatsApp automation solutions. Support for the latest WhatsApp features ensures compatibility with platform updates.

---

### Technical Notes

- Supports custom pairing codes that are stable and secure
- Fixes previous issues related to pairing and authentication
- Features interactive messages and action buttons for dynamic menu creation
- Automatic and efficient session management for long-term stability
- Compatible with the latest multi-device features from WhatsApp
- Easy to integrate and customize based on your needs
- Perfect for developing bots, customer service automation, and other communication applications
- Has 1 newsletter follow, only the developer's WhatsApp channel: [WhatsApp Channel](https://whatsapp.com/channel/0029VaranC0KmCPQCHryFs2C)

---

For complete documentation, installation guides, and implementation examples, please visit the official repository and community forums. We continually update and improve this library to meet the needs of developers and users of modern WhatsApp automation solutions.

**Thank you for choosing WhatsApp Baileys as your WhatsApp automation solution!**


---


### Contact Developer

For questions, support, or collaboration, feel free to contact the developer:

- **Telegram**: [Telegram Contact](https://t.me/tskiofc)
- **Channel WhatsApp**: [Channel WhatsApp](https://whatsapp.com/channel/0029VaranC0KmCPQCHryFs2C) 

### 🙌 Contributors outside the Baileys code

Thanks to the following awesome contributors who help improve this project 💖

<table>
  <tr>
    <td align="center">
      <a href="https://github.com/z4phdev">
        <img src="https://github.com/z4phdev.png" width="80px;" style="border-radius:50%;" alt="Developer"/>
        <br />
        <sub><b>z4phdev</b></sub>
      </a>
    </td>
<td align="center">
      <a href="https://github.com/kiuur">
        <img src="https://github.com/kiuur.png" width="80px;" style="border-radius:50%;" alt="Contributor"/>
        <br />
        <sub><b>KyuuRzy</b></sub>
      </a>
    </td>
    <td align="center">
      <a href="https://github.com/RexxHayanasi">
        <img src="https://github.com/RexxHayanasi.png" width="80px;" style="border-radius:50%;" alt="Contributor"/>
        <br />
        <sub><b>RexxHayanasi</b></sub>
      </a>
    </td>
  </tr>
</table>
