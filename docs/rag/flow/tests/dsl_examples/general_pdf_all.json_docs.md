# Documentation: rag/flow/tests/dsl_examples/general_pdf_all.json

## File Metadata

- **Path**: `rag/flow/tests/dsl_examples/general_pdf_all.json`
- **Size**: 3676 bytes
- **Type**: .json
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `rag/flow/tests/dsl_examples/general_pdf_all.json`.

## Original Source Code

```json
{
  "components": {
    "File": {
        "obj":{
            "component_name": "File",
            "params": {
            }
        },
        "downstream": ["Parser:0"],
        "upstream": []
    },
    "Parser:0": {
        "obj": {
            "component_name": "Parser",
            "params": {
              "setups": {
                "pdf": {
                  "parse_method": "deepdoc",
                  "vlm_name": "",
                  "lang": "Chinese",
                  "suffix": [
                    "pdf"
                  ],
                  "output_format": "json"
                },
                "spreadsheet": {
                  "suffix": [
                    "xls",
                    "xlsx",
                    "csv"
                  ],
                  "output_format": "html"
                },
                "word": {
                  "suffix": [
                    "doc",
                    "docx"
                  ],
                  "output_format": "json"
                },
                "slides": {
                    "parse_method": "presentation",
                    "suffix": [
                        "pptx"
                    ],
                    "output_format": "json"
                },
                "markdown": {
                  "suffix": [
                    "md",
                    "markdown"
                  ],
                  "output_format": "json"
                },
                "text": {
                  "suffix": ["txt"],
                  "output_format": "json"
                },
                "image": {
                  "parse_method": "vlm",
                  "llm_id":"glm-4.5v",
                  "lang": "Chinese",
                  "suffix": [
                    "jpg",
                    "jpeg",
                    "png",
                    "gif"
                  ],
                  "output_format": "text"
                },
                "audio": {
                  "suffix": [
                    "da",
                    "wave",
                    "wav",
                    "mp3",
                    "aac",
                    "flac",
                    "ogg",
                    "aiff",
                    "au",
                    "midi",
                    "wma",
                    "realaudio",
                    "vqf",
                    "oggvorbis",
                    "ape"
                  ],
                  "lang": "Chinese",
                  "llm_id": "SenseVoiceSmall",
                  "output_format": "json"
                },
                "email": {
                  "suffix": [
                    "msg"
                  ],
                  "fields": [
                    "from",
                    "to",
                    "cc",
                    "bcc",
                    "date",
                    "subject",
                    "body",
                    "attachments"
                  ],
                  "output_format": "json"
                }
              }
          }
        },
        "downstream": ["Splitter:0"],
        "upstream": ["Begin"]
    },
    "Splitter:0": {
        "obj": {
            "component_name": "Splitter",
            "params": {
              "chunk_token_size": 512,
              "delimiters": ["\n"],
              "overlapped_percent": 0
            }
        },
        "downstream": ["Tokenizer:0"],
        "upstream": ["Parser:0"]
    },
    "Tokenizer:0": {
        "obj": {
            "component_name": "Tokenizer",
            "params": {
            }
        },
        "downstream": [],
        "upstream": ["Chunker:0"]
    }
  },
  "path": []
}


```

## Detailed Analysis

### File Role in Repository

The file `rag/flow/tests/dsl_examples/general_pdf_all.json` is located in the `rag/flow/tests/dsl_examples` directory.

This file is part of the **RAG (Retrieval-Augmented Generation)** core engine.

### Architecture Context

Files in this location typically handle concerns related to dsl_examples.

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

- [hierarchical_merger.json](hierarchical_merger.json_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
