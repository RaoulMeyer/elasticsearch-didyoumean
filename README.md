# Elasticsearch did-you-mean example

This repository contains everything you need to try out the did-you-mean functionality provided by the Elasticsearch phrase suggester.

## Getting started

To run the example, you'll need to have [npm](https://www.npmjs.com/get-npm) and [docker](https://www.docker.com/get-started).

First install dependencies:

`npm install`

Then up the docker environment and import the mapping and some sample data:

`npm start`

This will open up the kibana dashboard, where you can then try out some queries.

## Running suggest queries 

After setting up the environment, you can run queries directly in Kibana. You can access the dev tools by going to [this url](http://localhost:5601/app/kibana#/dev_tools/console?_g=()).

For example, you can run the following query to get some suggestions:

```
GET library/book/_search
{
  "suggest": {
    "text": "prisoner of axaban",
    "phrase_suggester": {
      "phrase": {
        "field": "title.shingle",
        "confidence": 0.0,
        "direct_generator": [
          {
            "field": "title.shingle"
          }
        ]
      }
    }
  }
}
```

To try this out with a collate query, run the following query:

```
GET library/book/_search
{
  "suggest": {
    "text": "prisoner of axaban",
    "simple_phrase": {
      "phrase": {
        "field": "title.shingle",
        "confidence": 0.0,
        "direct_generator": [
          {
            "field": "title.shingle"
          }
        ],
        "collate": {
          "query": {
            "source": {
              "match": {
                "title": {
                  "query": "{{suggestion}}",
                  "fuzziness": "1",
                  "operator": "and"
                }
              }
            }
          },
          "prune": "true"
        }
      }
    }
  }
}
```
