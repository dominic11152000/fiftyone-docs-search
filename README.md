# fiftyone-docs-search
Search https://docs.voxel51.com with an LLM!

## Overview

This repo contains the code to enable semantic search on the 
[Voxel51 documentation](https://docs.voxel51.com) from Python or the command 
line. The search is powered by [FiftyOne](https://github.com/voxel51/fiftyone), 
OpenAI's [text-embedding-ada-002 model](https://platform.openai.com/docs/guides/embeddings), and [Qdrant vector search](https://qdrant.tech/).

## Installation

1. Clone the `fiftyone-docs-search` repo 

```
git clone https://github.com/voxel51/fiftyone-docs-search
```

2. Install [OpenAI's Python client](https://github.com/openai/openai-python) 

```
pip install openai
```

 and [register an API key](https://platform.openai.com/account/api-keys). Once you have your API key, set the `OPENAI_API_KEY` environment variable to it:

```
export OPENAI_API_KEY=<your key>
```

3. Install the [Qdrant Python client](https://github.com/qdrant/qdrant-client): 

```
pip install qdrant_client
```

and set up a Docker container with Qdrant running locally:

```
docker pull qdrant/qdrant
docker run -p 6333:6333 qdrant/qdrant
```

4. Install the `fiftyone-docs-search` package by `cd`ing into the repo and running:

```
pip install -e .
```

## Usage

### Command line

The `fiftyone-docs-search` package provides a command line interface for
searching the Voxel51 documentation. To use it, run:

```
fiftyone-docs-search query <query>
```

where `<query>` is the search query. For example:

```
fiftyone-docs-search query "how to load a dataset"
```

INSERT GIF HERE

The following flags can con give you control over search behavior:
- `--num_results`: the number of results returned
- `--open_url`: whether to open the top result in your browser
- `--score`: whether to return the score of each result
- `--doc_types`: the types of docs to search over (e.g., "tutorials", "api", "guides")
- `--block_types`: the types of blocks to search over (e.g., "code", "text")

You can also use the `--help` flag to see all available options:

```
fiftyone-docs-search --help
```

### Python

The `fiftyone-docs-search` package also provides a Python API for searching the
Voxel51 documentation. To use it, run:

```py
from fiftyone.docs_search import FiftyOneDocsSearch

fods = FiftyOneDocsSearch()
results = fods("how to load a dataset")
```

You can set defaults for the search behavior by passing arguments to the
constructor:

```py
fods = FiftyOneDocsSearch(
    num_results=5,
    open_url=True,
    score=True,
    doc_types=["tutorials", "api", "guides"],
    block_types=["code", "text"],
)
```

For any individual search, you can override these defaults by passing arguments.

## Versioning of the docs

The `fiftyone-docs-search` package is versioned to match the version of the
Voxel51 FiftyOne documentation that it is searching. For example, the `v0.20.1`
version of the `fiftyone-docs-search` package is designed to search the
`v0.20.1` version of the Voxel51 FiftyOne documentation.

By default, if you do not have a Qdrant collection instantiated yet, when you 
run a search, the `fiftyone-docs-search` package will automatically download
a JSON file containing a vector indexing of the latest version of the Voxel51
FiftyOne documentation.

If you would like, you can also build the index yourself from a local copy of
the Voxel51 FiftyOne documentation. To do so, first clone the FiftyOne repo if 
you haven't already:

```
git clone https://github.com/voxel51/fiftyone
```

and install FiftyOne, as described in the detailed installation instructions 
[here](https://github.com/voxel51/fiftyone#installation-1).

Build a local version of the docs by running:

```
bash docs/generate_docs.bash
```

Then run the following command to build the index:

```bash
fiftyone-docs-search create
```


## Contributing

We welcome contributions to this repo!


