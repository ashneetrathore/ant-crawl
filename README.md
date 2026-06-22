# :ant: ANT CRAWL

## :open_book: OVERVIEW
Date: February 2025\
Developer(s): Ashneet Rathore, Nura Ahmed Nasir Abdalla

Ant Crawl is a web crawler that fetches pages from a Spacetime cache server containing 35,000+ links. It starts from seed URLs and systematically follows links - hence the name "Ant Crawl", inspired by how ants methodically explore a vast space. A custom scraper is used to extract and filter new URLs to visit. The crawler continues until all reachable pages are processed, adhering to common practices used in modern web crawlers to ensure efficient and responsible operation.

## :gear: HOW IT WORKS
The program follows a custom **frontier-worker architecture** implemented in **Python**, where the frontier serves as a queue of URLs to be visited and a worker fetches pages from it for processing.

Upon launch, the program receives a cache host and port from the Spacetime server and instantiates a crawler. When the crawler starts, a worker takes a link from the frontier, downloads it from the cache server, and passes a response containing page content to a scraper. The **scraper** extracts links from the page, adds valid new URLs to the frontier, and marks the downloaded URL as visited. This cycle continues until the frontier is empty.

The scraper applies two types of filtering. Links first undergo low-cost **URL-based filtering**, where invalid URLs, such as those with undesirable file extensions, are discarded before ever reaching the frontier. The program also includes a **trap detection** scheme that logs recurring URL patterns to identify infinite traps. Once a page is downloaded, **content-based filtering** checks for page size, duplicates, low information, etc.

The program additionally gathers and stores analytics on the crawled pages, including the count of unique URLs, the page with the most words, and the top 50 most frequent words. These analytics are written to output files created at runtime.

## :open_file_folder: PROJECT FILE STRUCTURE
```bash
ant-crawl/
│── launch.py         # Cache server connection and crawler entry point
│── scraper.py        # URL extraction and filtering logic
│── crawler/          
│   │── __init__.py   # Crawler class
│   │── frontier.py   # Frontier class
│   └── worker.py     # Worker class
│── packages/         # External dependencies
│── utils/            # Helper modules for the crawler
│── config.ini        # Config settings
│── README.md         # Project documentation
└── .gitignore        # Ignored files
```

## :hammer: CONFIGURATION
> [!WARNING]
> The original Spacetime cache server is no longer live. The crawler **cannot be executed** anymore. The following is for reference only.

**1. Clone the repository**
```bash
git clone https://github.com/ashneetrathore/ant-crawl.git
```

**2. Install dependencies**
```bash
cd ant-crawl
python -m pip install packages/spacetime-2.1.1-py3-none-any.whl
python -m pip install -r packages/requirements.txt
```

**3. Assign a value of the format `IR UW25 uci-id1,uci-2` to `USERAGENT` in `config.ini`**
```ini
# Example assignment w/h team members' UCI IDs
USERAGENT = IR UW25 12345678,87654321
```

## :rocket: EXECUTION
**Run the crawler**
```bash
python3 launch.py
```
**To restart the crawler from the seed URL**
```bash
python3 launch.py --restart
```