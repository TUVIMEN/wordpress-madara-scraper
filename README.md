# wordpress-madara-scraper

A bash script for scraping image focused madara wordpress in json.

## Requirements

 - [hgrep](https://github.com/TUVIMEN/hgrep)
 - [jq](https://github.com/stedolan/jq)

## Installation
    
    install -m 755 wordpress-madara-scraper /usr/bin

## Json format

Here's example of [comics](comics-example.json).

## Tested sites

    https://manhwatop.com/
    https://www.nightcomic.com/
    https://shibamanga.com/
    https://topmanhua.com/

## Usage

    wordpress-madara-scraper [OPTIONS]...

Download links to comics into FILE

    wordpress-madara-scraper -p 'https://www.topmanhua.com' > FILE

Download comics from links in FILE using 4 threads into DIR, it will create json files named by md5 hash of their links

    wordpress-madara-scraper -d DIR -t 4 -c FILE

Download images links from chapters in comics FILE into FILES named by md5 hash of their links

    wordpress-madara-scraper -l FILE

Which can be united, creating files containing images links from chapters in directory named by comics file ended with '_'

    for i in $(find -maxdepth 1 -type f | grep -xE '\./[0-9a-f]{32}')
    do
        mkdir "${i}_";
        wordpress-madara-scraper -d "${i}_" -l <(jq -r '.chapters[].link' "$i");
    done

Or do everything above

    wordpress-madara-scraper -e 'https://www.topmanhua.com'

Get some help

    wordpress-madara-scraper -h
