# Documentation: conf/os_mapping.json

## File Metadata

- **Path**: `conf/os_mapping.json`
- **Size**: 5766 bytes
- **Type**: .json
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `conf/os_mapping.json`.

## Original Source Code

```json
{
  "settings": {
    "index": {
      "number_of_shards": 2,
      "number_of_replicas": 0,
      "refresh_interval": "1000ms",
      "knn": true,
      "similarity": {
        "scripted_sim": {
          "type": "scripted",
          "script": {
            "source": "double idf = Math.log(1+(field.docCount-term.docFreq+0.5)/(term.docFreq + 0.5))/Math.log(1+((field.docCount-0.5)/1.5)); return query.boost * idf * Math.min(doc.freq, 1);"
          }
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "lat_lon": {
        "type": "geo_point",
        "store": "true"
      }
    },
    "date_detection": "true",
    "dynamic_templates": [
      {
        "int": {
          "match": "*_int",
          "mapping": {
            "type": "integer",
            "store": "true"
          }
        }
      },
      {
        "ulong": {
          "match": "*_ulong",
          "mapping": {
            "type": "unsigned_long",
            "store": "true"
          }
        }
      },
      {
        "long": {
          "match": "*_long",
          "mapping": {
            "type": "long",
            "store": "true"
          }
        }
      },
      {
        "short": {
          "match": "*_short",
          "mapping": {
            "type": "short",
            "store": "true"
          }
        }
      },
      {
        "numeric": {
          "match": "*_flt",
          "mapping": {
            "type": "float",
            "store": true
          }
        }
      },
      {
        "tks": {
          "match": "*_tks",
          "mapping": {
            "type": "text",
            "similarity": "scripted_sim",
            "analyzer": "whitespace",
            "store": true
          }
        }
      },
      {
        "ltks": {
          "match": "*_ltks",
          "mapping": {
            "type": "text",
            "analyzer": "whitespace",
            "store": true
          }
        }
      },
      {
        "kwd": {
          "match_pattern": "regex",
          "match": "^(.*_(kwd|id|ids|uid|uids)|uid)$",
          "mapping": {
            "type": "keyword",
            "similarity": "boolean",
            "store": true
          }
        }
      },
      {
        "dt": {
          "match_pattern": "regex",
          "match": "^.*(_dt|_time|_at)$",
          "mapping": {
            "type": "date",
            "format": "yyyy-MM-dd HH:mm:ss||yyyy-MM-dd||yyyy-MM-dd_HH:mm:ss",
            "store": true
          }
        }
      },
      {
        "nested": {
          "match": "*_nst",
          "mapping": {
            "type": "nested"
          }
        }
      },
      {
        "object": {
          "match": "*_obj",
          "mapping": {
            "type": "object",
            "dynamic": "true"
          }
        }
      },
      {
        "string": {
          "match_pattern": "regex",
          "match": "^.*_(with_weight|list)$",
          "mapping": {
            "type": "text",
            "index": "false",
            "store": true
          }
        }
      },
      {
        "rank_feature": {
          "match": "*_fea",
          "mapping": {
            "type": "rank_feature"
          }
        }
      },
      {
        "rank_features": {
          "match": "*_feas",
          "mapping": {
            "type": "rank_features"
          }
        }
      },
      {
        "knn_vector": {
          "match": "*_512_vec",
          "mapping": {
            "type": "knn_vector",
            "index": true,
            "space_type": "cosinesimil",
            "dimension": 512
          }
        }
      },
      {
        "knn_vector": {
          "match": "*_768_vec",
          "mapping": {
            "type": "knn_vector",
            "index": true,
            "space_type": "cosinesimil",
            "dimension": 768
          }
        }
      },
      {
        "knn_vector": {
          "match": "*_1024_vec",
          "mapping": {
            "type": "knn_vector",
            "index": true,
            "space_type": "cosinesimil",
            "dimension": 1024
          }
        }
      },
      {
        "knn_vector": {
          "match": "*_1536_vec",
          "mapping": {
            "type": "knn_vector",
            "index": true,
            "space_type": "cosinesimil",
            "dimension": 1536
          }
        }
      },
      {
        "knn_vector": {
          "match": "*_2048_vec",
          "mapping": {
            "type": "knn_vector",
            "index": true,
            "space_type": "cosinesimil",
            "dimension": 2048
          }
        }
      },
      {
        "knn_vector": {
          "match": "*_4096_vec",
          "mapping": {
            "type": "knn_vector",
            "index": true,
            "space_type": "cosinesimil",
            "dimension": 4096
          }
        }
      },
      {
        "knn_vector": {
          "match": "*_6144_vec",
          "mapping": {
            "type": "knn_vector",
            "index": true,
            "space_type": "cosinesimil",
            "dimension": 6144
          }
        }
      },
      {
        "knn_vector": {
          "match": "*_8192_vec",
          "mapping": {
            "type": "knn_vector",
            "index": true,
            "space_type": "cosinesimil",
            "dimension": 8192
          }
        }
      },
      {
        "knn_vector": {
          "match": "*_10240_vec",
          "mapping": {
            "type": "knn_vector",
            "index": true,
            "space_type": "cosinesimil",
            "dimension": 10240
          }
        }
      },
      {
        "binary": {
          "match": "*_bin",
          "mapping": {
            "type": "binary"
          }
        }
      }
    ]
  }
}
```

## Detailed Analysis

### File Role in Repository

The file `conf/os_mapping.json` is located in the `conf` directory.

### Architecture Context

Files in this location typically handle concerns related to conf.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [infinity_mapping.json](infinity_mapping.json_docs.md)
- [llm_factories.json](llm_factories.json_docs.md)
- [mapping.json](mapping.json_docs.md)
- [private.pem](private.pem_docs.md)
- [public.pem](public.pem_docs.md)
- [service_conf.yaml](service_conf.yaml_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
