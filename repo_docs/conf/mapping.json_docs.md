# File Documentation: conf/mapping.json

## File Metadata

- **Path**: `conf/mapping.json`
- **Extension**: `.json`
- **Lines**: 212
- **Characters**: 4,450
- **Size**: 4,450 bytes
- **Purpose**: Data/Configuration - JSON data or configuration file

## Original Source

```json
{
  "settings": {
    "index": {
      "number_of_shards": 2,
      "number_of_replicas": 0,
      "refresh_interval": "1000ms"
    },
    "similarity": {
      "scripted_sim": {
        "type": "scripted",
        "script": {
          "source": "double idf = Math.log(1+(field.docCount-term.docFreq+0.5)/(term.docFreq + 0.5))/Math.log(1+((field.docCount-0.5)/1.5)); return query.boost * idf * Math.min(doc.freq, 1);"
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
        "dense_vector": {
          "match": "*_512_vec",
          "mapping": {
            "type": "dense_vector",
            "index": true,
            "similarity": "cosine",
            "dims": 512
          }
        }
      },
      {
        "dense_vector": {
          "match": "*_768_vec",
          "mapping": {
            "type": "dense_vector",
            "index": true,
            "similarity": "cosine",
            "dims": 768
          }
        }
      },
      {
        "dense_vector": {
          "match": "*_1024_vec",
          "mapping": {
            "type": "dense_vector",
            "index": true,
            "similarity": "cosine",
            "dims": 1024
          }
        }
      },
      {
        "dense_vector": {
          "match": "*_1536_vec",
          "mapping": {
            "type": "dense_vector",
            "index": true,
            "similarity": "cosine",
            "dims": 1536
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

## High-Level Overview

This file is part of the RAGFlow repository located at `conf/mapping.json`.

Based on the file structure and naming, it appears to be a data/configuration - json data or configuration file.

The file contains approximately 212 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

This is a configuration or data file. See the 'Original Source' section for full content.

## Code Structure Analysis

- Total lines: 212
- Blank lines: 0 (0.0%)
- Comment lines: ~0 (0.0%)
- Code lines: ~212


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `conf` directory, specifically within `conf`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `conf/` directory
- Potential test file: `test_mapping.json`

## Keywords

Math

---
*Generated by RAGFlow Repository Documentation Generator*
