# wordpress-madara-scraper

A bash script for scraping image focused madara wordpress in json.

## Requirements

 - [hgrep](https://github.com/TUVIMEN/hgrep)
 - [jq](https://github.com/stedolan/jq)

## Installation
    
    install -m 755 wordpress-madara-scraper /usr/bin

## Json format

Here's example of [comics](comics-example.json).

## Structure

There are two sets of options that define what will be downloaded, and are divided into those that download metadata and those thad download images.

### Metadata

Is downloaded by `-p`, `-c`, `-l`, `--full-comic` and `--full-pages`. Files created by them are named by the md5 hash of their urls.

`-p` takes `LINK` argument and outputs a list of urls to comics. This might be used to get all of the comics from the website, category or an artist.

`-c` takes `FILE` argument from which it reads urls to comics and saves them in json files.

`-l` takes `FILE` argument from which it reads urls to chapters and saves the list of urls to their images to files.

`--full-comic` takes `LINK` argument and downloads comic and its chapters creating a directory for its chapters named with its name with '_' character at the end.

`--full-pages` takes `LINK` argument and downloads all comics from pages using `--full-comic`.

Example structure created by `--full-pages`:

    0001c692d6cadaa3c692412bc0ac51fe
    0001c692d6cadaa3c692412bc0ac51fe_/
        02c8e3f630d0cd48f13515f65a91fe3e
        0ba18e4d9db640693a8584b01983b451
        0df4a828f07137e21f585aa29375b223
    008216d512f75bcb86e2a08c4df7ae8c
    008216d512f75bcb86e2a08c4df7ae8c_/
        091bf018a3e41cb974c20be4901ba89a
        4e35d40ad644114a17e2995b30aa52fb

### Images

These options are meant for consumption purposes only, and are just a practical simplification of Metadata. Files created by them are named by their names with `/` character translated to `|`.

`--download-chapter` takes `LINK` as argument and downloads the images of the chapter
`--download-comic` takes `LINK` as argument and downloads the comic, its chapters and their images.
`--download-pages` takes `LINK` as argument and downloads all comics from pages using `--download-comic`

Example structure created by `--download-pages`:

    +99 Wooden stick manhwa
    +99 Wooden stick manhwa_/
        Chapter 1/
            ch_0_1.jpg
            ch_0_2.jpg
            ch_0_3.jpg
        Chapter 89.5/
            45.webp
            46.webp
    My School Life Pretending To Be a Worthless Person
    My School Life Pretending To Be a Worthless Person_/
        Chapter 1/
            ch_0_1.jpg
            ch_0_2.jpg
            ch_0_3.jpg
        Chapter 59/
            13.webp
            14.webp
            15.webp

## Tested sites

    https://manhwatop.com/
    https://www.nightcomic.com/
    https://shibamanga.com/
    https://topmanhua.com/

## Usage

    wordpress-madara-scraper [OPTIONS]...

Download the images of the chapter, comic, genre and the whole site

    wordpress-madara-scraper --download-chapter 'https://manhwatop.com/manga/love-hug/chapter-233/'
    wordpress-madara-scraper --download-comic 'https://manhwatop.com/manga/love-hug/'
    wordpress-madara-scraper --download-pages 'https://manhwatop.com/manga-genre/magical-genre/'
    wordpress-madara-scraper --download-pages 'https://manhwatop.com/'

Download the metadata of comic and the whole page

    wordpress-madara-scraper --full-comic 'https://nightcomic.com/manga/versatile-mage/'
    wordpress-madara-scraper --full-pages 'https://nightcomic.com/new/'

Download links to comics into FILE

    wordpress-madara-scraper -p 'https://www.topmanhua.com' > FILE

Download comics from links in FILE using 4 threads into DIR, it will create json files named by md5 hash of their links

    wordpress-madara-scraper -d DIR -t 4 -c FILE

Download images links from chapters in comics FILE into FILES named by md5 hash of their links

    wordpress-madara-scraper -l FILE

Get some help

    wordpress-madara-scraper -h
