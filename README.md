# CivitAI Downloader

A Python tool for downloading media (images and videos) and metadata from CivitAI collections and posts.

This is the result of substantially more troubleshooting than I would like to admit, and a long series of "educational experiences" along the path to get here. I provide this with no warrenty, no maintainance and no promise of help should CivitAI break it tomorrow. It worked for me, on my system, downloading several thousand images in one go, as well as a few videos.

Post functionality is entirely untested, but given the rest of my disclaimers. If I run into situation where this doesn't work, or someone else does, and posts a solution or pull request, I will approve it, but quite frankly, I used this to grab the stuff that I needed and may not have use for it again.

A large portion of the code was generated with Claude Sonnet 3.7, mostly that to do with actually interfacing with the API

## Overview

This application allows you to easily download content from the CivitAI platform, including:

- Images and videos from collections
- Images and videos from posts
- Associated metadata (prompts, models used, tags, etc.)
- Complete collection/post information

All content is organized into a structured directory hierarchy for easy browsing and management.

## Features

- **Collection and Post Support**: Download from individual CivitAI collections and posts, or process multiple IDs in a single run
- **Complete Metadata**: Automatically retrieves and saves generation prompts, models used, tags, and other details
- **Reliable Downloads**: Built-in retry logic for handling network errors and interruptions
- **Flexible Configuration**: Customizable download locations and behavior
- **Dry Run Mode**: Preview what would be downloaded without actually downloading files
- **Verbose Logging**: Detailed information about the download process when needed

## Installation

### Prerequisites

- Python 3.6 or higher
- pip (Python package installer)

### Setup

1. Clone this repository or download the source code:

```bash
git clone https://github.com/yourusername/civitai-downloader.git
cd civitai-downloader
```

2. Install the required dependencies:

```bash
pip install requests pyyaml
```

## Configuration

On first run, the application will prompt you for:

1. Your CivitAI API key (required for accessing the CivitAI API)
2. A download directory (defaults to `~/Pictures/CivitAI`)

These settings are saved to a configuration file at `~/.civitai_downloader/config.json` for future use.

### Getting a CivitAI API Key

To use this tool, you need a CivitAI API key:

1. Create an account or log in at [CivitAI](https://civitai.com/)
2. Navigate to your user settings and find the API section
3. Generate a new API key

## Usage

### Basic Usage

To download media from a collection:

```bash
python main.py --collection 12345
```

To download media from a post:

```bash
python main.py --post 67890
```

### Multiple IDs

You can download multiple collections or posts in a single command:

```bash
python main.py --collection 12345 23456 34567
```

### Command Line Options

```
usage: main.py [-h] (-c COLLECTION [COLLECTION ...] | -p POST [POST ...]) [-o OUTPUT] [-v] [--no-metadata] [--dry-run]

Download images, videos, and metadata from CivitAI collections and posts.

options:
  -h, --help            show this help message and exit
  -c COLLECTION [COLLECTION ...], --collection COLLECTION [COLLECTION ...]
                        Collection ID(s) to download. Can specify multiple IDs.
  -p POST [POST ...], --post POST [POST ...]
                        Post ID(s) to download. Can specify multiple IDs.
  -o OUTPUT, --output OUTPUT
                        Override default download location
  -v, --verbose         Enable verbose output
  --no-metadata         Skip metadata generation
  --dry-run             Show what would be downloaded without downloading
```

### Examples

Download a single collection with verbose logging:

```bash
python main.py --collection 12345 --verbose
```

Download a post to a custom directory:

```bash
python main.py --post 67890 --output ~/Downloads/CivitAI_Post
```

Preview what would be downloaded from multiple collections:

```bash
python main.py --collection 12345 23456 34567 --dry-run
```

Download without saving metadata:

```bash
python main.py --collection 12345 --no-metadata
```

## Output Structure

For a collection with ID 12345 and name "Example Collection":

```
~/Pictures/CivitAI/12345-Example_Collection/
├── collection_metadata.json
├── image1.jpg
├── image1_metadata.json
├── image2.png
├── image2_metadata.json
└── ...
```

For a post with ID 67890 and title "Example Post":

```
~/Pictures/CivitAI/67890-Example_Post/
├── post_metadata.json
├── image1.jpg
├── image1_metadata.json
├── image2.png
├── image2_metadata.json
└── ...
```

## Metadata Format

The tool saves detailed metadata for each downloaded item, including:

- Basic information (dimensions, MIME type, creation date, etc.)
- Generation data (prompts, negative prompts)
- Models used in creation
- Tags and other metadata

## Troubleshooting

### API Key Issues

If you encounter authorization errors, check that your API key is valid and properly saved in the configuration file.

### Download Failures

The tool has built-in retry logic, but persistent failures might indicate:
- Network connectivity issues
- CivitAI server problems
- Invalid or deleted content IDs

Try using the `--verbose` flag to get more detailed error information.

### Changing Configuration

You can manually edit the configuration file at `~/.civitai_downloader/config.json` or delete it to be prompted again on the next run.
