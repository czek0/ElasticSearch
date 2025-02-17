# **Elasticsearch Support Repo** 🚀  

Welcome to the **Elasticsearch Support Repository**! This repo contains essential resources for enhancing **Dutch language support** in Elasticsearch, including **synonyms, custom stemming overrides, and useful reference materials**.  

## **📂 Repository Contents**  

### **1️⃣ Synonym File**  
📍 **Path:** `config/analysis/synonyms.txt`  
- Contains **custom synonym mappings** to improve search accuracy.  
- Elasticsearch uses this file to expand queries with equivalent terms.  
- Format:  
  ```txt
  fiets, rijwiel => fiets
  auto, wagen => auto
  ```

### 2️⃣ Dutch Stemmer Override File  
📍 **Path:** config/analysis/dutch_stem_override.txt  
- This file contains **custom stemming rules** for Dutch, specifically handling **irregular plurals and infinitives** that the default Snowball stemmer misses.  
- Helps ensure **more precise text analysis** and better search results.  
- Example entry:  
```
huizen => huis lopen => loop
```

### 3️⃣ Basics Page  
📍 **Path:** Basics.ipynb  
- A **beginner-friendly guide** to setting up and configuring **Elasticsearch analysis**.  

### 4️⃣ Elasticsearch Cheat Sheet  
📍 **Path:** ChearSheat.ipynb  
- A **quick reference** with commonly used **Elasticsearch queries, commands, and syntax**.  

---

## 📌 How to Use These Files in Elasticsearch  

### 🔹 Loading the Synonym & Stemmer Files  
Ensure your `elasticsearch.yml` is configured to recognize these files:  

```
index: analysis: filter: dutch_synonym_filter: type: synonym synonyms_path: "config/analysis/synonyms.txt" dutch_stem_override_filter: type: stemmer_override rules_path: "config/analysis/dutch_stem_override.txt"

```

### 🔹 Applying These Filters to an Analyzer  

```
PUT test
{
  "settings": {
    "refresh_interval": "1s",
    "analysis": {
      "filter": {
        "dutch_synonym" : {
            "type": "synonym",
            "synonyms_path": "dutch_synonyms_elasticsearch_10k.txt"
        },
        "dutch_stop": {
          "type": "stop",
          "stopwords": [
            "_dutch_"
          ]
        },
        "dutch_stemmer": {
          "type": "stemmer",
          "language": "dutch"
        },
        "custom_dutch_stem_override": {
        "type": "stemmer_override",
        "rules_path" : "dutch_stemmer_override.txt"
        }
      },
      "analyzer": {
        "nl": {
          "filter": [
            "lowercase",
            "custom_dutch_stem_override",
            "dutch_stop",
            "dutch_stemmer"
          ],
          "type": "custom",
          "tokenizer": "standard"
        }
      }
    },
    "number_of_shards": "1",
    "number_of_replicas": "0"
  }, 
  "mappings": {
    "properties": {
      "attachment": {
        "properties": {
          "author": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "content": {
            "type": "text",
            "analyzer": "nl"
          },
          "content_length": {
            "type": "long",
            "ignore_malformed": true
          },
          "content_type": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "date": {
            "type": "date",
            "ignore_malformed": true
          },
          "language": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "title": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          }
        }
      },
      "category": {
        "properties": {
          "description": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "id": {
            "type": "long",
            "ignore_malformed": true
          }
        }
      },
      "createdOn": {
        "type": "date",
        "ignore_malformed": true
      },
      "dossier": {
        "properties": {
          "id": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "name": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          }
        }
      },
      "dossierId": {
        "type": "text",
        "fields": {
          "keyword": {
            "type": "keyword",
            "ignore_above": 256
          }
        }
      },
      "dossierName": {
        "type": "text"
      },
      "filename": {
        "type": "text"
      },
      "id": {
        "type": "text",
        "fields": {
          "keyword": {
            "type": "keyword",
            "ignore_above": 256
          }
        }
      },
      "link": {
        "type": "text",
        "fields": {
          "keyword": {
            "type": "keyword",
            "ignore_above": 256
          }
        }
      },
      "publicationDate": {
        "type": "date",
        "ignore_malformed": true
      },
      "size": {
        "type": "long",
        "ignore_malformed": true
      }
    }
  }
}
```

---

## 👩‍💻 Contributing & Support  
- Feel free to **submit a pull request** for any improvements!  
- If you have **questions or need help**, open an **issue** in this repo.  




