{
  "name": "Agente teste",
  "nodes": [
    {
      "parameters": {
        "httpMethod": "POST",
        "path": "55a12fe0-0ac3-4a01-89c4-f6ab5ae3a08f",
        "options": {}
      },
      "type": "n8n-nodes-base.webhook",
      "typeVersion": 2,
      "position": [
        -80,
        0
      ],
      "id": "859bb0fc-2fa2-4efb-9493-0b12c4642884",
      "name": "Webhook",
      "webhookId": "55a12fe0-0ac3-4a01-89c4-f6ab5ae3a08f"
    },
    {
      "parameters": {
        "rules": {
          "values": [
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 2
                },
                "conditions": [
                  {
                    "leftValue": "={{ $item(\"0\").$node[\"Webhook\"].json[\"body\"][\"data\"][\"messageType\"] }}",
                    "rightValue": "conversation",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    },
                    "id": "0b5d8505-1dd2-4296-af23-53c9c72e64bb"
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "conversation"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 2
                },
                "conditions": [
                  {
                    "id": "2bd86ac3-0c57-40ed-a6c5-727d9f041896",
                    "leftValue": "={{ $item(\"0\").$node[\"Webhook\"].json[\"body\"][\"data\"][\"messageType\"] }}",
                    "rightValue": "extendedTextMenssage",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "extendedTextMenssage"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 2
                },
                "conditions": [
                  {
                    "id": "a4a36c5c-918a-4409-a868-f53cba2bbaa9",
                    "leftValue": "={{ $item(\"0\").$node[\"Webhook\"].json[\"body\"][\"data\"][\"messageType\"] }}",
                    "rightValue": "audioMessage",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "audio"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.switch",
      "typeVersion": 3.2,
      "position": [
        160,
        0
      ],
      "id": "dfd42ca2-1077-4401-bb81-cf787e043f5d",
      "name": "Roteamento de mensagens recebidas"
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "98b7aa54-ed96-4a9f-96af-91cf81d01fcd",
              "name": "mensagem",
              "value": "={{ $item(\"0\").$node[\"Roteamento de mensagens recebidas\"].json[\"body\"][\"data\"][\"message\"][\"conversation\"] }}",
              "type": "string"
            },
            {
              "id": "d2701eb1-302d-4fd8-9120-58747359c470",
              "name": "remotejid",
              "value": "={{ $item(\"0\").$node[\"Roteamento de mensagens recebidas\"].json[\"body\"][\"data\"][\"key\"][\"remoteJid\"] }}",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        580,
        -240
      ],
      "id": "1bffca38-3dfe-4c1b-ad96-baef84cbeccf",
      "name": "setando mensagem (conversation)"
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "98b7aa54-ed96-4a9f-96af-91cf81d01fcd",
              "name": "mensagem",
              "value": "={{ $item(\"0\").$node[\"Roteamento de mensagens recebidas\"].json[\"body\"][\"data\"][\"message\"][\"extendedTextMenssage\"] [\"text\"] }}",
              "type": "string"
            },
            {
              "id": "d2701eb1-302d-4fd8-9120-58747359c470",
              "name": "remotejid",
              "value": "={{ $item(\"0\").$node[\"Roteamento de mensagens recebidas\"].json[\"body\"][\"data\"][\"key\"][\"remoteJid\"] }}",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        580,
        0
      ],
      "id": "625a4348-4071-4c15-8003-38f5d516ee1c",
      "name": "setando mensagem (extendedTextMessage)"
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "685ac5eb-82d5-4443-b873-18c3e59e6c25",
              "name": "base64",
              "value": "={{ $item(\"0\").$node[\"Roteamento de mensagens recebidas\"].json[\"body\"][\"data\"][\"message\"][\"base64\"] }}",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        560,
        260
      ],
      "id": "0f6c1604-06f4-4e2d-9d58-c3c2ea440738",
      "name": "setando base 64"
    },
    {
      "parameters": {
        "operation": "toBinary",
        "sourceProperty": "base64",
        "options": {
          "mimeType": "audio/mp3"
        }
      },
      "type": "n8n-nodes-base.convertToFile",
      "typeVersion": 1.1,
      "position": [
        720,
        260
      ],
      "id": "6bc03151-8691-4758-ae4f-de6ccdac1a08",
      "name": "convertendo o base 64"
    },
    {
      "parameters": {
        "resource": "audio",
        "operation": "transcribe",
        "options": {
          "language": "pt"
        }
      },
      "type": "@n8n/n8n-nodes-langchain.openAi",
      "typeVersion": 1.8,
      "position": [
        940,
        260
      ],
      "id": "6a374593-fc16-48d1-b825-6342484a7f39",
      "name": "OpenAI",
      "credentials": {
        "openAiApi": {
          "id": "b88tHuYNk7kQOY7B",
          "name": "OpenAi account 2"
        }
      }
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "98b7aa54-ed96-4a9f-96af-91cf81d01fcd",
              "name": "mensagem",
              "value": "={{ $item(\"0\").$node[\"OpenAI\"].json[\"text\"] }}",
              "type": "string"
            },
            {
              "id": "d2701eb1-302d-4fd8-9120-58747359c470",
              "name": "remotejid",
              "value": "={{ $item(\"0\").$node[\"Roteamento de mensagens recebidas\"].json[\"body\"][\"data\"][\"key\"][\"remoteJid\"] }}",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        1140,
        260
      ],
      "id": "e7acf3d6-21cc-4793-bbf6-2ab6b6375cd5",
      "name": "setando a mensagem audio"
    },
    {
      "parameters": {
        "numberInputs": 3
      },
      "type": "n8n-nodes-base.merge",
      "typeVersion": 3.1,
      "position": [
        1380,
        -80
      ],
      "id": "35f62460-42d3-4f10-bb2c-59521a86813f",
      "name": "node de referencia da mensagem"
    },
    {
      "parameters": {
        "operation": "getAll",
        "tableId": "webmarcas",
        "returnAll": true,
        "filters": {
          "conditions": [
            {
              "keyName": "remotejid",
              "condition": "eq",
              "keyValue": "={{ $item(\"0\").$node[\"node de referencia da mensagem\"].json[\"remotejid\"] }}"
            }
          ]
        }
      },
      "type": "n8n-nodes-base.supabase",
      "typeVersion": 1,
      "position": [
        1600,
        -80
      ],
      "id": "a52ee438-4a81-40ec-b7f3-a5c4bd411f05",
      "name": "varredura do BD filtrando pelo remotejid",
      "alwaysOutputData": true,
      "credentials": {
        "supabaseApi": {
          "id": "XJVnZZX4is1jdDvh",
          "name": "Supabase WebMarcas"
        }
      }
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 2
          },
          "conditions": [
            {
              "id": "f57739a1-e30d-48c0-aa91-291559b9cb8d",
              "leftValue": "={{ $item(\"0\").$node[\"varredura do BD filtrando pelo remotejid\"].json }}",
              "rightValue": false,
              "operator": {
                "type": "object",
                "operation": "notEmpty",
                "singleValue": true
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.2,
      "position": [
        1860,
        -80
      ],
      "id": "ecf41bd7-4e1d-412b-b51e-ff936f0b899c",
      "name": "verifica se o lead existe"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.merge",
      "typeVersion": 3.1,
      "position": [
        2340,
        -80
      ],
      "id": "8d37cece-382a-4044-b0e8-264fa2559d5b",
      "name": "Merge"
    },
    {
      "parameters": {
        "tableId": "webmarcas",
        "fieldsUi": {
          "fieldValues": [
            {
              "fieldId": "nome",
              "fieldValue": "={{ $item(\"0\").$node[\"Webhook\"].json[\"body\"][\"data\"][\"pushName\"] }}"
            },
            {
              "fieldId": "remotejid",
              "fieldValue": "={{ $item(\"0\").$node[\"node de referencia da mensagem\"].json[\"remotejid\"] }}"
            }
          ]
        }
      },
      "type": "n8n-nodes-base.supabase",
      "typeVersion": 1,
      "position": [
        2100,
        40
      ],
      "id": "703bf46a-1271-448a-9b9d-9de316d4ae3d",
      "name": "cria o lead no bd",
      "credentials": {
        "supabaseApi": {
          "id": "XJVnZZX4is1jdDvh",
          "name": "Supabase WebMarcas"
        }
      }
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 2
          },
          "conditions": [
            {
              "id": "fab32e1e-e436-4f30-9de3-625c66af46e6",
              "leftValue": "={{ $item(\"0\").$node[\"Merge\"].json[\"possui_thread\"] }}",
              "rightValue": "",
              "operator": {
                "type": "boolean",
                "operation": "true",
                "singleValue": true
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.2,
      "position": [
        2620,
        -80
      ],
      "id": "436cc65c-6866-4ffd-a72d-27596da667ed",
      "name": "verifica se existe thread"
    },
    {
      "parameters": {
        "method": "POST",
        "url": "https://api.openai.com/v1/threads",
        "authentication": "predefinedCredentialType",
        "nodeCredentialType": "openAiApi",
        "sendHeaders": true,
        "headerParameters": {
          "parameters": [
            {
              "name": "OpenAI-Beta",
              "value": "assistants=v2"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        2860,
        100
      ],
      "id": "0b26a57f-d025-4b3f-9300-6e4eb42baffa",
      "name": "criar thread",
      "credentials": {
        "openAiApi": {
          "id": "b88tHuYNk7kQOY7B",
          "name": "OpenAi account 2"
        }
      }
    },
    {
      "parameters": {
        "operation": "update",
        "tableId": "webmarcas",
        "filters": {
          "conditions": [
            {
              "keyName": "remotejid",
              "condition": "eq",
              "keyValue": "={{ $item(\"0\").$node[\"node de referencia da mensagem\"].json[\"remotejid\"] }}"
            }
          ]
        },
        "fieldsUi": {
          "fieldValues": [
            {
              "fieldId": "thread_id",
              "fieldValue": "={{ $item(\"0\").$node[\"criar thread\"].json[\"id\"] }}"
            },
            {
              "fieldId": "possui_thread",
              "fieldValue": "true"
            }
          ]
        }
      },
      "type": "n8n-nodes-base.supabase",
      "typeVersion": 1,
      "position": [
        3080,
        100
      ],
      "id": "0bfa4817-35f8-40c1-9df7-57faa95d677a",
      "name": "Atualiza thread no Bd",
      "credentials": {
        "supabaseApi": {
          "id": "XJVnZZX4is1jdDvh",
          "name": "Supabase WebMarcas"
        }
      }
    },
    {
      "parameters": {
        "method": "POST",
        "url": "=https://api.openai.com/v1/threads/{{ $item(\"0\").$node[\"verifica se existe thread\"].json[\"thread_id\"] }}/messages",
        "authentication": "predefinedCredentialType",
        "nodeCredentialType": "openAiApi",
        "sendHeaders": true,
        "headerParameters": {
          "parameters": [
            {
              "name": "OpenAI-Beta",
              "value": "assistants=v2"
            }
          ]
        },
        "sendBody": true,
        "bodyParameters": {
          "parameters": [
            {
              "name": "role",
              "value": "user"
            },
            {
              "name": "content",
              "value": "={{ $item(\"0\").$node[\"node de referencia da mensagem\"].json[\"mensagem\"] }}"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        2860,
        -220
      ],
      "id": "07c5a09d-1155-4c50-83b6-2927fdd870d0",
      "name": "Criando MSG e Add na thread",
      "credentials": {
        "openAiApi": {
          "id": "b88tHuYNk7kQOY7B",
          "name": "OpenAi account 2"
        }
      }
    },
    {
      "parameters": {
        "amount": 3
      },
      "type": "n8n-nodes-base.wait",
      "typeVersion": 1.1,
      "position": [
        3080,
        -220
      ],
      "id": "3eea3eda-ce0f-4d8b-8e27-9e44931e68ec",
      "name": "Wait",
      "webhookId": "8709079b-b0eb-4292-8533-44a56d3e4236"
    },
    {
      "parameters": {
        "url": "=https://api.openai.com/v1/threads/{{ $item(\"0\").$node[\"verifica se existe thread\"].json[\"thread_id\"] }}/messages?order=desc",
        "authentication": "predefinedCredentialType",
        "nodeCredentialType": "openAiApi",
        "sendHeaders": true,
        "headerParameters": {
          "parameters": [
            {
              "name": "OpenAI-Beta",
              "value": "assistants=v2"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        3280,
        -220
      ],
      "id": "5097c2d5-6f98-4d2d-96c0-2a22a6d7f095",
      "name": "lista as mensagens",
      "credentials": {
        "openAiApi": {
          "id": "b88tHuYNk7kQOY7B",
          "name": "OpenAi account 2"
        }
      }
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 2
          },
          "conditions": [
            {
              "id": "7c77a5d3-0ac1-47b5-a3ea-8d549c2db9e4",
              "leftValue": "={{ $item(\"0\").$node[\"Criando MSG e Add na thread\"].json[\"id\"] }}",
              "rightValue": "={{ $item(\"0\").$node[\"lista as mensagens\"].json[\"data\"][\"0\"][\"id\"] }}",
              "operator": {
                "type": "string",
                "operation": "equals",
                "name": "filter.operator.equals"
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.2,
      "position": [
        3560,
        -220
      ],
      "id": "11313c20-3963-45ec-8032-1c9016bf197e",
      "name": "verifica os ids das msg"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.noOp",
      "typeVersion": 1,
      "position": [
        3760,
        180
      ],
      "id": "9ba0476b-4269-45c1-99c9-4f1737fedf6a",
      "name": "No Operation, do nothing1"
    },
    {
      "parameters": {
        "method": "POST",
        "url": "=https://api.openai.com/v1/threads/{{ $item(\"0\").$node[\"verifica se existe thread\"].json[\"thread_id\"] }}/runs",
        "authentication": "predefinedCredentialType",
        "nodeCredentialType": "openAiApi",
        "sendHeaders": true,
        "headerParameters": {
          "parameters": [
            {
              "name": "OpenAI-Beta",
              "value": "assistants=v2"
            }
          ]
        },
        "sendBody": true,
        "bodyParameters": {
          "parameters": [
            {
              "name": "assistant_id",
              "value": "asst_EjXD5YRncm2EeWNNdqnFL12Z"
            },
            {
              "name": "additional_instructions",
              "value": "=sempre chame o usuário pelo nome:\"{{ $item(\"0\").$node[\"Webhook\"].json[\"body\"][\"data\"][\"pushName\"] }}\n\na data e hora de hoje é {{ $now }}"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        3760,
        -300
      ],
      "id": "234ff9ab-9aaa-4dbe-9301-be1abdf4765f",
      "name": "criar run",
      "credentials": {
        "openAiApi": {
          "id": "b88tHuYNk7kQOY7B",
          "name": "OpenAi account 2"
        }
      }
    },
    {
      "parameters": {
        "url": "=https://api.openai.com/v1/threads/{{ $item(\"0\").$node[\"verifica se existe thread\"].json[\"thread_id\"] }}/runs?order=desc",
        "authentication": "predefinedCredentialType",
        "nodeCredentialType": "openAiApi",
        "sendHeaders": true,
        "headerParameters": {
          "parameters": [
            {
              "name": "OpenAI-Beta",
              "value": "assistants=v2"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        3960,
        -300
      ],
      "id": "d924caa4-2311-445d-b8bb-d89ea3ee917d",
      "name": "listar a run",
      "credentials": {
        "openAiApi": {
          "id": "b88tHuYNk7kQOY7B",
          "name": "OpenAi account 2"
        }
      }
    },
    {
      "parameters": {
        "rules": {
          "values": [
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 2
                },
                "conditions": [
                  {
                    "leftValue": "={{ $item(\"0\").$node[\"listar a run\"].json[\"data\"][\"0\"][\"status\"] }}",
                    "rightValue": "queued",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    },
                    "id": "40de457c-4bea-4960-b39a-826a8cb893f4"
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "queued"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 2
                },
                "conditions": [
                  {
                    "id": "fda005cf-e2bb-468f-a07d-2daf5d80e280",
                    "leftValue": "={{ $item(\"0\").$node[\"listar a run\"].json[\"data\"][\"0\"][\"status\"] }}",
                    "rightValue": "completed",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "completed"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 2
                },
                "conditions": [
                  {
                    "id": "3ed89312-bda7-422f-a970-918b4d22c1a3",
                    "leftValue": "={{ $item(\"0\").$node[\"listar a run\"].json[\"data\"][\"0\"][\"status\"] }}",
                    "rightValue": "in_progress",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "in_progress"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 2
                },
                "conditions": [
                  {
                    "id": "b24dd471-bb95-4c34-b9c9-ed3689a04b7e",
                    "leftValue": "={{ $item(\"0\").$node[\"listar a run\"].json[\"data\"][\"0\"][\"status\"] }}",
                    "rightValue": "requires_action",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "requires_action"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 2
                },
                "conditions": [
                  {
                    "id": "d71ebd8d-37e4-4a36-8caa-4563d731b185",
                    "leftValue": "={{ $item(\"0\").$node[\"listar a run\"].json[\"data\"][\"0\"][\"status\"] }}",
                    "rightValue": "cancelling",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "cancelling"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 2
                },
                "conditions": [
                  {
                    "id": "b9d1b3bc-06b8-4f87-8f66-a8d4427e248f",
                    "leftValue": "={{ $item(\"0\").$node[\"listar a run\"].json[\"data\"][\"0\"][\"status\"] }}",
                    "rightValue": "cancelled",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "cancelled"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 2
                },
                "conditions": [
                  {
                    "id": "e32e352f-47f6-4ec6-b85e-f6214129c67c",
                    "leftValue": "={{ $item(\"0\").$node[\"listar a run\"].json[\"data\"][\"0\"][\"status\"] }}",
                    "rightValue": "failed",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "failed"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 2
                },
                "conditions": [
                  {
                    "id": "4cc189d0-1729-4a03-85a5-2483161c9294",
                    "leftValue": "={{ $item(\"0\").$node[\"listar a run\"].json[\"data\"][\"0\"][\"status\"] }}",
                    "rightValue": "expired",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "expired"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.switch",
      "typeVersion": 3.2,
      "position": [
        4160,
        -400
      ],
      "id": "380affa8-cc2d-4c94-a309-a88464d5dc5c",
      "name": "verificar status da run"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.wait",
      "typeVersion": 1.1,
      "position": [
        4960,
        -40
      ],
      "id": "69010301-d8ba-4850-943c-814e8fad2241",
      "name": "Wait1",
      "webhookId": "51f99de9-918b-4a50-88f0-9764fb1afb69"
    },
    {
      "parameters": {
        "url": "=https://api.openai.com/v1/threads/{{ $item(\"0\").$node[\"verifica se existe thread\"].json[\"thread_id\"] }}/messages?order=desc",
        "authentication": "predefinedCredentialType",
        "nodeCredentialType": "openAiApi",
        "sendHeaders": true,
        "headerParameters": {
          "parameters": [
            {
              "name": "OpenAI-Beta",
              "value": "assistants=v2"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        4640,
        -380
      ],
      "id": "b9d362a4-1a2d-4b5a-ac89-fe2e50dc4476",
      "name": "pega resposta do assistente",
      "credentials": {
        "openAiApi": {
          "id": "b88tHuYNk7kQOY7B",
          "name": "OpenAi account 2"
        }
      }
    },
    {
      "parameters": {
        "options": {}
      },
      "type": "n8n-nodes-base.splitInBatches",
      "typeVersion": 3,
      "position": [
        5700,
        -380
      ],
      "id": "ce6334df-694b-4222-b6cb-fb352281cf15",
      "name": "Loop Over Items"
    },
    {
      "parameters": {
        "method": "POST",
        "url": "={{ $item(\"0\").$node[\"Webhook\"].json[\"body\"][\"server_url\"] }}/message/sendText/{{ $item(\"0\").$node[\"Webhook\"].json[\"body\"][\"instance\"] }}",
        "sendHeaders": true,
        "headerParameters": {
          "parameters": [
            {
              "name": "apikey",
              "value": "={{ $item(\"0\").$node[\"Webhook\"].json[\"body\"][\"apikey\"] }}"
            }
          ]
        },
        "sendBody": true,
        "bodyParameters": {
          "parameters": [
            {
              "name": "number",
              "value": "={{ $item(\"0\").$node[\"Webhook\"].json[\"body\"][\"data\"][\"key\"][\"remoteJid\"] }}"
            },
            {
              "name": "text",
              "value": "={{ $item(\"0\").$node[\"Loop Over Items\"].json[\"message\"] }}"
            },
            {
              "name": "2",
              "value": " "
            },
            {
              "name": "3",
              "value": " "
            },
            {
              "name": "4",
              "value": " "
            },
            {
              "name": "5",
              "value": " "
            },
            {
              "name": "6",
              "value": "\""
            },
            {
              "name": "7",
              "value": "n"
            },
            {
              "name": "8",
              "value": "u"
            },
            {
              "name": "9",
              "value": "m"
            },
            {
              "name": "10",
              "value": "b"
            },
            {
              "name": "11",
              "value": "e"
            },
            {
              "name": "12",
              "value": "r"
            },
            {
              "name": "13",
              "value": "\""
            },
            {
              "name": "14",
              "value": ":"
            },
            {
              "name": "15",
              "value": " "
            },
            {
              "name": "16",
              "value": "\""
            },
            {
              "name": "17",
              "value": "5"
            },
            {
              "name": "18",
              "value": "5"
            },
            {
              "name": "19",
              "value": "9"
            },
            {
              "name": "20",
              "value": "9"
            },
            {
              "name": "21",
              "value": "9"
            },
            {
              "name": "22",
              "value": "9"
            },
            {
              "name": "23",
              "value": "9"
            },
            {
              "name": "24",
              "value": "9"
            },
            {
              "name": "25",
              "value": "9"
            },
            {
              "name": "26",
              "value": "9"
            },
            {
              "name": "27",
              "value": "9"
            },
            {
              "name": "28",
              "value": "9"
            },
            {
              "name": "29",
              "value": " "
            },
            {
              "name": "30",
              "value": "("
            },
            {
              "name": "31",
              "value": "R"
            },
            {
              "name": "32",
              "value": "e"
            },
            {
              "name": "33",
              "value": "c"
            },
            {
              "name": "34",
              "value": "i"
            },
            {
              "name": "35",
              "value": "p"
            },
            {
              "name": "36",
              "value": "i"
            },
            {
              "name": "37",
              "value": "e"
            },
            {
              "name": "38",
              "value": "n"
            },
            {
              "name": "39",
              "value": "t"
            },
            {
              "name": "40",
              "value": " "
            },
            {
              "name": "41",
              "value": "n"
            },
            {
              "name": "42",
              "value": "u"
            },
            {
              "name": "43",
              "value": "m"
            },
            {
              "name": "44",
              "value": "b"
            },
            {
              "name": "45",
              "value": "e"
            },
            {
              "name": "46",
              "value": "r"
            },
            {
              "name": "47",
              "value": " "
            },
            {
              "name": "48",
              "value": "w"
            },
            {
              "name": "49",
              "value": "i"
            },
            {
              "name": "50",
              "value": "t"
            },
            {
              "name": "51",
              "value": "h"
            },
            {
              "name": "52",
              "value": " "
            },
            {
              "name": "53",
              "value": "C"
            },
            {
              "name": "54",
              "value": "o"
            },
            {
              "name": "55",
              "value": "u"
            },
            {
              "name": "56",
              "value": "n"
            },
            {
              "name": "57",
              "value": "t"
            },
            {
              "name": "58",
              "value": "r"
            },
            {
              "name": "59",
              "value": "y"
            },
            {
              "name": "60",
              "value": " "
            },
            {
              "name": "61",
              "value": "C"
            },
            {
              "name": "62",
              "value": "o"
            },
            {
              "name": "63",
              "value": "d"
            },
            {
              "name": "64",
              "value": "e"
            },
            {
              "name": "65",
              "value": ")"
            },
            {
              "name": "66",
              "value": "\""
            },
            {
              "name": "67",
              "value": ","
            },
            {
              "name": "68",
              "value": "\n"
            },
            {
              "name": "69",
              "value": " "
            },
            {
              "name": "70",
              "value": " "
            },
            {
              "name": "71",
              "value": " "
            },
            {
              "name": "72",
              "value": " "
            },
            {
              "name": "73",
              "value": "\""
            },
            {
              "name": "74",
              "value": "t"
            },
            {
              "name": "75",
              "value": "e"
            },
            {
              "name": "76",
              "value": "x"
            },
            {
              "name": "77",
              "value": "t"
            },
            {
              "name": "78",
              "value": "\""
            },
            {
              "name": "79",
              "value": ":"
            },
            {
              "name": "80",
              "value": " "
            },
            {
              "name": "81",
              "value": "\""
            },
            {
              "name": "82",
              "value": "t"
            },
            {
              "name": "83",
              "value": "e"
            },
            {
              "name": "84",
              "value": "s"
            },
            {
              "name": "85",
              "value": "t"
            },
            {
              "name": "86",
              "value": "e"
            },
            {
              "name": "87",
              "value": " "
            },
            {
              "name": "88",
              "value": "d"
            },
            {
              "name": "89",
              "value": "e"
            },
            {
              "name": "90",
              "value": " "
            },
            {
              "name": "91",
              "value": "e"
            },
            {
              "name": "92",
              "value": "n"
            },
            {
              "name": "93",
              "value": "v"
            },
            {
              "name": "94",
              "value": "i"
            },
            {
              "name": "95",
              "value": "o"
            },
            {
              "name": "96",
              "value": "\""
            },
            {
              "name": "97",
              "value": "\n"
            },
            {
              "name": "98",
              "value": " "
            },
            {
              "name": "99",
              "value": " "
            },
            {
              "name": "100",
              "value": " "
            },
            {
              "name": "101",
              "value": " "
            },
            {
              "name": "102",
              "value": "\n"
            },
            {
              "name": "103",
              "value": " "
            },
            {
              "name": "104",
              "value": " "
            },
            {
              "name": "105",
              "value": " "
            },
            {
              "name": "106",
              "value": " "
            },
            {
              "name": "107",
              "value": "\n"
            },
            {
              "name": "108",
              "value": " "
            },
            {
              "name": "109",
              "value": " "
            },
            {
              "name": "110",
              "value": " "
            },
            {
              "name": "111",
              "value": " "
            },
            {
              "name": "112",
              "value": "\n"
            },
            {
              "name": "113",
              "value": " "
            },
            {
              "name": "114",
              "value": " "
            },
            {
              "name": "115",
              "value": " "
            },
            {
              "name": "116",
              "value": " "
            },
            {
              "name": "117",
              "value": "\n"
            },
            {
              "name": "118",
              "value": " "
            },
            {
              "name": "119",
              "value": " "
            },
            {
              "name": "120",
              "value": " "
            },
            {
              "name": "121",
              "value": " "
            },
            {
              "name": "122",
              "value": "\n"
            },
            {
              "name": "123",
              "value": " "
            },
            {
              "name": "124",
              "value": " "
            },
            {
              "name": "125",
              "value": " "
            },
            {
              "name": "126",
              "value": " "
            },
            {
              "name": "127",
              "value": "\n"
            },
            {
              "name": "128",
              "value": " "
            },
            {
              "name": "129",
              "value": " "
            },
            {
              "name": "130",
              "value": " "
            },
            {
              "name": "131",
              "value": " "
            },
            {
              "name": "132",
              "value": "\n"
            },
            {
              "name": "133",
              "value": " "
            },
            {
              "name": "134",
              "value": " "
            },
            {
              "name": "135",
              "value": " "
            },
            {
              "name": "136",
              "value": " "
            },
            {
              "name": "137",
              "value": "\n"
            },
            {
              "name": "138",
              "value": " "
            },
            {
              "name": "139",
              "value": " "
            },
            {
              "name": "140",
              "value": " "
            },
            {
              "name": "141",
              "value": " "
            },
            {
              "name": "142",
              "value": "\n"
            },
            {
              "name": "143",
              "value": " "
            },
            {
              "name": "144",
              "value": " "
            },
            {
              "name": "145",
              "value": " "
            },
            {
              "name": "146",
              "value": " "
            },
            {
              "name": "147",
              "value": "\n"
            },
            {
              "name": "148",
              "value": " "
            },
            {
              "name": "149",
              "value": " "
            },
            {
              "name": "150",
              "value": " "
            },
            {
              "name": "151",
              "value": " "
            },
            {
              "name": "152",
              "value": "\n"
            },
            {
              "name": "153",
              "value": " "
            },
            {
              "name": "154",
              "value": " "
            },
            {
              "name": "155",
              "value": " "
            },
            {
              "name": "156",
              "value": " "
            },
            {
              "name": "157",
              "value": "\n"
            },
            {
              "name": "158",
              "value": " "
            },
            {
              "name": "159",
              "value": " "
            },
            {
              "name": "160",
              "value": " "
            },
            {
              "name": "161",
              "value": " "
            },
            {
              "name": "162",
              "value": "\n"
            },
            {
              "name": "163",
              "value": " "
            },
            {
              "name": "164",
              "value": " "
            },
            {
              "name": "165",
              "value": " "
            },
            {
              "name": "166",
              "value": " "
            },
            {
              "name": "167",
              "value": "\n"
            },
            {
              "name": "168",
              "value": " "
            },
            {
              "name": "169",
              "value": " "
            },
            {
              "name": "170",
              "value": " "
            },
            {
              "name": "171",
              "value": " "
            },
            {
              "name": "172",
              "value": "\n"
            },
            {
              "name": "173",
              "value": " "
            },
            {
              "name": "174",
              "value": " "
            },
            {
              "name": "175",
              "value": " "
            },
            {
              "name": "176",
              "value": " "
            },
            {
              "name": "177",
              "value": "\n"
            },
            {
              "name": "178",
              "value": "}"
            }
          ]
        },
        "options": {
          "redirect": {
            "redirect": {}
          }
        }
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        6240,
        -340
      ],
      "id": "f0d26eb4-4322-4cf4-8876-127f255131ad",
      "name": "HTTP Request"
    },
    {
      "parameters": {
        "amount": 1
      },
      "type": "n8n-nodes-base.wait",
      "typeVersion": 1.1,
      "position": [
        6500,
        -320
      ],
      "id": "2590ab94-d57a-4bee-be79-e57b1d45009a",
      "name": "Wait2",
      "webhookId": "38f68cb9-78a3-4c0b-a28d-bcb3738c6535"
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "={{ $json.data }}",
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.agent",
      "typeVersion": 1.9,
      "position": [
        5700,
        -1080
      ],
      "id": "c7663407-4d53-4f89-9882-8254957b293a",
      "name": "AI Agent"
    },
    {
      "parameters": {
        "model": {
          "__rl": true,
          "mode": "list",
          "value": "gpt-4o-mini"
        },
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatOpenAi",
      "typeVersion": 1.2,
      "position": [
        5760,
        -860
      ],
      "id": "4b953be1-d056-4190-9c2f-550f102019c3",
      "name": "OpenAI Chat Model",
      "credentials": {
        "openAiApi": {
          "id": "b88tHuYNk7kQOY7B",
          "name": "OpenAi account 2"
        }
      }
    },
    {
      "parameters": {
        "folderId": "default",
        "title": "teste"
      },
      "type": "n8n-nodes-base.googleDocs",
      "typeVersion": 2,
      "position": [
        6120,
        -1020
      ],
      "id": "9a924f66-6ed3-4f6a-a0da-df622be16d9b",
      "name": "Google Docs",
      "credentials": {
        "googleDocsOAuth2Api": {
          "id": "vdoWvQLzpclSzKB1",
          "name": "Google Docs account"
        }
      }
    },
    {
      "parameters": {
        "operation": "update",
        "documentURL": "={{ $item(\"0\").$node[\"Google Docs\"].json[\"id\"] }}",
        "actionsUi": {
          "actionFields": [
            {
              "action": "insert",
              "text": "={{ $item(\"0\").$node[\"AI Agent\"].json[\"output\"] }}"
            }
          ]
        }
      },
      "type": "n8n-nodes-base.googleDocs",
      "typeVersion": 2,
      "position": [
        6300,
        -1020
      ],
      "id": "0b53b2da-037a-4f54-9182-c9aad51b2414",
      "name": "Google Docs1",
      "credentials": {
        "googleDocsOAuth2Api": {
          "id": "vdoWvQLzpclSzKB1",
          "name": "Google Docs account"
        }
      }
    },
    {
      "parameters": {
        "operation": "download",
        "fileId": {
          "__rl": true,
          "value": "={{ $item(\"0\").$node[\"Google Docs1\"].json[\"documentId\"] }}",
          "mode": "id"
        },
        "options": {
          "googleFileConversion": {
            "conversion": {
              "docsToFormat": "application/pdf"
            }
          },
          "fileName": "teste.pdf"
        }
      },
      "type": "n8n-nodes-base.googleDrive",
      "typeVersion": 3,
      "position": [
        6480,
        -1020
      ],
      "id": "3aa0d6ef-7419-49cf-b963-672ed81dc1f0",
      "name": "Google Drive",
      "credentials": {
        "googleDriveOAuth2Api": {
          "id": "IgedJfDQR8L8CUnp",
          "name": "Google Drive account"
        }
      }
    },
    {
      "parameters": {
        "operation": "binaryToPropery",
        "options": {}
      },
      "type": "n8n-nodes-base.extractFromFile",
      "typeVersion": 1,
      "position": [
        6680,
        -1020
      ],
      "id": "61fbaa8c-0156-4ee1-9834-2ca5af58365c",
      "name": "Extract from File"
    },
    {
      "parameters": {
        "method": "POST",
        "url": "=https://webarcas-evolution-api.shrczq.easypanel.host/message/sendMedia/web_marcas",
        "sendHeaders": true,
        "headerParameters": {
          "parameters": [
            {
              "name": "apikey",
              "value": "2866B1DA3D90-4FD6-BB41-2CC53DB2CF62"
            }
          ]
        },
        "sendBody": true,
        "specifyBody": "json",
        "jsonBody": "={\n    \"number\": \"5511985014658\",\n    \"mediatype\": \"document\",\n    \"mimetype\": \"document/pdf\",\n    \"caption\": \"Teste de caption\",\n    \"media\": \"{{ $item(\"0\").$node[\"Extract from File\"].json[\"data\"] }}\",\n    \"fileName\": \"Imagem.pdf\"\n}",
        "options": {
          "redirect": {
            "redirect": {}
          }
        }
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        6880,
        -1020
      ],
      "id": "ac04f8f3-b982-4cff-b1fa-cb6c5ef4531c",
      "name": "envia o pdf para o whats"
    },
    {
      "parameters": {
        "rules": {
          "values": [
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 2
                },
                "conditions": [
                  {
                    "leftValue": "={{ $json.message }}",
                    "rightValue": "=",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    },
                    "id": "6594dc4a-dcaf-44e5-9fed-a349b133e3d8"
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "áudio"
            }
          ]
        },
        "options": {
          "fallbackOutput": "extra"
        }
      },
      "type": "n8n-nodes-base.switch",
      "typeVersion": 3.2,
      "position": [
        5940,
        -360
      ],
      "id": "18d5f881-36e1-46fe-8607-72b8f9e513ff",
      "name": "roteamento de resposta"
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "a7fa8f0f-cf18-4b26-809f-bde9a3438d4e",
              "name": "=tool calls",
              "value": "={{ $json.data[0].required_action.submit_tool_outputs.tool_calls }}",
              "type": "array"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        4340,
        -580
      ],
      "id": "1a9c363b-1738-4477-a4b7-80f2d017a7a8",
      "name": "isola o item do array"
    },
    {
      "parameters": {
        "operation": "delete",
        "tableId": "webmarcas",
        "filters": {
          "conditions": [
            {
              "keyName": "remotejid",
              "condition": "eq"
            }
          ]
        }
      },
      "type": "n8n-nodes-base.supabase",
      "typeVersion": 1,
      "position": [
        3820,
        -1160
      ],
      "id": "30006660-28cb-4ec5-a792-3c1231e6182f",
      "name": "Supabase",
      "credentials": {
        "supabaseApi": {
          "id": "XJVnZZX4is1jdDvh",
          "name": "Supabase WebMarcas"
        }
      }
    },
    {
      "parameters": {
        "fieldToSplitOut": "content",
        "options": {}
      },
      "type": "n8n-nodes-base.splitOut",
      "typeVersion": 1,
      "position": [
        5500,
        -380
      ],
      "id": "6a01fa27-2f82-4c61-8604-c1c3140a96d8",
      "name": "Split Out"
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "4763f642-38d0-497f-85e7-eb9be34d45ca",
              "name": "content",
              "value": "={{ $json.message.content }}",
              "type": "array"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        5260,
        -380
      ],
      "id": "b2ef3390-5920-4521-a3a9-5614e3007714",
      "name": "Edit Fields"
    },
    {
      "parameters": {
        "modelId": {
          "__rl": true,
          "value": "gpt-4o-mini-2024-07-18",
          "mode": "list",
          "cachedResultName": "GPT-4O-MINI-2024-07-18"
        },
        "messages": {
          "values": [
            {
              "content": "Você é um agente de IA que irá receber uma mensagem do usuário. Seu único objetivo é:\n\n1. Fracionar a mensagem em partes distintas, cada parte contendo **no máximo 150 tokens**.\n2. **Preservar exatamente** toda a estrutura, pontuação, palavras e caracteres originais.\n3. **Não** alterar a forma como o texto está escrito, mantendo todas as palavras, caracteres especiais, pontuações, quebras de linha, etc.\n4. Todas as frases devem **iniciar com letra maiúscula** (sem alterar o restante do texto).\n5. Sempre falhe uma linha entre as respostas para ficar visualmente apresentável \n6. A resposta final deve ser um **JSON** contendo um array de objetos, onde cada objeto representa uma parte fracionada do texto.\n\n\n### **Formato de saída (exemplo)**:\n[\n {\n   \"parte\": 1,\n   \"message\": \"Primeira parte da mensagem com até 150 tokens...\"\n },\n {\n   \"parte\": 2,\n   \"message\": \"Segunda parte da mensagem com até 150 tokens...\"\n },\n {\n   \"parte\": 3,\n   \"message\": \"Terceira parte da mensagem com até 150 tokens...\"\n }\n]",
              "role": "system"
            },
            {
              "content": "={{ $item(\"0\").$node[\"pega resposta do assistente\"].json[\"data\"][\"0\"][\"content\"][\"0\"][\"text\"][\"value\"] }}"
            }
          ]
        },
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.openAi",
      "typeVersion": 1.8,
      "position": [
        4880,
        -380
      ],
      "id": "dd3c195a-009d-47d4-b7d5-08bd13af84c5",
      "name": "fraciona a resposta1",
      "credentials": {
        "openAiApi": {
          "id": "b88tHuYNk7kQOY7B",
          "name": "OpenAi account 2"
        }
      }
    },
    {
      "parameters": {
        "functionCode": "const html = $json;\nif (html.includes(\"Resultado da busca\")) {\n  if (html.includes(\"Não foram encontrados resultados\")) {\n    return [{ colidencia: false, resultado: \"Nenhuma marca colidente encontrada no INPI.\" }];\n  } else {\n    return [{ colidencia: true, resultado: \"Foram encontradas marcas colidentes no INPI.\" }];\n  }\n} else {\n  return [{ colidencia: null, resultado: \"Não foi possível interpretar o resultado da pesquisa.\" }];\n}"
      },
      "name": "Analisa resultado INPI",
      "type": "n8n-nodes-base.function",
      "typeVersion": 1,
      "position": [
        5060,
        -1160
      ],
      "id": "9ed4dd9c-3ecc-4278-a835-a386f9a74180"
    },
    {
      "parameters": {
        "modelId": {
          "__rl": true,
          "value": "gpt-4o-mini-2024-07-18",
          "mode": "list",
          "cachedResultName": "GPT-4O-MINI-2024-07-18"
        },
        "messages": {
          "values": [
            {
              "content": "Você é um especialista em propriedade intelectual. Gere um Laudo Técnico de Viabilidade com base nos dados reais abaixo, indicando se é recomendável registrar a marca, e explicando brevemente se houve colidência e quais cuidados o cliente deve tomar. Use linguagem formal, consultiva e com parecer claro.",
              "role": "system"
            },
            {
              "content": "Marca: {{ $json.marca }}\nRamo de atividade: {{ $json.ramo }}\nResultado da pesquisa no INPI: {{ $json.resultado }}"
            }
          ]
        },
        "options": {}
      },
      "name": "Gera Laudo com base real",
      "type": "@n8n/n8n-nodes-langchain.openAi",
      "typeVersion": 1,
      "position": [
        4040,
        -1860
      ],
      "id": "b4db355c-f8e7-44ec-8b06-6bb1f210052a",
      "credentials": {
        "openAiApi": {
          "id": "b88tHuYNk7kQOY7B",
          "name": "OpenAi account 2"
        }
      }
    },
    {
      "parameters": {
        "options": {}
      },
      "name": "Prepara conteúdo do laudo",
      "type": "n8n-nodes-base.set",
      "typeVersion": 3,
      "position": [
        4380,
        -1860
      ],
      "id": "0dd39fa9-3176-43d6-a2ed-1fea9800e830"
    },
    {
      "parameters": {
        "fields": {
          "values": [
            {
              "name": "tool calls",
              "stringValue": "={{ $json[\"tool calls\"][0].id }}"
            },
            {
              "name": "nome_marca",
              "stringValue": "={{ JSON.parse($json[\"tool calls\"][0].function.arguments).nome_marca }}"
            },
            {
              "name": "ramo_atividade\t",
              "stringValue": "={{ JSON.parse($json[\"tool calls\"][0].function.arguments).ramo_atividade }}"
            }
          ]
        },
        "options": {}
      },
      "name": "Extrai dados do tool_call",
      "type": "n8n-nodes-base.set",
      "typeVersion": 3,
      "position": [
        4580,
        -680
      ],
      "id": "31dc4855-cfb8-4b06-8f20-be9e178bdb0a"
    },
    {
      "parameters": {
        "method": "POST",
        "url": "=https://api.openai.com/v1/threads/{{ $json.thread_id }}/runs/{{ $json.run_id }}/submit_tool_outputs",
        "authentication": "predefinedCredentialType",
        "nodeCredentialType": "openAiApi",
        "sendHeaders": true,
        "headerParameters": {
          "parameters": [
            {
              "name": "OpenAI-Beta",
              "value": "assistants=v2"
            },
            {
              "name": "Content-Type",
              "value": "application/json"
            }
          ]
        },
        "sendBody": true,
        "bodyParameters": {
          "parameters": [
            {}
          ]
        },
        "options": {}
      },
      "name": "Responder tool_call (submit_tool_outputs)",
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4,
      "position": [
        5380,
        -700
      ],
      "id": "b2229360-3980-40d5-916d-06fb91dfc346",
      "credentials": {
        "openAiApi": {
          "id": "b88tHuYNk7kQOY7B",
          "name": "OpenAi account 2"
        }
      }
    },
    {
      "parameters": {
        "options": {}
      },
      "name": "Seta dados para laudo",
      "type": "n8n-nodes-base.set",
      "typeVersion": 3,
      "position": [
        4800,
        -700
      ],
      "id": "5604d729-754a-4c4d-a652-9436a75db76b"
    },
    {
      "parameters": {
        "modelId": {
          "__rl": true,
          "value": "gpt-4o-mini-2024-07-18",
          "mode": "list",
          "cachedResultName": "GPT-4O-MINI-2024-07-18"
        },
        "messages": {
          "values": [
            {
              "content": "Você é um analista técnico especialista em marcas e patentes. Sua função é gerar um Laudo Técnico de Viabilidade de Marca com base em informações fornecidas.\n\nA estrutura do laudo deve seguir exatamente o modelo abaixo, preenchendo com os dados reais:\n\n# Laudo Técnico de Viabilidade de Marca\n\n**1. Nome da Marca:**\n{{nome_marca}}\n\n**2. Ramo de Atividade:**\n{{ramo_atividade}}\n\n**3. Data da Análise:**\n{{data}}\n\n**4. Resultado da Pesquisa no INPI:**\n{{resultado_inpi}}\n\n**5. Pesquisa de Colidência na Internet no Brasil:**\n{{colidencia_internet}}\n\n**6. Classes Sugeridas para Registro:**\n- Classe {{classe1_numero}} – {{classe1_descricao}}\n- Classe {{classe2_numero}} – {{classe2_descricao}}\n\n**7. Conclusão:**\nA marca '{{nome_marca}}' apresenta {{conclusao}}\n\n---\n**Importante:**\nEste laudo é uma análise técnica preliminar. A decisão final depende da análise do INPI.\n\n**Orientação Jurídica:**\nO ideal é registrar a marca nas classes recomendadas para garantir proteção ampla. Caso haja limitação financeira, registre ao menos na classe principal.\n\n❗Use sempre os dados fornecidos e não invente ou simule informações.\n❗Mantenha o formato exato, com quebras de linha e os títulos conforme o modelo.",
              "role": "system"
            }
          ]
        },
        "options": {}
      },
      "name": "Gerar laudo formatado (OpenAI)",
      "type": "@n8n/n8n-nodes-langchain.openAi",
      "typeVersion": 1,
      "position": [
        5000,
        -700
      ],
      "id": "7bb14737-9806-48c5-80f6-aabf425ae47d",
      "credentials": {
        "openAiApi": {
          "id": "b88tHuYNk7kQOY7B",
          "name": "OpenAi account 2"
        }
      }
    }
  ],
  "pinData": {
    "Webhook": [
      {
        "json": {
          "headers": {
            "host": "webarcas-n8n.shrczq.easypanel.host",
            "user-agent": "axios/1.7.9",
            "content-length": "939",
            "accept-encoding": "gzip, compress, deflate, br",
            "content-type": "application/json",
            "x-forwarded-for": "172.18.0.1",
            "x-forwarded-host": "webarcas-n8n.shrczq.easypanel.host",
            "x-forwarded-port": "443",
            "x-forwarded-proto": "https",
            "x-forwarded-server": "b91c477977b3",
            "x-real-ip": "172.18.0.1"
          },
          "params": {},
          "query": {},
          "body": {
            "event": "messages.upsert",
            "instance": "web_marcas",
            "data": {
              "key": {
                "remoteJid": "5511985014658@s.whatsapp.net",
                "fromMe": false,
                "id": "3ACC716AA33459EA988E"
              },
              "pushName": "Henrique",
              "status": "DELIVERY_ACK",
              "message": {
                "conversation": "pode mandar por aqui mesmo",
                "messageContextInfo": {
                  "deviceListMetadata": {
                    "senderKeyHash": "h03N9CVkdboing==",
                    "senderTimestamp": "1746552820",
                    "recipientKeyHash": "4T8/kq0yGI+A9w==",
                    "recipientTimestamp": "1747417551"
                  },
                  "deviceListMetadataVersion": 2,
                  "messageSecret": "tGq07335wycawszvNLMLWk695T5ynlcbpMneIiAJK10="
                }
              },
              "messageType": "conversation",
              "messageTimestamp": 1747616887,
              "instanceId": "4c1c17a4-f3d3-40c9-8e5b-245ea83d966f",
              "source": "ios"
            },
            "destination": "https://webarcas-n8n.shrczq.easypanel.host/webhook-test/55a12fe0-0ac3-4a01-89c4-f6ab5ae3a08f",
            "date_time": "2025-05-18T22:08:08.066Z",
            "sender": "5513981624853@s.whatsapp.net",
            "server_url": "https://webarcas-evolution-api.shrczq.easypanel.host",
            "apikey": "2866B1DA3D90-4FD6-BB41-2CC53DB2CF62"
          },
          "webhookUrl": "https://webarcas-n8n.shrczq.easypanel.host/webhook-test/55a12fe0-0ac3-4a01-89c4-f6ab5ae3a08f",
          "executionMode": "test"
        }
      }
    ]
  },
  "connections": {
    "Webhook": {
      "main": [
        [
          {
            "node": "Roteamento de mensagens recebidas",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Roteamento de mensagens recebidas": {
      "main": [
        [
          {
            "node": "setando mensagem (conversation)",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "setando mensagem (extendedTextMessage)",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "setando base 64",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "setando base 64": {
      "main": [
        [
          {
            "node": "convertendo o base 64",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "convertendo o base 64": {
      "main": [
        [
          {
            "node": "OpenAI",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "OpenAI": {
      "main": [
        [
          {
            "node": "setando a mensagem audio",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "setando mensagem (conversation)": {
      "main": [
        [
          {
            "node": "node de referencia da mensagem",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "setando mensagem (extendedTextMessage)": {
      "main": [
        [
          {
            "node": "node de referencia da mensagem",
            "type": "main",
            "index": 1
          }
        ]
      ]
    },
    "setando a mensagem audio": {
      "main": [
        [
          {
            "node": "node de referencia da mensagem",
            "type": "main",
            "index": 2
          }
        ]
      ]
    },
    "node de referencia da mensagem": {
      "main": [
        [
          {
            "node": "varredura do BD filtrando pelo remotejid",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "varredura do BD filtrando pelo remotejid": {
      "main": [
        [
          {
            "node": "verifica se o lead existe",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "verifica se o lead existe": {
      "main": [
        [
          {
            "node": "Merge",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "cria o lead no bd",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "cria o lead no bd": {
      "main": [
        [
          {
            "node": "Merge",
            "type": "main",
            "index": 1
          }
        ]
      ]
    },
    "Merge": {
      "main": [
        [
          {
            "node": "verifica se existe thread",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "verifica se existe thread": {
      "main": [
        [
          {
            "node": "Criando MSG e Add na thread",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "criar thread",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "criar thread": {
      "main": [
        [
          {
            "node": "Atualiza thread no Bd",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Atualiza thread no Bd": {
      "main": [
        [
          {
            "node": "Merge",
            "type": "main",
            "index": 1
          }
        ]
      ]
    },
    "Criando MSG e Add na thread": {
      "main": [
        [
          {
            "node": "Wait",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Wait": {
      "main": [
        [
          {
            "node": "lista as mensagens",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "lista as mensagens": {
      "main": [
        [
          {
            "node": "verifica os ids das msg",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "verifica os ids das msg": {
      "main": [
        [
          {
            "node": "criar run",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "No Operation, do nothing1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "criar run": {
      "main": [
        [
          {
            "node": "listar a run",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "listar a run": {
      "main": [
        [
          {
            "node": "verificar status da run",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "verificar status da run": {
      "main": [
        [
          {
            "node": "Wait1",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "pega resposta do assistente",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Wait1",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "isola o item do array",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Wait1",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Wait1",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Wait1",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Wait1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Wait1": {
      "main": [
        [
          {
            "node": "listar a run",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "pega resposta do assistente": {
      "main": [
        [
          {
            "node": "fraciona a resposta1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Loop Over Items": {
      "main": [
        [],
        [
          {
            "node": "roteamento de resposta",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "HTTP Request": {
      "main": [
        [
          {
            "node": "Wait2",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Wait2": {
      "main": [
        [
          {
            "node": "Loop Over Items",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "OpenAI Chat Model": {
      "ai_languageModel": [
        [
          {
            "node": "AI Agent",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "AI Agent": {
      "main": [
        [
          {
            "node": "Google Docs",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Google Docs": {
      "main": [
        [
          {
            "node": "Google Docs1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Google Docs1": {
      "main": [
        [
          {
            "node": "Google Drive",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Google Drive": {
      "main": [
        [
          {
            "node": "Extract from File",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Extract from File": {
      "main": [
        [
          {
            "node": "envia o pdf para o whats",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "roteamento de resposta": {
      "main": [
        [],
        [
          {
            "node": "HTTP Request",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "isola o item do array": {
      "main": [
        [
          {
            "node": "Extrai dados do tool_call",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Edit Fields": {
      "main": [
        [
          {
            "node": "Split Out",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "fraciona a resposta1": {
      "main": [
        [
          {
            "node": "Edit Fields",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Split Out": {
      "main": [
        [
          {
            "node": "Loop Over Items",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Analisa resultado INPI": {
      "main": [
        []
      ]
    },
    "Gera Laudo com base real": {
      "main": [
        [
          {
            "node": "Prepara conteúdo do laudo",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Extrai dados do tool_call": {
      "main": [
        [
          {
            "node": "Seta dados para laudo",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Seta dados para laudo": {
      "main": [
        [
          {
            "node": "Gerar laudo formatado (OpenAI)",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Gerar laudo formatado (OpenAI)": {
      "main": [
        [
          {
            "node": "Responder tool_call (submit_tool_outputs)",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": false,
  "settings": {
    "executionOrder": "v1"
  },
  "versionId": "6b92bebc-d762-4b2f-a6d7-5f6d3bab5705",
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "5e22cea8afbb92bbcf5cd757bc4df7ca25a0e28734f4247e9a6a17582c4f3f92"
  },
  "id": "gkqvASghoudpjY6E",
  "tags": []
}
