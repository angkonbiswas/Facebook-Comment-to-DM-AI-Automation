# Facebook Comment to DM AI Automation 🤖

An AI-powered Facebook Page automation system built with **n8n, OpenAI, Google Sheets, and the Facebook Graph API**.

## 🎯 Problem It Solves

Businesses often run Facebook campaigns where users comment things like **“link please,” “send me,” “PDF চাই,” or “material কোথায় পাব?”**. Manually identifying these requests, checking which campaign the comment belongs to, finding the correct resource, and sending it through Messenger is repetitive, time-consuming, and difficult to scale.

This automation solves that problem by handling the process automatically. It receives Facebook comment events through a webhook, checks the related campaign and resource from Google Sheets, and uses AI to determine whether the user is genuinely requesting the resource or whether the comment is irrelevant, spam, abusive, or promotional. fileciteturn13file0L13-L20 fileciteturn13file3L321-L339

For valid requests, the system automatically:

- ✅ Sends the requested resource through Facebook Messenger
- ✅ Posts a short public reply such as **“আপনার ইনবক্স চেক করুন!”**
- ✅ Finds the correct resource using the Facebook Post ID
- ✅ Ignores irrelevant, spam, abusive, or promotional comments
- ✅ Prevents AI from inventing resources by using campaign information from the Google Sheet only fileciteturn13file3L328-L339

## 🔄 Workflow

```text
Facebook Comment
       ↓
   Facebook Webhook
       ↓
Webhook Verification / Feed Filter
       ↓
      AI Agent
       ↓
Google Sheets Campaign Lookup
       ↓
Structured AI Output
       ↓
   Is Valid Request?
      ↙       ↘
    No          Yes
    ↓            ↓
No action   Facebook Graph API
                ↓
        Private DM + Public Reply
```

## 🧠 AI's Role

AI is used specifically for the part that requires language understanding: interpreting the user's Facebook comment, deciding whether the user genuinely wants the campaign resource, and generating natural Bengali DM and public-reply messages. Webhook handling, Google Sheets lookup, conditional routing, and Facebook Graph API requests are handled by deterministic automation. fileciteturn13file0L13-L20

## ✨ Key Features

- Facebook webhook event handling
- Webhook verification with `verify_token` and `hub.challenge`
- Feed-event filtering
- AI-based comment intent classification
- Google Sheets campaign/resource lookup
- Bengali AI-generated responses
- Structured JSON output validation
- Automatic private Messenger delivery
- Automatic public Facebook comment reply
- Conditional valid/invalid request routing
- Spam, irrelevant, abusive, and unsupported comment filtering

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| **n8n** | Workflow orchestration |
| **OpenAI / GPT-5-mini** | Comment intent understanding and response generation |
| **Google Sheets** | Campaign and resource data source |
| **Facebook Graph API** | Messenger and public comment actions |
| **Webhooks** | Facebook event ingestion |
| **Structured Output Parser** | Reliable AI response format |

The workflow uses an OpenAI Chat Model with `gpt-5-mini` and a Google Sheets tool that looks up campaign information by Post ID. fileciteturn13file2L199-L256

## 📋 AI Output

The AI Agent returns a predictable structure enforced by a Structured Output Parser:

```json
{
  "isValid": true,
  "dmMessage": "...",
  "publicReply": "..."
}
```

This prevents downstream automation nodes from breaking because of unpredictable AI output such as missing fields or invalid JSON. fileciteturn13file0L34-L43

## 🔐 Webhook Verification

The workflow uses two separate verification values:

- **`verify_token`** — confirms that Facebook is communicating with the intended webhook owner.
- **`hub.challenge`** — Facebook's challenge value, which the webhook must return to prove that the endpoint is reachable and responding correctly. fileciteturn13file0L21-L32

The n8n workflow checks the verification request and returns the exact `hub.challenge` value when verification succeeds. fileciteturn13file1L79-L135

## 🤖 Decision Logic

The AI first uses the Google Sheets campaign data for the relevant Post ID. A request is valid when a matching campaign exists and the comment clearly asks for the resource. Comments such as generic compliments, emoji-only messages, spam, abusive content, advertisements, promotional messages, or comments without a matching campaign should produce no message. fileciteturn13file3L323-L342

For valid requests:

- `dmMessage` contains the requested resource/link.
- `publicReply` remains short and does not expose the resource publicly.

For invalid requests, both message fields remain empty. fileciteturn13file4L362-L371

## 📈 Business Value

This workflow can help businesses:

- Reduce manual Facebook comment monitoring
- Deliver campaign resources faster
- Handle high-volume comment requests consistently
- Filter low-value and spam interactions automatically
- Maintain a better customer experience through immediate responses
- Scale campaign engagement without adding manual support workload

## 🔒 Security

**Never commit real credentials to this repository.** Facebook access tokens, OpenAI API keys, Google credentials, webhook verification secrets, and other sensitive values should be stored securely in n8n credentials/environment variables.

## 🚀 Future Improvements

- Add API retry and error-handling workflows
- Add execution logging and monitoring
- Add campaign analytics
- Support multiple campaign/resource types
- Add multilingual response support
- Add automated testing for webhook payloads and AI outputs
- Add production-grade secret management

## 👨‍💻 Author

**Angkon Biswas**  
AI Automation Engineer | AI Agents | Workflow Automation

[GitHub](https://github.com/angkonbiswas)
