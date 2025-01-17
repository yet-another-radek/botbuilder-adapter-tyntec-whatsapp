# `interface Activity`

`Activity` is a Microsoft Bot Framework representation of actions made by
conversation participants.

You can find the full specification of the `Activity` interface in the
Microsoft Bot Framework SDK documentation at https://docs.microsoft.com/en-us/azure/bot-service/index-bf-sdk
and in Microsoft Bot Framework SDK specs at https://github.com/microsoft/botframework-sdk/tree/main/specs.

However, at the moment, Tyntec WhatsApp Adapter supports only a limited subset
of activities. This means, that both activities passed to the adapter and
adapter created activities have stricter requirements.

The only supported activity type is a [WhatsApp message](#whatsapp-message-activity).

Properties of all supported activities:
* `type = "message"` (REQUIRED)
* `channelId = "whatsapp"` (REQUIRED)


## WhatsApp Message Activity

Properties of all supported WhatsApp message activities:
* `from: ChannelAccount` (REQUIRED)
* `from.id: string` (REQUIRED) - the WhatsApp ID of the sender
* `conversation: ConversationAccount` (REQUIRED)
* `conversation.id: string` (REQUIRED) - the WhatsApp ID of the recipient
* `replyToId?: string` (IGNORED)
* `entities?: Entity[]` (IGNORED)
* `textFormat?: TextFormatTypes` (IGNORED)
* `locale?: string` (IGNORED)
* `speak = undefined` (DISALLOWED)
* `inputHint?: string` (IGNORED)
* `attachmentLayout?: AttachmentLayoutTypes` (IGNORED)
* `suggestedActions?: SuggestedActions` (IGNORED)
* `expiration?: Date` (IGNORED)
* `importance?: ActivityImportance` (IGNORED)
* `deliveryMode = undefined` (DISALLOWED)
* `listenFor?: string[]` (IGNORED)
* `semanticAction?: SemanticAction` (IGNORED)

Additional properties of channel WhatsApp message activities:
* `from.name?: string` (OPTIONAL) - the display name of the sender
* `recipient: ChannelAccount` (REQUIRED)
* `recipient.id: string` (REQUIRED) - the WhatsApp ID of the recipient
* `conversation.name?: string` (OPTIONAL) - the display name of the sender
* `conversation.isGroup = false` (REQUIRED)

The supported WhatsApp messages are [audio](#whatsapp-audio-message-activity),
[contacts](#whatsapp-contacts-message-activity),
[document](#whatsapp-document-message-activity),
[image](#whatsapp-image-message-activity),
[interactive](#whatsapp-interactive-message-activity),
[location](#whatsapp-location-message-activity),
[postback](#whatsapp-postback-message-activity),
[sticker](#whatsapp-sticker-message-activity),
[template](#whatsapp-template-message-activity),
[text](#whatsapp-text-message-activity),
[video](#whatsapp-video-message-activity) and
[voice](#whatsapp-voice-message-activity) messages.


### WhatsApp Audio Message Activity

Properties of all supported WhatsApp audio message activities:
* `channelData: any` (REQUIRED)
* `channelData.contentType = "audio"` (REQUIRED)
* `channelData.contacts = undefined` (DISALLOWED)
* `channelData.interactive = undefined` (DISALLOWED)
* `channelData.location = undefined` (DISALLOWED)
* `channelData.postback = undefined` (DISALLOWED)
* `channelData.template = undefined` (DISALLOWED)
* `text = undefined` (DISALLOWED)
* `attachments: Attachment[]` (REQUIRED) - exactly one attachment is required
* `attachments[i].content = undefined` (DISALLOWED)
* `attachments[i].contentUrl: string` (REQUIRED)
* `attachments[i].thumbnailUrl = undefined` (DISALLOWED)

The MIME MUST be either `audio/aac`, `audio/mp4`, `audio/amr`, `audio/mpeg` or
`audio/ogg; codecs=opus`. The size MUST be up to 16 MB.

A WhatsApp audio message activity example:

```javascript
activity === {
    type: "message",
    channelId: "whatsapp",
    id: "77185196-664a-43ec-b14a-fe97036c697e",
    timestamp: new Date("2019-06-26T09:41:00.000Z"),
    from: {
        id: "+1233423454"
    },
    recipient: {
        id: "545345345"
    },
    conversation: {
        id: "+1233423454",
        isGroup: false,
        name: "John Doe"
    },
    channelData: {
        contentType: "audio"
    },
    serviceUrl: "https://api.tyntec.com/conversations/v3/messages",
    attachments: [
        {
            contentType: "audio/aac",
            contentUrl: "https://example.com/audio.ac3"
        }
    ]
}
```


### WhatsApp Contacts Message Activity

Properties of all supported WhatsApp contacts message activities:
* `channelData: any` (REQUIRED)
* `channelData.contentType = "contacts"` (REQUIRED)
* `channelData.contacts: Contact` (REQUIRED) - a valid [Contact](https://api.tyntec.com/reference/conversations/current.html)
  object
* `channelData.interactive = undefined` (DISALLOWED)
* `channelData.location = undefined` (DISALLOWED)
* `channelData.postback = undefined` (DISALLOWED)
* `channelData.template = undefined` (DISALLOWED)
* `text = undefined` (DISALLOWED)
* `attachments = undefined` (DISALLOWED)

A WhatsApp contacts message activity example:

```javascript
activity === {
    type: "message",
    channelId: "whatsapp",
    id: "77185196-664a-43ec-b14a-fe97036c697e",
    timestamp: new Date("2019-06-26T09:41:00.000Z"),
    from: {
        id: "+1233423454"
    },
    recipient: {
        id: "545345345"
    },
    conversation: {
        id: "+1233423454",
        isGroup: false,
        name: "John Doe"
    },
    channelData: {
        contentType: "contacts",
        contacts: [{
            addresses: [{city: "Dortmund", type: "WORK"}],
            emails: [{email: "whatsapp@tyntec.com", type: "WORK"}],
            ims: [],
            name: {firstName: "Peter", formattedName: "Peter Tyntec", lastName: "Tyntec"},
            org: {},
            phones: [{phone: "+49 231 477 90 813", type: "WORK"}],
            urls: []
        }]
    },
    serviceUrl: "https://api.tyntec.com/conversations/v3/messages"
}
```


### WhatsApp Document Message Activity

Properties of all supported WhatsApp document message activities:
* `channelData: any` (REQUIRED)
* `channelData.contentType = "document"` (REQUIRED)
* `channelData.contacts = undefined` (DISALLOWED)
* `channelData.interactive = undefined` (DISALLOWED)
* `channelData.location = undefined` (DISALLOWED)
* `channelData.postback = undefined` (DISALLOWED)
* `channelData.template = undefined` (DISALLOWED)
* `text?: string` (OPTIONAL) - the document caption
* `attachments: Attachment[]` (REQUIRED) - exactly one attachment is required
* `attachments[i].content = undefined` (DISALLOWED)
* `attachments[i].contentUrl: string` (REQUIRED)
* `attachments[i].thumbnailUrl = undefined` (DISALLOWED)

The name attachment name (`attachments[i].name`) MUST be up to 240 characters
long. The size MUST be up to 100 MB.

A WhatsApp document message activity example:

```javascript
activity === {
    type: "message",
    channelId: "whatsapp",
    id: "77185196-664a-43ec-b14a-fe97036c697e",
    timestamp: new Date("2019-06-26T09:41:00.000Z"),
    from: {
        id: "+1233423454"
    },
    recipient: {
        id: "545345345"
    },
    conversation: {
        id: "+1233423454",
        isGroup: false,
        name: "John Doe"
    },
    channelData: {
        contentType: "document"
    },
    serviceUrl: "https://api.tyntec.com/conversations/v3/messages",
    text: "A document caption",
    attachments: [
        {
            contentType: "application/pdf",
            contentUrl: "https://example.com/document.pdf",
            name: "document.pdf"
        }
    ]
}
```


### WhatsApp Image Message Activity

Properties of all supported WhatsApp image message activities:
* `channelData: any` (REQUIRED)
* `channelData.contentType = "image"` (REQUIRED)
* `channelData.contacts = undefined` (DISALLOWED)
* `channelData.interactive = undefined` (DISALLOWED)
* `channelData.location = undefined` (DISALLOWED)
* `channelData.postback = undefined` (DISALLOWED)
* `channelData.template = undefined` (DISALLOWED)
* `text?: string` (OPTIONAL) - the image caption
* `attachments: Attachment[]` (REQUIRED) - exactly one attachment is required
* `attachments[i].content = undefined` (DISALLOWED)
* `attachments[i].contentUrl: string` (REQUIRED)
* `attachments[i].thumbnailUrl = undefined` (DISALLOWED)

The image caption (`text`) MUST be up to 4096 characters long. The MIME MUST be
either `image/jpeg` or `image/png`. The size MUST be up to 5 MB.

A WhatsApp image message activity example:

```javascript
activity === {
    type: "message",
    channelId: "whatsapp",
    id: "77185196-664a-43ec-b14a-fe97036c697e",
    timestamp: new Date("2019-06-26T09:41:00.000Z"),
    from: {
        id: "+1233423454"
    },
    recipient: {
        id: "545345345"
    },
    conversation: {
        id: "+1233423454",
        isGroup: false,
        name: "John Doe"
    },
    channelData: {
        contentType: "image"
    },
    serviceUrl: "https://api.tyntec.com/conversations/v3/messages",
    text: "An image caption",
    attachments: [
        {
            contentType: "image/jpeg",
            contentUrl: "https://example.com/image.png"
        }
    ]
}
```


### WhatsApp Interactive Message Activity

Properties of all supported WhatsApp interactive message activities:
* `channelData: any` (REQUIRED)
* `channelData.contentType = "interactive"` (REQUIRED)
* `channelData.contacts = undefined` (DISALLOWED)
* `channelData.interactive: WhatsAppInteractiveButtonMessage | WhatsAppInteractiveListMessage` (REQUIRED) - a valid [WhatsAppInteractiveButtonMessage](https://api.tyntec.com/reference/conversations/current.html)
  object or a valid [WhatsAppInteractiveListMessage](https://api.tyntec.com/reference/conversations/current.html)
  object
* `channelData.location = undefined` (DISALLOWED)
* `channelData.postback = undefined` (DISALLOWED)
* `channelData.template = undefined` (DISALLOWED)
* `text = undefined` (DISALLOWED)
* `attachments = undefined` (DISALLOWED)

A WhatsApp interactive message activity example:

```javascript
activity === {
    type: "message",
    channelId: "whatsapp",
    id: "77185196-664a-43ec-b14a-fe97036c697e",
    timestamp: new Date("2019-06-26T09:41:00.000Z"),
    from: {
        id: "+1233423454"
    },
    recipient: {
        id: "545345345"
    },
    conversation: {
        id: "+1233423454",
        isGroup: false,
        name: "John Doe"
    },
    channelData: {
        contentType: "interactive",
        interactive: {
            subType: "buttons",
            components: {
                body: {
                    type: "text",
                    text: "How would you rate your bot experience"
                },
                buttons: [
                    {
                        type: "reply",
                        reply: {
                            payload: "9080923445nlkjß0_gß0923845083245dfg",
                            title: "Good"
                        }
                    }
                ]
            }
        }
    },
    serviceUrl: "https://api.tyntec.com/conversations/v3/messages"
}
```


### WhatsApp Location Message Activity

Properties of all supported WhatsApp location message activities:
* `channelData: any` (REQUIRED)
* `channelData.contentType = "location"` (REQUIRED)
* `channelData.contacts = undefined` (DISALLOWED)
* `channelData.interactive = undefined` (DISALLOWED)
* `channelData.location: WhatsAppLocation` (REQUIRED) - a valid [WhatsAppLocation](https://api.tyntec.com/reference/conversations/current.html)
  object
* `channelData.postback = undefined` (DISALLOWED)
* `channelData.template = undefined` (DISALLOWED)
* `text = undefined` (DISALLOWED)
* `attachments = undefined` (DISALLOWED)

A WhatsApp location message activity example:

```javascript
activity === {
    type: "message",
    channelId: "whatsapp",
    id: "77185196-664a-43ec-b14a-fe97036c697e",
    timestamp: new Date("2019-06-26T09:41:00.000Z"),
    from: {
        id: "+1233423454"
    },
    recipient: {
        id: "545345345"
    },
    conversation: {
        id: "+1233423454",
        isGroup: false,
        name: "John Doe"
    },
    channelData: {
        contentType: "location",
        location: {
            address: "tyntec GmbH, Semerteichstraße, Dortmund",
            latitude: 51.5005765,
            longitude: 7.4954884,
            name: "tyntec GmbH"
        }
    },
    serviceUrl: "https://api.tyntec.com/conversations/v3/messages"
}
```


### WhatsApp Postback Message Activity

Properties of all supported WhatsApp postback message activities:
* `channelData: any` (REQUIRED)
* `channelData.contentType = "postback"` (REQUIRED)
* `channelData.contacts = undefined` (DISALLOWED)
* `channelData.interactive = undefined` (DISALLOWED)
* `channelData.location = undefined` (DISALLOWED)
* `channelData.postback: any` (REQUIRED)
* `channelData.postback.data: string` (REQUIRED) - the payload of the interactive item clicked
* `channelData.postback.whatsapp?: any` (OPTIONAL)
* `channelData.postback.whatsapp.description?: string` (OPTIONAL) - the description of the interactive item clicked
* `channelData.postback.whatsapp.text?: string` (OPTIONAL) - the caption of the interactive item clicked
* `channelData.postback.whatsapp.title?: string` (OPTIONAL) - the title of the interactive item clicked
* `channelData.template = undefined` (DISALLOWED)
* `text = undefined` (DISALLOWED)
* `attachments = undefined` (DISALLOWED)

A WhatsApp postback message activity example:

```javascript
activity === {
    type: "message",
    channelId: "whatsapp",
    id: "77185196-664a-43ec-b14a-fe97036c697e",
    timestamp: new Date("2019-06-26T09:41:00.000Z"),
    from: {
        id: "+1233423454"
    },
    recipient: {
        id: "545345345"
    },
    conversation: {
        id: "+1233423454",
        isGroup: false,
        name: "John Doe"
    },
    channelData: {
        contentType: "postback",
        postback: {
            data: "9080923445nlkjß0_gß0923845083245dfg",
            whatsapp: {
                title: "Good"
            }
        }
    },
    serviceUrl: "https://api.tyntec.com/conversations/v3/messages"
}
```


### WhatsApp Sticker Message Activity

Properties of all supported WhatsApp sticker message activities:
* `channelData: any` (REQUIRED)
* `channelData.contentType = "sticker"` (REQUIRED)
* `channelData.contacts = undefined` (DISALLOWED)
* `channelData.interactive = undefined` (DISALLOWED)
* `channelData.location = undefined` (DISALLOWED)
* `channelData.postback = undefined` (DISALLOWED)
* `channelData.template = undefined` (DISALLOWED)
* `text = undefined` (DISALLOWED)
* `attachments: Attachment[]` (REQUIRED) - exactly one attachment is required
* `attachments[i].content = undefined` (DISALLOWED)
* `attachments[i].contentUrl: string` (REQUIRED)
* `attachments[i].thumbnailUrl = undefined` (DISALLOWED)

The MIME MUST be `image/webp`. The size MUST be up to 100 KB. The dimensions
MUST be 512 px x 512 px. There MUST be a 16 px margin around the sticker. The
background MUST be transparent.

A WhatsApp sticker message activity example:

```javascript
activity === {
    type: "message",
    channelId: "whatsapp",
    id: "77185196-664a-43ec-b14a-fe97036c697e",
    timestamp: new Date("2019-06-26T09:41:00.000Z"),
    from: {
        id: "+1233423454"
    },
    recipient: {
        id: "545345345"
    },
    conversation: {
        id: "+1233423454",
        isGroup: false,
        name: "John Doe"
    },
    channelData: {
        contentType: "sticker"
    },
    serviceUrl: "https://api.tyntec.com/conversations/v3/messages",
    attachments: [
        {
            contentType: "image/webp",
            contentUrl: "https://example.com/sticker.webp"
        }
    ]
}
```


### WhatsApp Template Message Activity

Properties of all supported WhatsApp template message activities:
* `channelData: any` (REQUIRED)
* `channelData.contentType = "template"` (REQUIRED)
* `channelData.contacts = undefined` (DISALLOWED)
* `channelData.interactive = undefined` (DISALLOWED)
* `channelData.location = undefined` (DISALLOWED)
* `channelData.postback = undefined` (DISALLOWED)
* `channelData.template: WhatsAppTemplate` (REQUIRED) - a valid [WhatsAppTemplate](https://api.tyntec.com/reference/conversations/current.html)
  object
* `text = undefined` (DISALLOWED)
* `attachments = undefined` (DISALLOWED)

The template must be up to 1024 characters long.

A WhatsApp template message activity example:

```javascript
activity === {
    type: "message",
    channelId: "whatsapp",
    id: "77185196-664a-43ec-b14a-fe97036c697e",
    timestamp: new Date("2019-06-26T09:41:00.000Z"),
    from: {
        id: "+1233423454"
    },
    recipient: {
        id: "545345345"
    },
    conversation: {
        id: "+1233423454",
        isGroup: false,
        name: "John Doe"
    },
    channelData: {
        contentType: "template",
        template: {
            templateId: "template_id",
            templateLanguage: "en",
            components: {
                header: [
                    {
                        type: "image",
                        image: {
                            url: "https://example.com/image.png"
                        }
                    }
                ],
                body: [
                    {
                        type: "text",
                        text: "lorem"
                    }
                ]
            }
        }
    },
    serviceUrl: "https://api.tyntec.com/conversations/v3/messages"
}
```


### WhatsApp Text Message Activity

Properties of all supported WhatsApp text message activities:
* `channelData?: any` (OPTIONAL)
* `channelData.contentType = "text"` (OPTIONAL)
* `channelData.contacts = undefined` (DISALLOWED)
* `channelData.interactive = undefined` (DISALLOWED)
* `channelData.location = undefined` (DISALLOWED)
* `channelData.postback = undefined` (DISALLOWED)
* `channelData.template = undefined` (DISALLOWED)
* `text: string` (REQUIRED)
* `attachments = undefined` (DISALLOWED)

The text content (`text`) MUST be up to 4096 characters long.

A WhatsApp text message activity example:

```javascript
activity === {
    type: "message",
    channelId: "whatsapp",
    id: "77185196-664a-43ec-b14a-fe97036c697e",
    timestamp: new Date("2019-06-26T09:41:00.000Z"),
    from: {
        id: "+1233423454"
    },
    recipient: {
        id: "545345345"
    },
    conversation: {
        id: "+1233423454",
        isGroup: false,
        name: "John Doe"
    },
    channelData: {
        contentType: "text"
    },
    serviceUrl: "https://api.tyntec.com/conversations/v3/messages",
    text: "A simple text message"
}
```


### WhatsApp Video Message Activity

Properties of all supported WhatsApp video message activities:
* `channelData: any` (REQUIRED)
* `channelData.contentType = "video"` (REQUIRED)
* `channelData.contacts = undefined` (DISALLOWED)
* `channelData.interactive = undefined` (DISALLOWED)
* `channelData.location = undefined` (DISALLOWED)
* `channelData.postback = undefined` (DISALLOWED)
* `channelData.template = undefined` (DISALLOWED)
* `text?: string` (OPTIONAL) - the video caption
* `attachments: Attachment[]` (REQUIRED) - exactly one attachment is required
* `attachments[i].content = undefined` (DISALLOWED)
* `attachments[i].contentUrl: string` (REQUIRED)
* `attachments[i].thumbnailUrl = undefined` (DISALLOWED)

The video caption (`text`) MUST be up to 4096 characters long. The MIME MUST be
either `video/mp4` or `video/3gpp`. The size MUST be up to 16 MB. The video
codec MUST be H.264. The audio codec MUST be AAC. The audio stream MUST be only
one.

A WhatsApp video message activity example:

```javascript
activity === {
    type: "message",
    channelId: "whatsapp",
    id: "77185196-664a-43ec-b14a-fe97036c697e",
    timestamp: new Date("2019-06-26T09:41:00.000Z"),
    from: {
        id: "+1233423454"
    },
    recipient: {
        id: "545345345"
    },
    conversation: {
        id: "+1233423454",
        isGroup: false,
        name: "John Doe"
    },
    channelData: {
        contentType: "video"
    },
    serviceUrl: "https://api.tyntec.com/conversations/v3/messages",
    text: "A video caption",
    attachments: [
        {
            contentType: "video/mp4",
            contentUrl: "https://example.com/video.mp4"
        }
    ]
}
```


### WhatsApp Voice Message Activity

Properties of all supported WhatsApp voice message activities:
* `channelData: any` (REQUIRED)
* `channelData.contentType = "voice"` (REQUIRED)
* `channelData.contacts = undefined` (DISALLOWED)
* `channelData.interactive = undefined` (DISALLOWED)
* `channelData.location = undefined` (DISALLOWED)
* `channelData.postback = undefined` (DISALLOWED)
* `channelData.template = undefined` (DISALLOWED)
* `text = undefined` (DISALLOWED)
* `attachments: Attachment[]` (REQUIRED) - exactly one attachment is required
* `attachments[i].content = undefined` (DISALLOWED)
* `attachments[i].contentUrl: string` (REQUIRED)
* `attachments[i].thumbnailUrl = undefined` (DISALLOWED)

A WhatsApp voice message activity example:

```javascript
activity === {
    type: "message",
    channelId: "whatsapp",
    id: "77185196-664a-43ec-b14a-fe97036c697e",
    timestamp: new Date("2019-06-26T09:41:00.000Z"),
    from: {
        id: "+1233423454"
    },
    recipient: {
        id: "545345345"
    },
    conversation: {
        id: "+1233423454",
        isGroup: false,
        name: "John Doe"
    },
    channelData: {
        contentType: "voice"
    },
    serviceUrl: "https://api.tyntec.com/conversations/v3/messages",
    attachments: [
        {
            contentType: "audio/ogg; codecs=opus",
            contentUrl: "https://example.com/voice.ogg"
        }
    ]
}
```
