{
  "name": "Website Contact Form Backend",
  "nodes": [
    {
      "parameters": {
        "httpMethod": "POST",
        "path": "contact-form",
        "responseMode": "responseNode",
        "options": {}
      },
      "id": "webhook-node",
      "name": "Webhook",
      "type": "n8n-nodes-base.webhook",
      "typeVersion": 2,
      "position": [
        240,
        300
      ],
      "webhookId": "contact-form-webhook"
    },
    {
      "parameters": {
        "mode": "manual",
        "fields": {
          "values": [
            {
              "name": "name",
              "stringValue": "={{$json.body.name}}"
            },
            {
              "name": "email",
              "stringValue": "={{$json.body.email}}"
            },
            {
              "name": "phone",
              "stringValue": "={{$json.body.phone}}"
            },
            {
              "name": "message",
              "stringValue": "={{$json.body.message}}"
            }
          ]
        },
        "options": {}
      },
      "id": "set-node",
      "name": "Set Fields",
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        500,
        300
      ]
    },
    {
      "parameters": {
        "sendTo": "yourmail@example.com",
        "subject": "=New Website Lead from {{$json.name}}",
        "message": "=You received a new website inquiry.\n\nName: {{$json.name}}\nEmail: {{$json.email}}\nPhone: {{$json.phone}}\nMessage: {{$json.message}}",
        "options": {}
      },
      "id": "gmail-node",
      "name": "Send Email",
      "type": "n8n-nodes-base.gmail",
      "typeVersion": 2.1,
      "position": [
        760,
        300
      ],
      "credentials": {
        "gmailOAuth2": {
          "id": "YOUR_GMAIL_CREDENTIAL_ID",
          "name": "Gmail account"
        }
      }
    },
    {
      "parameters": {
        "respondWith": "json",
        "responseBody": "={\"status\":\"success\",\"message\":\"Form submitted successfully\"}",
        "options": {}
      },
      "id": "respond-node",
      "name": "Respond to Webhook",
      "type": "n8n-nodes-base.respondToWebhook",
      "typeVersion": 1.1,
      "position": [
        1020,
        300
      ]
    }
  ],
  "pinData": {},
  "connections": {
    "Webhook": {
      "main": [
        [
          {
            "node": "Set Fields",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Set Fields": {
      "main": [
        [
          {
            "node": "Send Email",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Send Email": {
      "main": [
        [
          {
            "node": "Respond to Webhook",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": false,
  "settings": {},
  "versionId": "1"
}
