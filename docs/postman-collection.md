# Postman collection — GameHunter CRUD

Simpan JSON berikut sebagai `GameHunter_Minggu_5.postman_collection.json`, lalu import ke Postman. Jalankan server lokal dan jalankan collection berurutan. Create Game menyimpan ID untuk update dan delete.

Collection berisi 8 request, 26 assertion, dan contoh response aktual. Memakai No Auth dan `http://127.0.0.1:8000`.

<details>
<summary>JSON collection lengkap</summary>

```json
{
  "info": {
    "name": "Minggu 5 - GameHunter CRUD",
    "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json",
    "description": "Praktikum 5: CRUD GameHunter. Jalankan berurutan: buat data contoh, validasi, PATCH, GET, DELETE, lalu pastikan ID yang dihapus menghasilkan 404. No Auth untuk server development lokal."
  },
  "auth": {
    "type": "noauth"
  },
  "variable": [
    {
      "key": "base_url",
      "value": "http://127.0.0.1:8000"
    },
    {
      "key": "crud_game_id",
      "value": ""
    }
  ],
  "item": [
    {
      "name": "List Games",
      "request": {
        "method": "GET",
        "header": [
          {
            "key": "Accept",
            "value": "application/json"
          },
          {
            "key": "Content-Type",
            "value": "application/json"
          }
        ],
        "url": "{{base_url}}/api/games"
      },
      "event": [
        {
          "listen": "test",
          "script": {
            "type": "text/javascript",
            "exec": [
              "pm.test('Status 200',()=>pm.response.to.have.status(200));",
              "pm.test('JSON response',()=>pm.expect(pm.response.headers.get('Content-Type')).to.include('application/json'));",
              "pm.test('List data array',()=>pm.expect(pm.response.json().data).to.be.an('array'));"
            ]
          }
        }
      ],
      "response": [
        {
          "name": "Actual Laravel 200",
          "originalRequest": {
            "method": "GET",
            "header": [
              {
                "key": "Accept",
                "value": "application/json"
              },
              {
                "key": "Content-Type",
                "value": "application/json"
              }
            ],
            "url": "{{base_url}}/api/games"
          },
          "status": "OK",
          "code": 200,
          "header": [
            {
              "key": "Content-Type",
              "value": "application/json"
            }
          ],
          "body": "{\n  \"data\": [\n    {\n      \"id\": 1,\n      \"title\": \"Game Contoh PC\",\n      \"platform\": \"pc\",\n      \"is_free\": true,\n      \"source\": \"fixture\",\n      \"updated_at\": \"2026-10-09T08:22:09.000000Z\"\n    },\n    {\n      \"id\": 2,\n      \"title\": \"Game Contoh Browser\",\n      \"platform\": \"browser\",\n      \"is_free\": true,\n      \"source\": \"fixture\",\n      \"updated_at\": \"2026-10-09T08:22:09.000000Z\"\n    }\n  ]\n}"
        }
      ]
    },
    {
      "name": "Create Game",
      "request": {
        "method": "POST",
        "header": [
          {
            "key": "Accept",
            "value": "application/json"
          },
          {
            "key": "Content-Type",
            "value": "application/json"
          }
        ],
        "url": "{{base_url}}/api/games",
        "body": {
          "mode": "raw",
          "raw": "{\n  \"title\": \"Game Praktikum CRUD\",\n  \"platform\": \"pc\",\n  \"is_free\": true\n}",
          "options": {
            "raw": {
              "language": "json"
            }
          }
        }
      },
      "event": [
        {
          "listen": "test",
          "script": {
            "type": "text/javascript",
            "exec": [
              "pm.test('Status 201',()=>pm.response.to.have.status(201));",
              "pm.test('JSON response',()=>pm.expect(pm.response.headers.get('Content-Type')).to.include('application/json'));",
              "pm.test('Contract Game',()=>{const g=pm.response.json().data;pm.expect(Object.keys(g).sort()).to.eql(['id','title','platform','is_free','source','updated_at'].sort());pm.expect(g.id).to.be.a('number');pm.expect(g.title).to.be.a('string');pm.expect(g.platform).to.be.oneOf(['pc','browser']);pm.expect(g.is_free).to.be.a('boolean');pm.expect(g.source).to.be.a('string');pm.expect(g.updated_at).to.match(/^\\d{4}-\\d{2}-\\d{2}T/);});",
              "pm.test('Created values',()=>{const g=pm.response.json().data;pm.expect(g.title).to.eql('Game Praktikum CRUD');pm.expect(g.platform).to.eql('pc');pm.expect(g.is_free).to.eql(true);pm.expect(g.source).to.eql('manual');pm.collectionVariables.set('crud_game_id',g.id);});"
            ]
          }
        }
      ],
      "response": [
        {
          "name": "Actual Laravel 201",
          "originalRequest": {
            "method": "POST",
            "header": [
              {
                "key": "Accept",
                "value": "application/json"
              },
              {
                "key": "Content-Type",
                "value": "application/json"
              }
            ],
            "url": "{{base_url}}/api/games",
            "body": {
              "mode": "raw",
              "raw": "{\n  \"title\": \"Game Praktikum CRUD\",\n  \"platform\": \"pc\",\n  \"is_free\": true\n}",
              "options": {
                "raw": {
                  "language": "json"
                }
              }
            }
          },
          "status": "Created",
          "code": 201,
          "header": [
            {
              "key": "Content-Type",
              "value": "application/json"
            }
          ],
          "body": "{\n  \"data\": {\n    \"id\": 3,\n    \"title\": \"Game Praktikum CRUD\",\n    \"platform\": \"pc\",\n    \"is_free\": true,\n    \"source\": \"manual\",\n    \"updated_at\": \"2026-10-12T09:53:18.000000Z\"\n  }\n}"
        }
      ]
    },
    {
      "name": "Validation Error",
      "request": {
        "method": "POST",
        "header": [
          {
            "key": "Accept",
            "value": "application/json"
          },
          {
            "key": "Content-Type",
            "value": "application/json"
          }
        ],
        "url": "{{base_url}}/api/games",
        "body": {
          "mode": "raw",
          "raw": "{\n  \"title\": \"\",\n  \"platform\": \"console\"\n}",
          "options": {
            "raw": {
              "language": "json"
            }
          }
        }
      },
      "event": [
        {
          "listen": "test",
          "script": {
            "type": "text/javascript",
            "exec": [
              "pm.test('Status 422',()=>pm.response.to.have.status(422));",
              "pm.test('JSON response',()=>pm.expect(pm.response.headers.get('Content-Type')).to.include('application/json'));",
              "pm.test('Validation fields',()=>{const e=pm.response.json().errors;pm.expect(e.title).to.eql(['The title field is required.']);pm.expect(e.platform).to.eql(['The selected platform is invalid.']);});"
            ]
          }
        }
      ],
      "response": [
        {
          "name": "Actual Laravel 422",
          "originalRequest": {
            "method": "POST",
            "header": [
              {
                "key": "Accept",
                "value": "application/json"
              },
              {
                "key": "Content-Type",
                "value": "application/json"
              }
            ],
            "url": "{{base_url}}/api/games",
            "body": {
              "mode": "raw",
              "raw": "{\n  \"title\": \"\",\n  \"platform\": \"console\"\n}",
              "options": {
                "raw": {
                  "language": "json"
                }
              }
            }
          },
          "status": "Unprocessable Content",
          "code": 422,
          "header": [
            {
              "key": "Content-Type",
              "value": "application/json"
            }
          ],
          "body": "{\n  \"message\": \"The title field is required. (and 1 more error)\",\n  \"errors\": {\n    \"title\": [\n      \"The title field is required.\"\n    ],\n    \"platform\": [\n      \"The selected platform is invalid.\"\n    ]\n  }\n}"
        }
      ]
    },
    {
      "name": "Patch Game",
      "request": {
        "method": "PATCH",
        "header": [
          {
            "key": "Accept",
            "value": "application/json"
          },
          {
            "key": "Content-Type",
            "value": "application/json"
          }
        ],
        "url": "{{base_url}}/api/games/{{crud_game_id}}",
        "body": {
          "mode": "raw",
          "raw": "{\n  \"is_free\": false\n}",
          "options": {
            "raw": {
              "language": "json"
            }
          }
        }
      },
      "event": [
        {
          "listen": "prerequest",
          "script": {
            "type": "text/javascript",
            "exec": [
              "if(!pm.collectionVariables.get('crud_game_id')) throw new Error('Jalankan Create Game dahulu.');"
            ]
          }
        },
        {
          "listen": "test",
          "script": {
            "type": "text/javascript",
            "exec": [
              "pm.test('Status 200',()=>pm.response.to.have.status(200));",
              "pm.test('JSON response',()=>pm.expect(pm.response.headers.get('Content-Type')).to.include('application/json'));",
              "pm.test('Contract Game',()=>{const g=pm.response.json().data;pm.expect(Object.keys(g).sort()).to.eql(['id','title','platform','is_free','source','updated_at'].sort());pm.expect(g.id).to.be.a('number');pm.expect(g.title).to.be.a('string');pm.expect(g.platform).to.be.oneOf(['pc','browser']);pm.expect(g.is_free).to.be.a('boolean');pm.expect(g.source).to.be.a('string');pm.expect(g.updated_at).to.match(/^\\d{4}-\\d{2}-\\d{2}T/);});",
              "pm.test('Partial update only',()=>{const g=pm.response.json().data;pm.expect(g.title).to.eql('Game Praktikum CRUD');pm.expect(g.platform).to.eql('pc');pm.expect(g.source).to.eql('manual');pm.expect(g.is_free).to.eql(false);});"
            ]
          }
        }
      ],
      "response": [
        {
          "name": "Actual Laravel 200",
          "originalRequest": {
            "method": "PATCH",
            "header": [
              {
                "key": "Accept",
                "value": "application/json"
              },
              {
                "key": "Content-Type",
                "value": "application/json"
              }
            ],
            "url": "{{base_url}}/api/games/{{crud_game_id}}",
            "body": {
              "mode": "raw",
              "raw": "{\n  \"is_free\": false\n}",
              "options": {
                "raw": {
                  "language": "json"
                }
              }
            }
          },
          "status": "OK",
          "code": 200,
          "header": [
            {
              "key": "Content-Type",
              "value": "application/json"
            }
          ],
          "body": "{\n  \"data\": {\n    \"id\": 3,\n    \"title\": \"Game Praktikum CRUD\",\n    \"platform\": \"pc\",\n    \"is_free\": false,\n    \"source\": \"manual\",\n    \"updated_at\": \"2026-10-12T09:53:18.000000Z\"\n  }\n}"
        }
      ]
    },
    {
      "name": "Show Game",
      "request": {
        "method": "GET",
        "header": [
          {
            "key": "Accept",
            "value": "application/json"
          },
          {
            "key": "Content-Type",
            "value": "application/json"
          }
        ],
        "url": "{{base_url}}/api/games/{{crud_game_id}}"
      },
      "event": [
        {
          "listen": "prerequest",
          "script": {
            "type": "text/javascript",
            "exec": [
              "if(!pm.collectionVariables.get('crud_game_id')) throw new Error('Jalankan Create Game dahulu.');"
            ]
          }
        },
        {
          "listen": "test",
          "script": {
            "type": "text/javascript",
            "exec": [
              "pm.test('Status 200',()=>pm.response.to.have.status(200));",
              "pm.test('JSON response',()=>pm.expect(pm.response.headers.get('Content-Type')).to.include('application/json'));",
              "pm.test('Contract Game',()=>{const g=pm.response.json().data;pm.expect(Object.keys(g).sort()).to.eql(['id','title','platform','is_free','source','updated_at'].sort());pm.expect(g.id).to.be.a('number');pm.expect(g.title).to.be.a('string');pm.expect(g.platform).to.be.oneOf(['pc','browser']);pm.expect(g.is_free).to.be.a('boolean');pm.expect(g.source).to.be.a('string');pm.expect(g.updated_at).to.match(/^\\d{4}-\\d{2}-\\d{2}T/);});",
              "pm.test('Persisted update',()=>pm.expect(pm.response.json().data.is_free).to.eql(false));"
            ]
          }
        }
      ],
      "response": [
        {
          "name": "Actual Laravel 200",
          "originalRequest": {
            "method": "GET",
            "header": [
              {
                "key": "Accept",
                "value": "application/json"
              },
              {
                "key": "Content-Type",
                "value": "application/json"
              }
            ],
            "url": "{{base_url}}/api/games/{{crud_game_id}}"
          },
          "status": "OK",
          "code": 200,
          "header": [
            {
              "key": "Content-Type",
              "value": "application/json"
            }
          ],
          "body": "{\n  \"data\": {\n    \"id\": 3,\n    \"title\": \"Game Praktikum CRUD\",\n    \"platform\": \"pc\",\n    \"is_free\": false,\n    \"source\": \"manual\",\n    \"updated_at\": \"2026-10-12T09:53:18.000000Z\"\n  }\n}"
        }
      ]
    },
    {
      "name": "Game Not Found",
      "request": {
        "method": "GET",
        "header": [
          {
            "key": "Accept",
            "value": "application/json"
          },
          {
            "key": "Content-Type",
            "value": "application/json"
          }
        ],
        "url": "{{base_url}}/api/games/999999"
      },
      "event": [
        {
          "listen": "test",
          "script": {
            "type": "text/javascript",
            "exec": [
              "pm.test('Status 404',()=>pm.response.to.have.status(404));",
              "pm.test('JSON response',()=>pm.expect(pm.response.headers.get('Content-Type')).to.include('application/json'));",
              "pm.test('404 contract',()=>pm.expect(pm.response.json()).to.eql({message:'Resource not found',errors:null}));"
            ]
          }
        }
      ],
      "response": [
        {
          "name": "Actual Laravel 404",
          "originalRequest": {
            "method": "GET",
            "header": [
              {
                "key": "Accept",
                "value": "application/json"
              },
              {
                "key": "Content-Type",
                "value": "application/json"
              }
            ],
            "url": "{{base_url}}/api/games/999999"
          },
          "status": "Not Found",
          "code": 404,
          "header": [
            {
              "key": "Content-Type",
              "value": "application/json"
            }
          ],
          "body": "{\n  \"message\": \"Resource not found\",\n  \"errors\": null\n}"
        }
      ]
    },
    {
      "name": "Delete Game",
      "request": {
        "method": "DELETE",
        "header": [
          {
            "key": "Accept",
            "value": "application/json"
          },
          {
            "key": "Content-Type",
            "value": "application/json"
          }
        ],
        "url": "{{base_url}}/api/games/{{crud_game_id}}"
      },
      "event": [
        {
          "listen": "prerequest",
          "script": {
            "type": "text/javascript",
            "exec": [
              "if(!pm.collectionVariables.get('crud_game_id')) throw new Error('Jalankan Create Game dahulu.');"
            ]
          }
        },
        {
          "listen": "test",
          "script": {
            "type": "text/javascript",
            "exec": [
              "pm.test('Status 204',()=>pm.response.to.have.status(204));",
              "pm.test('Empty response body',()=>pm.expect(pm.response.text()).to.eql(''));"
            ]
          }
        }
      ],
      "response": [
        {
          "name": "Actual Laravel 204",
          "originalRequest": {
            "method": "DELETE",
            "header": [
              {
                "key": "Accept",
                "value": "application/json"
              },
              {
                "key": "Content-Type",
                "value": "application/json"
              }
            ],
            "url": "{{base_url}}/api/games/{{crud_game_id}}"
          },
          "status": "No Content",
          "code": 204,
          "header": [],
          "body": ""
        }
      ]
    },
    {
      "name": "Verify Deleted",
      "request": {
        "method": "GET",
        "header": [
          {
            "key": "Accept",
            "value": "application/json"
          },
          {
            "key": "Content-Type",
            "value": "application/json"
          }
        ],
        "url": "{{base_url}}/api/games/{{crud_game_id}}"
      },
      "event": [
        {
          "listen": "prerequest",
          "script": {
            "type": "text/javascript",
            "exec": [
              "if(!pm.collectionVariables.get('crud_game_id')) throw new Error('Jalankan Create Game dahulu.');"
            ]
          }
        },
        {
          "listen": "test",
          "script": {
            "type": "text/javascript",
            "exec": [
              "pm.test('Status 404',()=>pm.response.to.have.status(404));",
              "pm.test('JSON response',()=>pm.expect(pm.response.headers.get('Content-Type')).to.include('application/json'));",
              "pm.test('Deleted ID returns 404',()=>pm.expect(pm.response.json()).to.eql({message:'Resource not found',errors:null}));"
            ]
          }
        }
      ],
      "response": [
        {
          "name": "Actual Laravel 404",
          "originalRequest": {
            "method": "GET",
            "header": [
              {
                "key": "Accept",
                "value": "application/json"
              },
              {
                "key": "Content-Type",
                "value": "application/json"
              }
            ],
            "url": "{{base_url}}/api/games/{{crud_game_id}}"
          },
          "status": "Not Found",
          "code": 404,
          "header": [
            {
              "key": "Content-Type",
              "value": "application/json"
            }
          ],
          "body": "{\n  \"message\": \"Resource not found\",\n  \"errors\": null\n}"
        }
      ]
    }
  ]
}
```

</details>
